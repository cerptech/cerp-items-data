# Política de estabilidad del dataset

Qué puede dar por sentado quien construye sobre el Banco de ítems de
construcción CERP, y qué no. Este archivo viaja tal cual al bundle público.
Describe garantías que el pipeline ya hace cumplir hoy; no es una hoja de
ruta.

## 1. Identificadores

### 1.1 Forma

Todo ítem tiene un `id` que cumple:

```
^(wi|mt|mo|mq|au|ch|ot)-[a-z0-9][a-z0-9-]{1,78}$
```

El prefijo es el tipo del ítem y no cambia nunca:

| Prefijo | `kind` | Qué es |
|---|---|---|
| `wi` | `work_item` | Partida de obra (unidad de obra con descomposición) |
| `mt` | `material` | Material |
| `mo` | `labor` | Mano de obra |
| `mq` | `machinery` | Maquinaria |
| `au` | `auxiliary` | Precio auxiliar |
| `ch` | `chapter` | Capítulo (no se publica como ítem) |
| `ot` | `other` | Otro concepto simple |

Los ítems de países distintos de España llevan el código de país (ISO 3166-1
alfa-2, en minúsculas) intercalado tras el prefijo: `mo-ar-oficial-cct-76-75`,
`mt-ar-cemento-portland-cpc40`. Los ids españoles no llevan país porque el
banco nació España-only y un id acuñado no se rederiva (ver 1.2).

El resto del id es un slug legible derivado del resumen del ítem en el momento
de acuñarlo. **Es un identificador, no una descripción**: el resumen puede
corregirse después sin que el id cambie, y un id nunca se regenera a partir de
un resumen nuevo. No infiera nada del slug más allá del prefijo y, si lo hay,
el país.

### 1.2 Acuñación única

- Un id se acuña **una sola vez**, cuando el ítem entra al canon por una fuente
  semilla (BCCA, UOCRA) o por curación propia. Las demás fuentes solo se
  mapean a ids existentes (`data/mappings/<fuente>.json`); nunca acuñan.
- Un id acuñado **jamás se rederiva**: reimportar la misma fuente, cambiar el
  algoritmo de slug o corregir el resumen no produce un id nuevo para el
  mismo ítem. El vínculo `(fuente, código de origen) → id` es persistente y
  versionado en git.
- Una colisión al acuñar se resuelve con un sufijo determinista derivado de
  la fuente y el código de origen, nunca reasignando el id existente.

### 1.3 Nunca se renombra, nunca se reutiliza

- Un id publicado **no se renombra**. Si dos ítems resultan ser el mismo
  concepto, la aduana lo señala (`duplicate_candidate`) y la resolución es
  humana, pero ninguno de los dos ids cambia.
- Un id **no se reutiliza** para otro concepto. El significado de un id es
  estable para siempre: `wi-x` en la versión 0.3.0 y `wi-x` en la versión
  2.0.0 son la misma partida.
- El canon (`data/canonical/items/<id>.json`) no se edita a mano: toda
  corrección pasa por curación reproducible con procedencia, así que un
  cambio de contenido siempre tiene un rastro en git.

### 1.4 Ítems que dejan de publicarse

- Un ítem puede **desaparecer del catálogo publicado** sin que su id
  desaparezca del canon: la aduana pone en cuarentena (`quarantined`) los
  ítems con hallazgos críticos abiertos y `publish` los excluye. Cuando el
  hallazgo se resuelve, el ítem vuelve con el **mismo id**. Cada bundle
  informa cuántos quedaron fuera en `stats.quarantined`.
- Los hallazgos de la aduana (`data/aduana/findings/`) **nunca se borran**:
  los cerrados quedan como historial, así que la razón por la que un ítem
  salió del catálogo es auditable.
- **Hoy no existe una baja formal (tombstone)**: el modelo no tiene un campo
  `deprecated`/`supersededBy`, y ningún ítem ha sido retirado del canon
  hasta ahora. Si en el futuro se retira un ítem, la política es que su id
  no vuelve a aparecer con otro significado (1.3); introducir un marcador
  explícito de baja es un cambio de contrato y saldría como MAJOR (ver 2).
  Un consumidor que guarda ids debe tolerar que un id de una versión anterior
  falte en la siguiente sin tratarlo como error del dato.

## 2. Versionado del dataset

La versión del dataset (`data/VERSION`, replicada en `VERSION`, `index.json`,
`catalog.json` y cada capítulo del bundle) sigue **semver**:

| Incremento | Cuándo | Qué puede romper |
|---|---|---|
| **MAJOR** | Cambio de schema: se quita o renombra un campo, cambia el tipo de un campo, cambia la forma del id o se introduce una baja formal de ítems | Cualquier consumidor. Se anuncia en `CHANGELOG.md` con guía de migración |
| **MINOR** | Ítems, fuentes, países o taxonomías nuevas; campos **nuevos** opcionales; precios añadidos a ítems existentes | Nada, si el consumidor ignora los campos que no conoce |
| **PATCH** | Correcciones de datos (precio, unidad, descomposición, clasificación, texto) sin ítems nuevos ni cambios de forma | Nada estructural. Los valores sí pueden cambiar |

Compromisos que esto implica:

- **Campos nuevos se añaden, nunca se quitan en MINOR/PATCH.** Un consumidor
  escrito contra 0.5.0 sigue leyendo 0.9.0. Diseñe el parser para ignorar
  claves desconocidas.
- **Los campos ausentes tienen significado estable**: `country` ausente es
  `ES`; `unit` ausente es un concepto sin unidad de medida; `classification`
  ausente es un ítem sin clasificar.
- **Cada versión publicada es verificable**: `SHA256SUMS` certifica los bytes
  exactos de cada archivo del bundle, así que dos descargas de la misma
  versión son idénticas byte a byte. Los artefactos se serializan con claves
  ordenadas y orden estable de ítems: un diff entre dos versiones muestra
  solo cambios de datos (y la marca `generatedAt`).
- **Los precios de fuente son append-only con procedencia.** Todo precio
  lleva `source`, `edition`, `scope`, `date` y `kind`. Los precios de lista
  (`kind: "list"`) que publica una fuente se añaden por edición y nunca se
  sobrescriben: reimportar la misma edición no los duplica ni les cambia el
  valor, y una edición nueva añade entradas sin tocar las anteriores. Un
  precio de lista puede **retirarse** por curación cuando la propia fuente lo
  trae roto (`suppressListPrices`, con razón obligatoria), lo que se publica
  como PATCH y queda en el `CHANGELOG.md`.
- **Los precios propios de CERP (`source: "cerp"`) no son append-only.** Hay
  dos casos:
  - **Calculados (`kind: "computed_from_bom"`).** Se derivan de la
    descomposición y se recalculan en cada corrida del cálculo. Cada corrida
    reemplaza todas las entradas calculadas del ítem, con la `edition` del
    mes en que corrió, y las quita si la descomposición deja de poder
    costearse (por ejemplo, porque un hijo perdió su precio). Si necesita su
    historia, guárdela del lado del consumidor.
  - **Curados (`kind: "curated"`), dentro de su edición.** Si CERP releva
    otra vez el mismo mes, el valor nuevo reemplaza al anterior de esa
    edición; un mes nuevo añade una entrada. Lo mismo pasa con el costo
    empresa de la mano de obra (jornal × coeficiente de cargas sociales,
    `sourceCode` `COSTO-EMPRESA-<edición>`), que se recalcula en cada corrida
    para la edición del coeficiente.
- **No hay conversión de divisas** en ningún punto: EUR y ARS conviven sin
  relacionarse.

## 3. Lo que NO se garantiza

- **Que un precio siga vigente.** Cada precio lleva su fecha y su edición;
  la vigencia la decide el consumidor. La aduana marca como `stale_edition`
  los ítems cuyos precios superan 24 meses (España) o 3 meses (Argentina).
- **Que un precio calculado o curado conserve su valor entre versiones.**
  Solo los precios de lista son append-only; los `computed_from_bom` y los
  `curated` de una misma edición se reemplazan (ver 2).
- **Que el catálogo sea completo** para ningún capítulo ni país. Los
  conteos por tipo y por capítulo están en `index.json` y `tree.json`.
- **Que el resumen, la clasificación o la descomposición de un ítem no
  cambien.** Cambian por curación (PATCH) y el id se mantiene; use el id,
  no el resumen, como clave.
- **Que las URLs de `raw.githubusercontent.com` tengan SLA.** El bundle es
  estático y se puede espejar; el manifiesto `countries.json` y los links
  relativos de `llms.txt`/`README.md` funcionan servidos desde cualquier
  dominio.
- **Compatibilidad hacia atrás entre MAJOR.** Un cambio MAJOR se anuncia en
  `CHANGELOG.md` y mantiene el id de cada ítem (1.3), pero puede cambiar la
  forma de los archivos.

## 4. Cómo verificar una descarga

```sh
sha256sum -c SHA256SUMS      # desde la raíz del bundle; cubre ar/ y countries.json
```

Todos los archivos del bundle van en LF. El repo público lleva un
`.gitattributes` (`* -text`) para que git no los convierta al clonar, así la
verificación da el mismo resultado sobre una descarga directa y sobre un clon
(también en Windows con `core.autocrlf=true`).

`index.json` repite el hash por archivo en `checksums.files` (los del bundle
del país) y por artefacto en el campo `sha256` de `catalog`, `basicos`,
`searchIndex`, cada `chapters[]` y cada `taxonomies[]`; `checksums.manifest`
es la ruta relativa al `SHA256SUMS` de la raíz.

## 5. Dónde reportar

Problemas con los datos, ids que faltan o dudas sobre esta política:
<https://github.com/cerptech/cerp-items-data/issues> · admin@cerp.es.

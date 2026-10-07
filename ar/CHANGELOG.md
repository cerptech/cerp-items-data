# Changelog del dataset

Historial por versión del Banco de ítems de construcción CERP. La versión es la
de `data/VERSION` (semver: MAJOR = cambio de schema, MINOR = ítems o fuentes
nuevas, PATCH = correcciones). Este archivo viaja tal cual al bundle público
(`cli export-public` lo copia a la raíz y a cada país), así que se escribe para
quien consume los datos, no para quien mantiene el pipeline.

Las fechas son las del commit que publicó el bundle en `cerp-items-data`. Las
entradas anteriores a este archivo (0.1.0 a 0.5.0) están reconstruidas desde el
historial de git de `cerp-item-bank` y de `cerp-items-data` y desde los
informes `docs/CURACION-*.md`, `docs/CORRECCION-BOM-2026-08.md` y
`docs/AUDITORIA-CLASIFICACION-2026-08.md`.

Formato: [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/).

## [Unreleased]

## [0.6.0] - 2026-10-07

Lote argentino de octubre de 2026 y bundle público con checksums y licencia.
Publicado: 12.697 ítems ES + 666 AR (13.363 en total); 43 ítems ES en
cuarentena, excluidos.

### Añadido

- **Argentina** (lote 2026-10, plaza AMBA, precios netos sin IVA relevados el
  2026-10-06 y 07, mediana de varios comercios cuando hubo más de una fuente):
  el catálogo pasa de 284 a 666 ítems canónicos — 115 → 330 partidas,
  154 → 317 materiales, 10 → 14 ítems de maquinaria; la mano de obra sigue en
  5 ítems.
  - 215 partidas con análisis de precio unitario completo (materiales, mano
    de obra CCT 76/75 por zona A, B, C y C-Austral, y equipos). 211 quedan
    costeadas desde su descomposición; 4 contrapisos de arcilla expandida
    quedan sin precio porque el material no se vende para obra en Argentina,
    y lo dicen en su texto.
  - 163 materiales y 4 equipos nuevos, cada precio con vendedor, URL y fecha
    de captura. Nuevo insumo «Equipos y herramientas menores (por hora de
    uso)» a $1.100/h, con el cálculo de respaldo escrito en el propio ítem.
  - 20 materiales existentes suman un precio 2026-10 y conservan los
    anteriores (los precios nunca se pisan). Cuatro refrescos con un salto
    mayor al 40 % se retuvieron por probable diferencia de producto.
  - Los rendimientos provienen de un template de ítems de obra que remite al
    Manual del computista de la construcción; cada partida lo declara en su
    texto.
- El bundle público lleva `LICENSE` (el mismo texto de licencia y atribución
  que el README), `CHANGELOG.md` (este archivo), `STABILITY.md` (política de
  estabilidad de ids y versionado) y `SHA256SUMS` (hash SHA-256 de cada
  archivo del bundle, países incluidos, en formato `sha256sum`).
- `.gitattributes` en la raíz del bundle (`* -text`): todos los archivos van
  en LF y git no los convierte al clonar, así `sha256sum -c SHA256SUMS`
  también verifica un clon en Windows.
- `index.json` añade `sha256` por artefacto (`catalog`, `basicos`,
  `searchIndex`, cada `chapters[]` y cada `taxonomies[]`), el bloque
  `checksums` (`algorithm`, `files`, `manifest`) y `docs`. No se renombra ni
  se quita ningún campo existente.
- Sección «Contacto y soporte» en el README y en `llms.txt` del bundle:
  issues de `cerptech/cerp-items-data` y correo de contacto.

### Cambiado

- **Argentina**: las 10 máquinas que vivían en `materials/` de la curación
  pasan a `machinery/`. Los ids no cambian.

### Cambiado (pipeline)

- CI en `cerp-item-bank` (`typecheck`, `test`, `validate` en cada push y PR) y
  comando `cli validate`. Test de regresión del merge de hallazgos de la
  aduana (incidente `ed939f66`). No cambia los datos publicados.

## [0.5.0] - 2026-09-11

Curación propia de agosto y septiembre de 2026 en los dos países. Publicado:
12.697 ítems ES + 284 AR (12.981 en total); 43 ítems ES en cuarentena,
excluidos.

### Añadido

- **España** (rondas A y B, 2026-08-22, `data/curation/es/`): 132 ítems
  canónicos nuevos y 4 enriquecidos. La ronda A cierra los cinco huecos que
  dejó la investigación de fuentes (78 ítems); la ronda B puebla los capítulos
  anémicos (54 ítems). La curación ES pasa de 26 a 162 entradas. Sin fuentes
  externas nuevas y sin inventar precios: 17 partidas costeadas desde su
  descomposición y 51 quedan sin costear porque dependen de materiales sin
  precio de lista publicado.
- **Argentina** (rondas B a F, del 2026-08-22 al 2026-09-07, plaza AMBA,
  precios netos sin IVA, mediana de varios comercios): el catálogo pasa de
  18 a 284 ítems canónicos — 3 → 115 partidas, 10 → 164 materiales (154 `mt-`
  y 10 `mq-`), 0 → 10 ítems de maquinaria; de 18 a 25 rubros; las partidas
  completas (materiales + mano de obra) pasan de 40 a 80.
  - Se abren los capítulos de obra gruesa que existían en España y faltaban
    enteros en Argentina: demoliciones y trabajos previos, gestión de
    residuos, seguridad y salud, estructuras de acero y madera, losas
    alivianadas. Primer bloque de maquinaria del banco.
  - El hormigón elaborado tiene precio (escala completa H-8 a H-40, dos
    hormigoneras del GBA, sin IVA): desbloquea las ocho partidas de estructura
    que llevaban tres rondas paradas.
  - Rendimientos de mano de obra de dos fuentes de descarga libre: cátedra de
    Organización y Conducción de Obras de la FACET (UNT) y Manual Técnico
    Durlock Tomo 4, declaradas con sus limitaciones en el texto de cada ítem.
  - Lo que no tuvo fuente pública verificable quedó **sin precio** y con el
    motivo escrito en el propio ítem. No se cargaron rendimientos de
    instalaciones, cubierta de chapa, estructura metálica ni seguridad e
    higiene: ninguna fuente argentina de descarga libre los publica con horas
    por categoría.

### Corregido

- **AR**: la partida de armadura no tenía el hierro en su descomposición.
- **Aduana**: se restauran 6 hallazgos `duplicate_candidate` despachados a
  mano (`dismissed`) que la corrida del 2026-09-08 había reabierto porque la
  curación cambió la tilde de «diámetro» en los resúmenes (`ed939f66`).

### Cambiado

- Corrida completa de la aduana (2026-09-08) sobre 13.024 ítems canónicos:
  43 criticals, exactamente los mismos 43 de 0.4.0 (todos
  `negative_or_zero_price` heredados de la BCCA), ninguno sobre ítems `-ar-`
  ni sobre ítems nuevos. Los 132 ítems ES nuevos pasan de `unverified` a
  `auto_ok` o `conflicted`.
- Taxonomías `orden-ejecucion` y `tipo-obra` recalculadas sobre el canon
  ampliado.

## [0.4.0] - 2026-08-17

Precios corregidos y reclasificación de capítulos tras el incidente Cresata.
12.626 ítems canónicos comprobados por la aduana.

### Corregido

- **15 partidas ES con precios rotos por descomposición mal resuelta**,
  presentes tal cual en el BC3 de la BCCA 2024-01. Patrón A (12 partidas):
  el precio por m² de un material de piezas se consumía como precio por pieza
  multiplicado por las piezas que caben en 1 m²/1 m (alicatados de gres
  porcelánico y rústico, cenefas, pavimento de baldosa de acero). Patrón B
  (3 demoliciones de tabique): 1 h completa de camión/pala/peón por m².
  Corrección vía curación: 11 materiales corrigen unidad `u` → `m2` (8) o
  `u` → `m` (3) conservando el precio de la BCCA; las 15 partidas corrigen la
  línea de material (1,05 m²/m con mermas) o los rendimientos, y suprimen el
  precio de lista derivado del BOM roto (`suppressListPrices`), de modo que
  el precio vigente pasa a ser el computado. Ejemplos: alicatado gres
  porcelánico 25×25, 362,55 → 42,19 €/m²; demolición de tabique con medios
  manuales, 60,13 → 7,09 €/m². Antes/después completo en
  `docs/CORRECCION-BOM-2026-08.md`.
- **110 partidas reubicadas en `orden-ejecucion`**: el archivado de
  revestimientos dependía de palabras clave sobre el resumen y los nombres
  abreviados (PAV., SOL. G., PARQUE, TECHO LAMAS) caían al capítulo genérico.
  Ahora manda el subcapítulo oficial de la fuente (Suelos/Peldaños → 12-PAV,
  Techos → 14-TEC, Continuos → 11-REV, Aplacados/Ligeros → 13-RVV,
  trasdosados → 08-ALB). 0 mapeos borrados, 88 entradas `ai` intactas. Informe
  en `docs/AUDITORIA-CLASIFICACION-2026-08.md`.

### Añadido

- Regla de aduana `chapter_mismatch` (R9): partida archivada en una taxonomía
  en un capítulo distinto al esperado por la familia de su código de origen.
  El mapa familia → capítulo esperado es un dato versionado
  (`data/taxonomies/orden-ejecucion/expected-chapters.json`, 52 familias,
  5.427 de 6.465 partidas cubiertas). 38 hallazgos abiertos a decisión humana.
- Comando `detect-bom` (solo reporta descomposiciones rotas de los patrones A
  y B) y `suppressListPrices` en el schema de curación, con razón obligatoria.

## [0.3.0] - 2026-07-22

Primer bundle publicado con el mensaje `chore: publish bundle`. 12.565 ítems
ES + 18 AR.

### Añadido

- Taxonomía superpuesta **`tipo-obra`**: 5 nodos planos de códigos estables
  con labels es/en — `01-ONU` obra nueva (483), `02-REH` rehabilitación
  (793), `03-CIV` obra civil (715), `09-COM` común a todos los tipos (682),
  `99-SIN` sin clasificar (3.795); 6.468 partidas ES, 100 % mapeado. Es un
  eje de **priorización**, no de filtro: el consumidor sube los tramos del
  tipo de obra declarado y baja el resto, nunca esconde partidas.

### Cambiado

- Bump de versión obligatorio para que el eje llegue al ERP: el sync
  cortocircuita por `contentHash` y ese hash no incluye la taxonomía.

## [0.2.0] - 2026-07-21

Apertura del modelo canónico a multi-país (ES + AR). Publicado en
`cerp-items-data` el mismo día, sin mensaje `publish bundle`.

### Añadido

- `country` y `currency` pasan de constantes a datos del ítem; un catálogo
  por país (`catalog.json` = España en la raíz del bundle, `ar/` para
  Argentina) y `countries.json` como manifiesto. Los ids de países no
  españoles llevan el país en el namespace (`mo-ar-...`). No hay conversión
  de divisas en ningún punto: EUR y ARS conviven sin relacionarse.
- **Argentina**: mano de obra desde la escala salarial del CCT 76/75 (UOCRA)
  con costo empresa (jornal × coeficiente de cargas sociales, guardado como
  precio `curated` aparte del jornal de convenio), curación propia de
  materiales con precio y partidas con BOM (18 ítems en total).
- Capa de índices de precios (`dist/indices.json`) con adaptador INDEC. El
  canon nunca guarda precios derivados de índice.
- Precios computados desde la descomposición (`cli compute`) para partidas
  sin precio de lista.
- Taxonomía superpuesta **`orden-ejecucion`** v1: 34 capítulos según el
  procedimiento real de ejecución de obra, 6.468 partidas mapeadas.
- En el bundle: `tree.json` + `outlines/` para navegación jerárquica y
  `search-index.json` para búsqueda client-side.

## [0.1.0] - 2026-07-21

### Añadido

- Primera publicación: pipeline BC3 (FIEBDC-3) completo e ingesta de la
  **BCCA enero 2024** (Base de Costes de la Construcción de Andalucía, Junta
  de Andalucía), fuente semilla que acuña los ids canónicos. 12.565 ítems ES
  con descomposición, precios con procedencia y estado de verificación de la
  aduana.
- Bundle público agent-first: `llms.txt`, `README.md`, `index.json`,
  `basicos.json`, `catalog.json` y un JSON + Markdown por capítulo.

[Unreleased]: https://github.com/cerptech/cerp-items-data/compare/main...HEAD
[0.6.0]: https://github.com/cerptech/cerp-items-data/compare/5247c12...main
[0.5.0]: https://github.com/cerptech/cerp-items-data/commit/5247c12
[0.4.0]: https://github.com/cerptech/cerp-items-data/commit/77272a3
[0.3.0]: https://github.com/cerptech/cerp-items-data/commit/79d718b
[0.2.0]: https://github.com/cerptech/cerp-items-data/commit/82985b2
[0.1.0]: https://github.com/cerptech/cerp-items-data/commit/4520fe1

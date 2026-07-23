# Banco de ítems de construcción CERP

Catálogo abierto de **18 ítems de construcción de Argentina** con precios de referencia y descomposición completa (mano de obra, materiales y maquinaria con rendimientos). Versión **0.3.0** · generado 2026-07-22 · mantenido por [CERP](https://cerp.es).

3 partidas de obra · 10 materiales · 5 recursos de mano de obra y maquinaria

## Para personas

**Web con buscador y fichas por ítem: <https://cerp-items-web.vercel.app>**

También podés navegar los capítulos en [`chapters/`](./chapters): cada capítulo es una tabla con id, resumen, unidad, precio y descomposición.

| Capítulo | Ítems | |
|----------|------:|--|
| [Albañilería > Mampostería](./chapters/albanileria--mamposteria.md) | 2 | [JSON](./chapters/albanileria--mamposteria.json) |
| [Morteros > Mortero de cemento](./chapters/morteros--mortero-de-cemento.md) | 1 | [JSON](./chapters/morteros--mortero-de-cemento.json) |

## Para agentes y software

- **[llms.txt](./llms.txt)** — índice pensado para agentes
- **[index.json](./index.json)** — índice máquina (chunks, conteos, atribuciones)
- **[basicos.json](./basicos.json)** — conceptos simples para resolver los BOM (2 fetches alcanzan: capítulo + básicos)
- **[catalog.json](./catalog.json)** — todo el catálogo en un archivo

Los archivos se sirven con CORS abierto vía `raw.githubusercontent.com`. Estructura de cada ítem: ver `CatalogItem` en el schema del [repo del pipeline](https://github.com/cerptech/cerp-item-bank).

## Licencia y atribución

- Escala salarial del CCT 76/75 (UOCRA — CAMARCO/FAEC), según acuerdo homologado por la Secretaría de Trabajo
- Información obtenida del Portal de la Junta de Andalucía

Los precios de referencia provienen de bases públicas reutilizadas conforme a la Ley 37/2007 (RISP) con la atribución indicada. La estructura canónica, la curación y el pipeline son de CERP Tech.

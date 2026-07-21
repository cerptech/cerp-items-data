# Banco de ítems de construcción CERP

Catálogo abierto de **12565 ítems de construcción de España** con precios de referencia y descomposición completa (mano de obra, materiales y maquinaria con rendimientos). Versión **0.1.0** · generado 2026-07-21 · mantenido por [CERP](https://cerp.es).

6510 partidas de obra · 5639 materiales · 107 recursos de mano de obra y maquinaria

## Para personas

**Web con buscador y fichas por ítem: <https://cerp-items-web.vercel.app>**

También podés navegar los capítulos en [`chapters/`](./chapters): cada capítulo es una tabla con id, resumen, unidad, precio y descomposición.

| Capítulo | Ítems | |
|----------|------:|--|
| [Precios Auxiliares > AGLOMERANTES Y MORTEROS](./chapters/precios-auxiliares--aglomerantes-y-morteros.md) | 39 | [JSON](./chapters/precios-auxiliares--aglomerantes-y-morteros.json) |
| [Precios Auxiliares > RESIDUOS](./chapters/precios-auxiliares--residuos.md) | 3 | [JSON](./chapters/precios-auxiliares--residuos.json) |
| [Precios Unitarios > ACONDICIONAMIENTO DE TERRENOS](./chapters/precios-unitarios--acondicionamiento-de-terrenos.md) | 137 | [JSON](./chapters/precios-unitarios--acondicionamiento-de-terrenos.json) |
| [Precios Unitarios > AISLAMIENTOS](./chapters/precios-unitarios--aislamientos.md) | 147 | [JSON](./chapters/precios-unitarios--aislamientos.json) |
| [Precios Unitarios > ALBAÑILERÍA](./chapters/precios-unitarios--albanileria.md) | 322 | [JSON](./chapters/precios-unitarios--albanileria.json) |
| [Precios Unitarios > CARPINTERÍA Y ELEMENTOS DE SEGURIDAD Y PROTECCIÓN](./chapters/precios-unitarios--carpinteria-y-elementos-de-seguridad-y-proteccion.md) | 475 | [JSON](./chapters/precios-unitarios--carpinteria-y-elementos-de-seguridad-y-proteccion.json) |
| [Precios Unitarios > CIMENTACIONES](./chapters/precios-unitarios--cimentaciones.md) | 181 | [JSON](./chapters/precios-unitarios--cimentaciones.json) |
| [Precios Unitarios > CONTROL DE CALIDAD, PRUEBAS Y ENSAYOS](./chapters/precios-unitarios--control-de-calidad-pruebas-y-ensayos.md) | 335 | [JSON](./chapters/precios-unitarios--control-de-calidad-pruebas-y-ensayos.json) |
| [Precios Unitarios > CUBIERTAS](./chapters/precios-unitarios--cubiertas.md) | 120 | [JSON](./chapters/precios-unitarios--cubiertas.json) |
| [Precios Unitarios > DEMOLICIONES Y TRABAJOS PREVIOS](./chapters/precios-unitarios--demoliciones-y-trabajos-previos.md) | 490 | [JSON](./chapters/precios-unitarios--demoliciones-y-trabajos-previos.json) |
| [Precios Unitarios > EQUIPAMIENTO](./chapters/precios-unitarios--equipamiento.md) | 54 | [JSON](./chapters/precios-unitarios--equipamiento.json) |
| [Precios Unitarios > ESTRUCTURAS](./chapters/precios-unitarios--estructuras.md) | 187 | [JSON](./chapters/precios-unitarios--estructuras.json) |
| [Precios Unitarios > GESTIÓN DE RESIDUOS](./chapters/precios-unitarios--gestion-de-residuos.md) | 104 | [JSON](./chapters/precios-unitarios--gestion-de-residuos.json) |
| [Precios Unitarios > INSTALACIONES](./chapters/precios-unitarios--instalaciones.md) | 1989 | [JSON](./chapters/precios-unitarios--instalaciones.json) |
| [Precios Unitarios > MANTENIMIENTO](./chapters/precios-unitarios--mantenimiento.md) | 288 | [JSON](./chapters/precios-unitarios--mantenimiento.json) |
| [Precios Unitarios > PINTURAS](./chapters/precios-unitarios--pinturas.md) | 97 | [JSON](./chapters/precios-unitarios--pinturas.json) |
| [Precios Unitarios > REVESTIMIENTOS](./chapters/precios-unitarios--revestimientos.md) | 525 | [JSON](./chapters/precios-unitarios--revestimientos.json) |
| [Precios Unitarios > SANEAMIENTO](./chapters/precios-unitarios--saneamiento.md) | 68 | [JSON](./chapters/precios-unitarios--saneamiento.json) |
| [Precios Unitarios > SEGURIDAD Y SALUD](./chapters/precios-unitarios--seguridad-y-salud.md) | 243 | [JSON](./chapters/precios-unitarios--seguridad-y-salud.json) |
| [Precios Unitarios > URBANIZACIONES](./chapters/precios-unitarios--urbanizaciones.md) | 604 | [JSON](./chapters/precios-unitarios--urbanizaciones.json) |
| [Precios Unitarios > VIDRIERÍA Y ELABORADOS SINTÉTICOS](./chapters/precios-unitarios--vidrieria-y-elaborados-sinteticos.md) | 95 | [JSON](./chapters/precios-unitarios--vidrieria-y-elaborados-sinteticos.json) |
| [Sin clasificar](./chapters/sin-clasificar.md) | 7 | [JSON](./chapters/sin-clasificar.json) |

## Para agentes y software

- **[llms.txt](./llms.txt)** — índice pensado para agentes
- **[index.json](./index.json)** — índice máquina (chunks, conteos, atribuciones)
- **[basicos.json](./basicos.json)** — conceptos simples para resolver los BOM (2 fetches alcanzan: capítulo + básicos)
- **[catalog.json](./catalog.json)** — todo el catálogo en un archivo

Los archivos se sirven con CORS abierto vía `raw.githubusercontent.com`. Estructura de cada ítem: ver `CatalogItem` en el schema del [repo del pipeline](https://github.com/cerptech/cerp-item-bank).

## Licencia y atribución

- Información obtenida del Portal de la Junta de Andalucía

Los precios de referencia provienen de bases públicas reutilizadas conforme a la Ley 37/2007 (RISP) con la atribución indicada. La estructura canónica, la curación y el pipeline son de CERP Tech.

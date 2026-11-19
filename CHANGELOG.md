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

## [0.12.0] - 2026-11-19

Argentina deja de tener copias de Revista CIFRAS. Las partidas y los
materiales del banco toman el precio de la revista donde son el mismo
trabajo o el mismo producto, y las copias se borran.

### Qué cambia

- **Se borran las copias.** Salen las 236 partidas «Cifras NN.NN · …» de la
  0.10.0, los 470 materiales «Cifras · …» de la 0.11.0 y las dos hojas en
  pesos que usaban esas partidas. Ya no hay dos ítems para el mismo
  trabajo con dos precios distintos.
- **123 partidas propias cuestan lo que dice Cifras.** Son las que hacen el
  mismo trabajo que un rubro de la tabla «Costos unitarios por rubros» del
  n.º 368, en la misma unidad o con una conversión directa (por ejemplo,
  una puerta de 0,80 x 2,00 m vale 1,6 m² del rubro). Se multiplicaron las
  cantidades de su descomposición para que en zona A cuesten exactamente
  la ejecución y los materiales de la revista. En las otras zonas se
  aplican los mismos multiplicadores, y el costo que resulta depende de la
  mano de obra de cada zona. El texto de cada partida dice el rubro, los dos
  importes y por cuánto se multiplicó.
- **58 materiales propios tienen el precio de la lista de materiales** de la
  revista (n.º 368, relevado al 30-09-2026, sin IVA): cemento, ladrillos,
  hierros, hormigón elaborado, placas y perfiles de construcción en seco,
  cables y otros. Si la revista trae el producto en varios comercios, se
  tomó la mediana. El precio anterior queda en el historial del ítem.
- Tres materiales equivalentes se quedan con su precio propio, porque se
  relevó en noviembre y es más nuevo que el de la revista: cal hidratada
  en bolsa de 25 kg, cable unipolar de 2,5 mm² y vigueta pretensada por
  metro.
- Por el cambio de esos materiales, otras 127 partidas cambian de precio
  (mediana −0,6 %). 72 partidas no cambian.
- Con el precio de Cifras, las 123 partidas suben en mediana 17,6 %.
  Las que más suben son las estructuras de hierro (unas 5 veces), las
  demoliciones, los cercos de obra, las bocas eléctricas y las cañerías de
  agua y de gas, que tenían la descomposición incompleta o muy por debajo.
- Para costar lo de la revista se multiplicaron las cantidades: la
  descomposición de esas partidas ya no es un consumo real de obra (por
  ejemplo, el muro de ladrillo hueco de 12 lleva 26,7 ladrillos por m²).
  Sirve para el costo, no para pedir materiales.

### Qué hay que saber

- Las cantidades que menciona el texto de esas 123 partidas son las de su
  fuente original. Las de la descomposición son las ajustadas.
- La revista sigue citada como fuente de cada partida y material que toma
  su precio. Su nombre ya no aparece en el nombre de ningún ítem.
- No pasaron a precio de Cifras: los rubros de la revista sin una partida
  propia equivalente (151 de 236), los renglones de la lista sin un
  material propio equivalente (383 de 470), los materiales que la revista
  vende en un envase más chico que el del banco (el precio por litro o por
  kilo no es el mismo) y las partidas de hormigón armado que no incluyen el
  encofrado (Cifras sí lo incluye y se pagaría dos veces).
- Dos partidas equivalentes no se pudieron ajustar porque hoy tienen un
  material sin precio: «Demolición de contrapiso y piso» y «Piso de
  cerámico esmaltado 30x30 cm».

### Sin cambios

- Los ids de las partidas y materiales propios y todo el catálogo de
  España.

## [0.11.0] - 2026-11-18

Argentina suma la lista de precios de materiales de Revista CIFRAS, cargada
tal cual la publica la revista. Lo que ya estaba en el banco no cambia.

### Nuevo

- **470 materiales «Cifras · …»**, uno por cada renglón con precio de la lista
  «Precios de materiales a octubre 2026» de Revista CIFRAS n.º 368 (noviembre
  de 2026, Santa Fe), páginas 110 a 112. Están en el capítulo «Revista CIFRAS:
  precios de materiales», con las 41 secciones de la revista: aberturas,
  cementos, hierros, ladrillos, electricidad, pinturas, pisos, vidrios y las
  demás.
- El nombre de cada material es la descripción de la revista y dónde figura
  en la lista (sección, marca o comercio). Por ejemplo, «Cifras · Cemento Loma
  Negra x 25 kg · CEMENTOS › Spahn». El texto trae la descripción completa,
  la página y el importe impreso.
- Son precios de mostrador que la revista relevó al 30 de septiembre de 2026.

### Qué hay que saber antes de usarlos

- **El precio va sin IVA**, como todos los precios del banco. La revista
  publica la lista con IVA del 21% incluido: cada precio se dividió por 1,21
  y el importe impreso queda en el texto. La revista dice «sin IVA» en dos
  lugares, que se cargaron sin dividir: la sección de ascensores (4 precios)
  y la primera tabla de vidrios, que es para mayoristas (16).
- **La unidad es la de la revista.** 390 van «por unidad», que es como la
  revista publica la lista. En general la descripción dice la presentación
  (bolsa de 25 kg, barra de 12 m, balde de 20 litros), pero algunos renglones
  no la dicen, como los cables. Los otros 80 van por m², m³, kg o metro
  porque la revista los publica así.
- **Cada precio es el que la revista relevó para un comercio o una marca**,
  no una mediana de mercado, y no se ajusta por zona. La revista es de Santa
  Fe. El mismo producto puede figurar más de una vez, con el precio de cada
  comercio: son materiales distintos.
- La lista trae algunos renglones que no son materiales: servicios (bombeo
  de hormigón, ploteo, colocación, relevamiento y montaje) y equipos
  (herramientas, ascensores, portones automáticos). Se cargaron igual, como
  figuran.
- **No reemplazan a los materiales propios del banco** ni entran en ninguna
  partida. Las partidas propias siguen con sus materiales y sus precios.
- 10 renglones de la lista figuran «sin cotizar» en la revista y no se
  cargaron.
- 12 descripciones no entran en el nombre y van abreviadas con «…». El texto
  las trae completas.
- Dos importes impresos llaman la atención frente a sus vecinos de la lista,
  un flexible cromado y un bajo mesada de 40. Se cargaron como están y el
  texto de cada uno lo avisa.
- La sección de vidrios tiene dos tablas. La primera dice «precios para
  mayoristas (sin IVA) x m2» y se cargó sin dividir. La segunda, de vidrios
  decorativos (8 precios), dice «x m2» y nada del IVA: se dividió por 1,21,
  por la regla general de la lista, y el texto de cada uno avisa la duda.
- Dos renglones de la primera tabla de vidrios describen piezas con medidas
  propias, una puerta y un perfil. Su importe puede ser por pieza y no por
  m²: se cargaron por m², como dice la tabla, y el texto lo avisa.

### Sin cambios

- Las 236 partidas «Cifras NN.NN · …» de la 0.10.0, las 333 partidas propias
  de Argentina, sus materiales y todo el catálogo de España.

### Pendiente conocido

- La lista cargada es la del n.º 368. La revista publica una lista nueva cada
  mes y el banco no la toma sola: hay que cargarla.
- Sigue sin haber confirmación escrita del editor de la revista para este
  uso. El banco se apoya en la autorización general de su ejemplar.
- Esta versión se publica el mismo día que se cargó, sin la espera de 24
  horas y sin la aprobación de una persona: la revisaron revisores
  automáticos, independientes de quien la cargó. El cotejo contra el
  ejemplar (470 de 470 renglones, importe por importe) se hizo dos veces,
  con programas distintos.
- La verificación del banco marcó 273 pares de estos materiales como
  posibles duplicados, porque sus nombres se parecen: son medidas,
  presentaciones o comercios distintos de la misma lista. Se revisaron y se
  cerraron con su explicación; ninguno quedó abierto.
- Lo que las versiones 0.9.1, 0.9.2 y 0.10.0 dejaron pendiente sigue igual,
  porque esta versión solo agrega la lista de la revista.

## [0.10.0] - 2026-11-18

Argentina suma los costos por rubro de Revista CIFRAS, cargados tal cual los
publica la revista. Las partidas propias del banco no cambian.

### Nuevo

- **236 partidas «Cifras NN.NN · …»**, una por cada rubro de la tabla «Costos
  unitarios por rubros» de Revista CIFRAS n.º 368 (noviembre de 2026, Santa
  Fe, región Litoral). Están en el capítulo «Revista CIFRAS: costos por
  rubro», con los 20 capítulos de la revista, y su nombre lleva el código del
  rubro (por ejemplo, «Cifras 04.08 · Ladrillos cerámicos huecos 12x18x33cm»).
- Cada partida trae los dos importes que publica la revista, sin modificar:
  ejecución (mano de obra y equipos) y materiales. Son costos directos, sin
  IVA, gastos generales ni beneficio. Su suma es el costo unitario del rubro.
- **Fuente nueva: Revista CIFRAS**, citada. Su ejemplar autoriza publicar sus
  datos citando la fuente; la atribución viaja en el catálogo y cada partida
  nombra el rubro y la edición de los que sale.
- Dos hojas nuevas que valen un peso por unidad, «Mano de obra y equipos
  según Revista CIFRAS (en pesos)» y «Materiales según Revista CIFRAS (en
  pesos)». Son las líneas con las que cada partida carga sus dos importes.

### Qué hay que saber antes de usarlas

- **No tienen análisis.** La revista publica importes, no horas ni consumos:
  estas partidas sirven para presupuestar por rubro, no para computar
  materiales ni planificar mano de obra. Para eso están las partidas propias
  del banco.
- **No se ajustan por zona.** Los valores son de la región Litoral.
- **En varios trabajos ahora hay dos partidas**: la propia del banco, con su
  análisis, y la de la revista. No valen lo mismo: sobre 36 trabajos
  equivalentes, la mediana de la propia queda un 19,5% por debajo.
- La revista declara qué incluye cada sección en instalaciones, movimiento
  de tierra, estructuras, mamposterías y tabiquerías; el texto de cada
  partida lo dice. Donde no lo declara, el texto también lo dice.
- 13 rubros no tienen materiales en la revista (demoliciones, excavaciones,
  alquiler de volquete, ayuda de gremio): su partida lleva solo ejecución.

### Sin cambios

- Las 333 partidas propias de Argentina, sus 339 hojas y todo el catálogo de
  España.

### Pendiente conocido

- Calibrar las partidas propias del banco contra la revista queda para otra
  versión: hoy conviven las dos series.
- La edición cargada es la de noviembre de 2026. La revista publica una tabla
  nueva cada mes y el banco no la toma sola: hay que cargarla.
- No hay confirmación escrita del editor de la revista para este uso. El
  banco se apoya en la autorización general de su ejemplar.
- Esta versión se publicó el mismo día que se cargó, sin la espera de 24
  horas y sin la aprobación de una persona distinta de quien la cargó. El
  cotejo contra la planilla original (236 de 236 rubros, importe por
  importe) lo hizo un programa aparte.
- Lo que la 0.9.1 y la 0.9.2 dejaron para la 0.10.0 sigue pendiente y pasa a
  la 0.11.0, porque esta versión solo agrega la tabla de la revista: las
  quince partidas propias fuera del rango de mercado (once de ellas con el
  aviso «LÍMITE CONOCIDO» en su texto), las ocho subas de más del 100% sin
  firma, las 180 partidas sin fuente declarada y los once precios relevados
  el 18-11 que esperan decisión.

## [0.9.2] - 2026-11-18

Revisión de la base argentina contra sus fuentes: 124 partidas pasan a tener
sus horas de mano de obra tomadas de una fuente declarada, y 81 de ellas
también sus consumos. Los ids no cambian y no se relevó ningún precio.

Los porcentajes son de la zona A salvo que se diga otra cosa. En las zonas C
y C austral la hora de mano de obra vale más: en general, allí suben más las
partidas que ganan horas y bajan más las que las pierden, salvo en algunas
donde también cambian los materiales, como la columna de 30x30 (−15% en C
austral) o el revoque fino a la cal predosificado, que en C austral sube un
punto menos.

### Corregido

- **Argentina — 124 partidas rehechas sobre su fuente.** En 81, las horas y
  los consumos quedan tomados de la fuente; en 43, solo las horas. Tomar el
  valor de la fuente no siempre lo cambia: en 42 de las 81 las horas quedan
  iguales, y en 10 de las 124 no cambia nada de la descomposición, solo el
  texto. En las 43, los materiales quedan como estaban, salvo en cuatro
  losas de viguetas, que suman la madera de apuntalamiento de la fuente (y
  la de 9 cm, además, vigueta). Las fuentes son el análisis de precios del
  Colegio de Arquitectos de Salta (edición octubre 2026, 72 partidas), los
  «Coeficientes de Consumo» de la FACET-UNT (36) y, donde ninguna fuente
  argentina cubre el trabajo, la BCCA (16). En zona A, 69 suben, 42 bajan y
  10 no cambian de precio; las otras tres no tenían precio (el zócalo
  cerámico, que ahora lo tiene, y dos contrapisos con arcilla expandida, que
  siguen sin él). Fuera de las 124, otras tres mamposterías suben menos de
  0,5% por arrastre: usan el mortero de cemento 1:6, que pasa de arena fina
  a arena gruesa.
- **Argentina — hormigón armado.** «Columna de hormigón armado», «Viga
  resistente de hormigón armado», «Losa maciza de hormigón armado»,
  «Encadenado horizontal de hormigón armado» y «Viga de fundación de
  hormigón armado» tenían cargadas como horas las cantidades de madera del
  ítem que citaban y no llevaban el hierro ni el encofrado de su
  descripción. Quedan iguales a su fuente: su precio se multiplica por entre
  3,4 y 5,2 en zona A y por hasta 6,4 en C austral. «Base de hormigón
  armado» suma el hierro que le faltaba (+51% en zona A, +33% en C austral).
  Las estructuras por sección (columnas de 25x25, 30x30 y 40x40; vigas de
  20x50 y 30x90) toman las horas y los consumos del mismo análisis: las
  columnas de 25x25 y de 30x30 bajan 28% y 17%, y la viga de 30x90 sube 20%
  en zona A y 35% en C austral.
- **Argentina — hormigón preparado en obra.** Esas once partidas y otras
  cinco (las bases céntricas sin encofrado, +36%; la gradería, +36% en zona
  A y +54% en C austral; las vigas de fundación del capítulo de estructuras;
  la losa maciza de 10 cm y el contrapiso armado con malla Q92) dejan el
  hormigón elaborado de planta y pasan a hormigón preparado en obra, con su
  cemento, su arena y su ripio: las horas de la fuente son de hormigón hecho
  en obra.
- **Argentina — losas de viguetas con ladrillo cerámico** de 9 y de 12,5 cm
  y de viguetas dobles: horas del análisis (2,50 h de oficial y 3,10 h de
  ayudante por m²) y madera de apuntalamiento, que no tenían. Suben entre
  54% y 91% en zona A y entre 62% y 109% en C austral. Las de 9 y de 12,5 cm
  pasan además de 2 a 2,10 m de vigueta por m², y la de 12,5 cm toma el
  hormigón y la malla de la capa de compresión de la fuente. Las losas con
  bloque de EPS suman la misma madera (+14% en zona A, +8% en C austral).
- **Argentina — mampostería de ladrillo hueco de 12x18x33**: horas y
  consumos del análisis (0,65 h de oficial y 0,70 h de ayudante, 17
  ladrillos por m²). Sube 25% en zona A y 31% en C. Las de 8x18x33 y
  18x18x33 abren su mortero en cemento, cal y arena, como la fuente, y la de
  8x18x33 suma 0,3 kg de hierro por m².
- **Argentina — cielorraso de machimbre suspendido bajo losa**: tenía 0,25 h
  de oficial especializado y 0,25 h de ayudante por m², sin fuente; pasa a
  1,60 h de oficial y 1,30 h de ayudante, las de la FACET-UNT. Sube 132% en
  zona A y 203% en C austral.
- **Argentina — revoques, carpetas, contrapisos, pisos, revestimientos,
  cubiertas y estructura metálica**: horas y, donde la fuente los cubre,
  consumos del ítem que cada partida cita, con la cuenta de cada adaptación
  en el texto. Las que más suben en zona A: el revoque fino a la cal
  predosificado de 3 mm (+88%), el zócalo de piedra natural de 10 cm de
  altura (+46%), las dos carpetas para clavar pisos de madera (+36% y +37%),
  el revestimiento de piedra natural de 1,2 cm (+36%) y el revoque fino
  exterior a la cal de 3 mm (+35%).
- **Argentina — partidas que bajan más de 15%.** En zona A: el revoque
  grueso exterior reforzado (−35%), la pared simple divisoria de locales
  húmedos con placa resistente a la humedad (−30%), el revestimiento de
  pared existente sobre perfil omega (−28%) y el piso de cemento alisado
  (−20%), además de las columnas de 25x25 y de 30x30. En las zonas C y C
  austral las dos de placa de yeso bajan entre 36% y 40%, y otras seis,
  entre 15% y 22%: las estructuras de hierro semipesada y pesada, los
  zócalos en piso de mármol o granito, el revoque grueso de vermiculita, el
  contrapiso de hormigón de cascotes de 20 cm y la mampostería de ladrillos
  comunes en elevación de 0,15 a 0,30 m.
- **Argentina — seis partidas que no tenían mano de obra ahora la tienen**:
  canaleta de chapa galvanizada, cartel de obra, pileta de patio, tablero
  seccional, columna de caño estructural y correa de perfil C. Tenían un
  precio único para todo el país y pasan a tener precio por zona: suben
  entre 11% y 67% en zona A, y la canaleta, 133% en C austral.
- **Argentina — zócalo cerámico de 10 cm**: pasa a tener precio. Se computa
  con el cerámico de piso, como lo trae la fuente.
- Cada partida corregida declara la fuente, el ítem, las adaptaciones, qué
  incluye y qué no.

### Pendiente conocido

- Las correcciones las propusieron, verificaron y cotejaron a ciegas agentes
  de corridas distintas. Ninguna persona abrió las fuentes.
- Ocho partidas suben más del 100% en alguna zona (las cinco de hormigón
  armado, el cielorraso de machimbre suspendido y, en las zonas C y C
  austral, la canaleta de chapa galvanizada y la losa de viguetas de 9 cm):
  falta la firma de quien cura el país.
- Quince partidas corregidas quedan fuera del rango de las referencias de
  mercado consultadas; se revisan en la 0.10.0. Once quedan muy por debajo y
  lo avisan en su texto («LÍMITE CONOCIDO»): las tres mamposterías de
  ladrillo hueco, armadura por kilo, cañería de polipropileno de 25 mm,
  pileta de patio, zócalo cerámico, esmalte sobre herrería, contrapiso
  alivianado, excavación de bases y replanteo. En unas, la fuente da menos
  horas o menos material que esas referencias; en otras, el rubro de la
  referencia tiene otro alcance; en otras pesa el precio de agosto de sus
  materiales. Las otras cuatro quedan más cerca: el revoque fino a la cal
  predosificado y la pintura al látex para exteriores, por debajo; el
  contrapiso de hormigón de cascote sobre losa de 8 cm y el cielorraso de
  machimbre suspendido, por encima. De estas cuatro, solo la pintura lo
  avisa en su texto.
- 29 correcciones verificadas contra su fuente no se aplicaron, porque
  alejan la partida de las referencias de mercado o bajan su costo sin
  respaldo. Cinco de esas partidas citan una fuente y sus números no
  coinciden con los de esa fuente: «Revoque interior completo a la cal,
  grueso y fino» y «Cielorraso aplicado bajo losa», que cargan unas cinco
  veces la cal de su fuente, «Cielorraso suspendido de placa de roca de
  yeso», «Cubierta de teja colonial cerámica sobre losa» y «Piso flotante
  laminado AC4 de 8 mm». Quedan para revisión de una persona con experiencia
  de obra.
- 180 de las 333 partidas argentinas siguen sin una fuente declarada para
  sus horas y consumos, sin contar dos que no necesitan rendimiento (el
  sereno por mes y la bomba de achique por día). En 111, ninguna de las tres
  fuentes cubre el trabajo; en 45, la corrección propuesta no alcanzó la
  certeza pedida o la verificación la rechazó (37 se apoyaban solo en la
  BCCA); las otras 24 están entre las correcciones retenidas. Entre ellas
  están los cielorrasos desmontables, las bocas eléctricas, dos de las tres
  plateas y buena parte de las instalaciones.
- En 39 de las 43 partidas que tomaron solo las horas los materiales no
  cambian: los que no tenían fuente declarada siguen sin tenerla. Las otras
  cuatro son las losas de viguetas que suman la madera de apuntalamiento; la
  de viguetas dobles conserva sus 3,20 m de vigueta por m², sin fuente. En
  varias de las 43, además, las horas salen de una tarea parecida y no de la
  misma: las losas con bloque de EPS toman las de la losa con bovedilla
  cerámica; los contrapisos de cascote, las del contrapiso de hormigón
  simple o armado; el revoque de vermiculita, las del revoque rústico. Fuera
  de las 43, la carpeta alisada con malla de fibra toma las horas y los
  consumos de la carpeta niveladora. Cada texto lo dice.
- Hormigón de planta: ninguna fuente permitida da las horas de estas
  estructuras con hormigón elaborado, así que base, columna, encadenado,
  losa maciza y vigas quedan solo con hormigón preparado en obra. Otras
  partidas del catálogo siguen con hormigón elaborado (las de hormigón a la
  vista, las bases con encofrado, las plateas y los tabiques, entre otras).
- Las columnas de 25x25, 30x30 y 40x40 quedan con la misma descomposición
  por m³, salvo la línea de equipos menores, y las vigas de 20x50 y 30x90
  también: la fuente no distingue secciones. El encofrado real por m³ cambia
  con la sección.
- Las fuentes de rendimientos no declaran su metodología y la de la
  FACET-UNT no tiene fecha. La BCCA es práctica española.
- Precios: 84 materiales argentinos siguen con precio de agosto de 2026.
  Once se relevaron de nuevo el 18-11-2026 y esperan la decisión de quien
  cura el país antes de entrar (entre ellos, el ladrillo hueco de 12x18x33 y
  el portante, que dan más bajo que en agosto). La arcilla expandida sigue
  sin precio y deja cuatro contrapisos sin costo.
- Quedan pares de partidas parecidas con fuentes distintas (por ejemplo
  «Columna de hormigón armado», con encofrado, y «Hormigón armado para
  columnas elaborado en obra», sin encofrado). Los textos dicen la
  diferencia.
- Dos partidas conservan en sus fuentes una referencia a la BCCA que ya no
  usan: la mampostería de ladrillo hueco de 12x18x33 y la cañería de agua de
  polipropileno de 25 mm.
- Siguen vigentes los pendientes de la 0.9.1 sobre pinturas y porcelanato y
  sobre las losas con bloque de EPS (la malla sigue sin fuente declarada y
  su consumo de vigueta es una inferencia), y las salvedades de la 0.9.0
  sobre los precios de la ventana con DVH y de la puerta de cedro. También
  el cotejo por una persona de las fichas de comercio de la 0.9.0 (esa
  versión lo anunció para la siguiente y sigue sin hacerse) y de las fichas
  y el análisis de precios de la 0.9.1: vence en la 0.10.0. Las partidas
  argentinas de ejecución sin mano de obra bajan de 18 a 12; ese pendiente
  vence el 14-01-2027.

## [0.9.1] - 2026-11-17

Corrección de consumos de material en cinco partidas argentinas, cada uno con
su fuente declarada. Los ids no cambian, no se relevó ningún precio y las
horas de mano de obra son las mismas.

### Corregido

- **Argentina — pintura al látex acrílico para exteriores, tres manos**: el
  látex pasa de 0,30 a 0,333 l/m², el rendimiento con el trabajo terminado
  que da la ficha del fabricante del producto cuya cotización fija el precio
  del material. La partida sube 1,8%.
- **Argentina — pintura al látex en cielorrasos y pintura al látex interior
  con enduido previo**: el enduido pasa de 0,45 y 0,50 a 0,56 kg/m², según la
  ficha del fabricante del producto del precio; es consistente con el consumo
  del análisis que da las horas. Suben 1,4% y 0,9%.
- **Argentina — losas de viguetas y bloques de EPS de 12,5 y de 16,5 cm, luz
  3,50 m**: la vigueta pasa de 2 a 2,10 m por m², el consumo del mismo
  análisis que ya da las horas. Suben 0,4%.
- Las cinco partidas declaran ahora qué incluyen y qué no, qué consumos no
  tienen fuente declarada y lo que está en revisión.

### Pendiente conocido

- Los consumos los cotejaron agentes, a ciegas, contra las fichas de los
  fabricantes y el análisis de precios; ninguna persona abrió las fuentes.
  Queda pendiente ese cotejo, igual que el de las partidas nuevas de la
  0.9.0.
- Pintura para exteriores: el fijador con el que se relevó el precio es un
  sellador al agua para interiores. Falta decidir si la partida lleva un
  fijador para exteriores o ninguno. Sus horas siguen calculadas con el
  consumo de pintura anterior (0,30 l/m²).
- Pintura de cielorrasos: no lleva fijador. El análisis que da las horas
  lleva 0,12 l/m² de fijador al aguarrás y la ficha del enduido pide volver a
  aplicar sellador o fijador sobre lo enduido. Falta decidir cuál lleva.
- Pinturas interiores: el consumo de látex (0,24 l/m²) no tiene fuente
  declarada, aunque queda dentro del rango de la ficha de su fabricante. El
  fijador de la pintura interior (0,10 l/m²) tampoco la tiene.
- Pinturas: los precios de los materiales son de agosto de 2026. Contra una
  de las referencias de mercado consultadas las tres partidas quedan por
  debajo, y la diferencia está en los materiales.
- Losas con bloque de EPS: la malla (1,1 m² de 4,2 mm) no tiene fuente
  declarada. No incluyen apuntalamiento, encofrado de borde ni hierros de
  negativos. Que el consumo de vigueta del análisis valga para el bloque de
  EPS es una inferencia: la fuente no declara la separación entre viguetas.
- Las losas de viguetas con ladrillo cerámico de 9 y de 12,5 cm y la de
  viguetas dobles siguen con horas y consumo de vigueta que no declaran
  fuente.
- Siguen vigentes los pendientes de la 0.9.0: el porcelanato queda por debajo
  de las referencias de mercado consultadas y 18 partidas argentinas de
  ejecución no tienen mano de obra cargada (se completan antes del
  14-01-2027).

## [0.9.0] - 2026-11-16

Segunda tanda de la base argentina: dos partidas de gama media que faltaban,
con su material, y un precio re-relevado. Los ids existentes no cambian y los
precios relevados anteriores se conservan.

### Añadido

- **Argentina — partidas**: «Colocación de puerta placa de cedro de 80x200
  cm» (u) y «Colocación de ventana DVH de aluminio Módena 150x110 cm» (u).
  Usan los mismos rendimientos de la fuente que sus variantes ya publicadas
  (puerta placa de 70x200 cm y ventana de vidrio simple).
- **Argentina — materiales**: «Puerta placa de cedro de 80x200 cm con marco de
  chapa» (mediana de 4 comercios) y «Ventana de aluminio línea Módena de
  150x110 cm con DVH» (mediana de 3 comercios). Precio neto sin IVA, relevado
  el 16-11-2026, una ficha por comercio.

### Corregido

- **Argentina — ventana de aluminio Módena de 150x110 cm con vidrio simple**:
  precio re-relevado en tres comercios (el anterior era de agosto). El
  material sube 7,3% y la partida de colocación 5,5%. El texto decía
  «anodizado natural, con mosquitero»: las fichas son de aluminio blanco y sin
  mosquitero.
- **Argentina — carpeta bajo piso cerámico de 1,8 cm**: solo texto. Aclara
  que es de cal reforzada y nombra las carpetas de cemento del banco
  («Carpeta de nivelación de 3 cm» y «Carpeta hidrófuga de 2 cm»).

### Pendiente conocido

- Los precios de las dos partidas nuevas los relevaron y cotejaron agentes;
  ninguna persona abrió las fichas de los comercios. Deuda con vencimiento en
  la próxima versión.
- Ventana con DVH: los tres comercios difieren 1,8 veces y la ficha que fija
  la mediana incluye contramarcos. Puerta de cedro: no todas las fichas
  declaran cerradura y bisagras, y el marco no es idéntico en las cuatro.
  Ninguno de los dos materiales tiene una ficha de Córdoba.
- El piso y el revestimiento de porcelanato siguen por debajo de las
  referencias de mercado consultadas. El porcelanato rectificado de primera
  relevado en comercios no explica la diferencia y no hay una fuente
  permitida que respalde un precio mayor.
- 18 partidas argentinas de ejecución no tienen mano de obra cargada (aviso
  nuevo de la verificación). Se completan antes del 14-01-2027.

## [0.8.0] - 2026-11-14

Corrección de la base argentina: rendimientos, alcance y materiales de 36
partidas corregidos por curación, y 21 partidas más que cambian solo por
arrastre de sus insumos (entre −2,9% y +18,3%). Los ids no cambian y los
precios relevados anteriores se conservan.

### Añadido

- **Argentina — partidas**: «Cielorraso aplicado de yeso bajo losa» (m2).
- **Argentina — materiales**: adhesivo cementicio para porcelanato en bolsa de
  25 kg.

### Corregido

- **Argentina — hormigón armado elaborado en obra** (bases, columnas, vigas,
  dinteles, encadenados y escaleras): cada partida tenía las horas y los
  consumos de la fila vecina de su fuente. Con los valores propios, bases baja
  64% y escaleras sube 55%; las otras cuatro se mueven entre −5% y +6%.
- **Argentina — errores de lectura de la fuente**: piso de cemento alisado
  (0,60 h de oficial y 0,30 h de ayudante por m2; baja 54%), cubierta de teja
  colonial (−12%), piso de mosaico granítico (−17%), piso flotante (−16%),
  cielorraso de machimbre (−15%) y colocación de ventana de aluminio de
  150x110 cm con vidrio simple (2,5 h + 2,5 h y material de amure; −7%).
- **Argentina — pinturas**: las tres partidas de látex «tres manos» tenían
  horas que, por el consumo de pintura de la fuente, equivalen a una o una y
  media manos; se escalaron por el consumo de pintura de la partida (inferencia
  y criterio de CERP, declarados en cada ítem). Suben
  entre 39% y 69%. El esmalte sintético sobre herrería sube 53%.
- **Argentina — losas de viguetas y bloques de EPS** (12,5 y 16,5 cm): mano de
  obra de 2,5 h de oficial y 3,1 h de ayudante por m2 (antes 0,5 y 1,4 en la
  de 12,5 cm, y 0,6 y 1,5 en la de 16,5 cm).
  Suben 89% y 77%. No incluyen apuntalamiento como partida aparte.
- **Argentina — otras partidas**: mampostería de ladrillo común de 12 cm
  (+14%), cubierta de chapa galvanizada (+15%), enlucido de yeso (+74%),
  picado de revoque (+67%), armaduras de hierro (+13%), vigas y dinteles de
  perfil IPN/UPN (+18% a +23%), colocación de inodoro y de lavatorio (+9%),
  retiro de piso cerámico (+9%). «Limpieza final de obra» ahora tiene
  descomposición y precio.
- **Argentina — colocación de puerta placa de 70x200 cm**: el material pasa a
  ser el kit con marco, bisagras y cerradura (mediana de comercios, nov-2026).
  Sube 15%. No incluye manija ni picaporte.
- **Argentina — materiales re-relevados en noviembre de 2026** (precio neto
  sin IVA, una ficha por comercio): cal hidratada, vigueta pretensada,
  porcelanato 60x60 (mediana de tres comercios), caño de PVC de 110 mm,
  cable de 2,5 mm² y yeso. El piso y el revestimiento de porcelanato bajan
  11%.
- El texto de un material citaba una publicación que no es fuente del banco;
  se quitó la cita.

### Pendiente conocido

- Carpeta bajo piso, cielorraso a la cal, revestimiento de porcelanato,
  puerta placa y ventana de vidrio simple siguen por debajo de las
  referencias de mercado consultadas; su composición se revisa en la próxima
  versión. La variante de ventana con doble vidriado hermético no se publica
  hasta tener un precio relevado de esa medida.

## [0.7.0] - 2026-11-06

### Añadido

- **Argentina — mano de obra**: escala salarial UOCRA (CCT 76/75) del tramo de
  noviembre de 2026 (acuerdo UOCRA–CAMARCO–FAEC del 21/09/2026, +1,9% sep,
  +1,8% oct, +1,7% nov), 20 valores nuevos (5 categorías × 4 zonas) en los
  ítems `mo-ar-*`. Transcripción por cruce de fuentes secundarias, pendiente de
  cotejo contra el Boletín Oficial (`cross-check-secundario`).
- **Argentina — índices**: series internas del Indicador CAMARCO del costo de
  la construcción (general, materiales y mano de obra, ene-2025 a sep-2026)
  para contraste y actualización. No se redistribuyen en el bundle público
  (fuente `on-request` sin licencia de reutilización).

### Añadido (precios indexados)

- Nuevo tipo de precio **`indexed`**: precio relevado actualizado por una serie
  de índices, con su derivación completa (serie, meses, factor y precio base).
  Lo escribe el paso `index-prices` solo en hojas de entrada; las partidas se
  recalculan desde sus insumos. Semántica de reemplazo: a lo sumo uno por
  ítem/zona/serie, siempre re-derivable. Dos reglas nuevas de la aduana lo
  custodian (`indexed_integrity`, `indexed_basis_too_old`). Los valores crudos
  de la serie nunca se redistribuyen; con la política legal vigente de CAMARCO
  los precios indexados tampoco salen aún al bundle público.

### Cambiado

- **Argentina — cargas sociales**: coeficiente de costo empresa actualizado de
  2,1548 (feb-2020) a **2,1350** (Trabajo Nº 185 de CAMARCO, vigencia
  01/07/2026). El costo empresa por hora de los ítems `mo-ar-*` se recalculó
  con el coeficiente nuevo sobre el jornal de noviembre de 2026.
- **Argentina — partidas**: 317 partidas recalculadas desde su descomposición
  con la mano de obra actualizada (los materiales conservan su último
  relevamiento; su actualización por índice CAC está en decisión).

## [0.6.1] - 2026-10-07

### Corregido

- **Argentina**: el texto de los 382 ítems del lote 2026-10 y las notas de
  procedencia de 52 materiales ya no nombran el origen de los rendimientos ni
  la referencia de contraste de precio. Los precios, las descomposiciones y
  los ids no cambian.

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
[0.6.1]: https://github.com/cerptech/cerp-items-data/compare/19c056c...main
[0.6.0]: https://github.com/cerptech/cerp-items-data/compare/5247c12...19c056c
[0.5.0]: https://github.com/cerptech/cerp-items-data/commit/5247c12
[0.4.0]: https://github.com/cerptech/cerp-items-data/commit/77272a3
[0.3.0]: https://github.com/cerptech/cerp-items-data/commit/79d718b
[0.2.0]: https://github.com/cerptech/cerp-items-data/commit/82985b2
[0.1.0]: https://github.com/cerptech/cerp-items-data/commit/4520fe1

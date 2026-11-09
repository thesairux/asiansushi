# Asian Sushi Ovalle: página web

Página de pedidos de **Asian Sushi Ovalle** (Sergio Peralta 758, Ovalle · +56 9 5883 6237 · @asiansushiovalle),
hecha con la plantilla de páginas de pedidos de altiroweb.cl: portada con promos destacadas, carta con buscador
y fotos, «Arma tu roll», gohan y sushiburger con sus ingredientes, y envío del pedido ordenado por WhatsApp, con
retiro o delivery.

La carta es la del menú en PDF del local (14 páginas): tablas, tablas especiales y veggie, entrantes, creación de la
casa, rolls premium, gohan y sushiburger, arma tu roll (con arroz, sin arroz y hosomaki) y extras. Las fotos salen
del mismo PDF, recortadas y mejoradas.

El pedido va **solo por WhatsApp**: no hay cuentas, correo ni pago online. El total es referencial: el local
confirma disponibilidad, delivery y total al responder.

La página funciona sin servidor (también abriendo `index.html` directo en el navegador). El único servidor es el
del panel `/admin`, para guardar los cambios.

## Publicar

1. En Vercel: *Add New… → Project* → importa este repositorio → **Deploy**.
2. Activa el panel (ver abajo): `GITHUB_TOKEN` y `ADMIN_PASSWORD` en Vercel, y **Redeploy**.

Antes de publicar, revisa en `carta.js` → `negocio` el WhatsApp, la dirección, el mapa, las redes y los datos de
transferencia. El logo (`img/logo-asian.webp`), `favicon.ico`, `img/icono-192.png` e `img/icono-180.png` salen del
logo redondo del local.

## Pendiente de confirmar con el local

- **Formas de pago**: el menú no las dice. Quedaron Efectivo y Transferencia, al retirar y al recibir
  (`negocio.pagosRetiro` y `negocio.pagosDelivery`). Si acepta tarjeta o Edenred, se agregan ahí.
- **Delivery**: cargado del afiche «Delivery Ovalle y alrededores»: Ovalle $2.500, extremo norte $3.000 y extremo
  sur $4.000. Se cambian en el panel, en «Local y WhatsApp».
- **Horario**: lunes a jueves 13:30-22:30, viernes y sábado 14:30-23:30, domingo 12:30-21:30. Está confirmado
  (`"horarioConfirmado": true`): fuera de horario los productos dicen «Cerrado» y no se pueden agregar. Arriba no sale aviso de abierto o cerrado.
  Con `false` el horario solo se muestra como referencia.
- **Modificaciones en las tablas**: el menú dice «máximo 2 modificaciones, cada una con costo adicional», sin el
  valor. Está como nota en cada grupo de tablas; el cliente las escribe en la nota del producto.
- **Salsas incluidas en las tablas**: dice «soya o teriyaki» (3, 4 o 5 salsas según la tabla); el cliente lo
  anota. En los rolls, nigiris, sashimi, gohan y arma tu roll (1 salsa) el cliente elige soya o teriyaki.
- **Ingrediente extra y rebosar ($990 cada uno)**: no van sueltos en «Extras»; se eligen dentro de cada producto.
  «Ingrediente extra» (hasta 3, de los ingredientes secundarios) está en los rolls premium, el gohan, la sushi
  burger y los tres «Arma tu roll». «Rebosar el ingrediente base» (tempura o panko) está en los tres «Arma tu
  roll»; el gohan y la sushi burger ya traen «rebosado o frío» sin costo. Confirmar si el ingrediente extra
  también puede ser una proteína (grupos `ingrediente-extra` y `rebosar` en `personalizacion`).
- **Fotos**: el hosomaki y las salsas van sin foto (el hosomaki muestra el logo hasta tener una foto del local).
  Las cuatro «balls» usan la misma foto.
- Datos para transferencia y Facebook (se agregan en el panel).

## Dirección y pruebas

**Dirección**: con delivery, el cliente elige su sector y escribe su dirección a mano (calle, número, villa o
población y depto). No hay GPS, mapa ni sugerencias automáticas: la dirección llega tal cual en el mensaje de WhatsApp.

**Pruebas** (necesitan Playwright): `node pruebas/servidor.js` levanta la página y el panel con GitHub simulado
(«Publicar» no guarda nada). Con eso corriendo, `node pruebas/pedido.js` arma pedidos de retiro y delivery y revisa
el enlace y el mensaje de WhatsApp de cada sucursal, y `node pruebas/panel.js` revisa Stock, Promos y Publicar.

## Asistente (chat de abajo a la derecha)

Abajo a la derecha aparece el **asistente**: un botón con el logo y, la primera vez, un saludo («¡Hola! 👋 ¿Te ayudo a
elegir tu sushi?»). Al tocarlo se abre un chat que contesta sobre la carta, precios, horario, delivery, formas de pago y
dirección, recomienda tablas según cuántas personas son y muestra los productos con su botón **Agregar** (respeta el
stock, los días y el horario igual que la carta). No toma pedidos: el cliente igual envía su pedido por WhatsApp.

Opciones del chat (botones bajo cada respuesta, y el botón ☰ junto al cuadro de texto para verlas todas):
- **Ayúdame a pedir**: en 3 pasos (¿para cuántos?, ¿qué les gusta?, recomendación) y después sumar entrantes,
  bebidas y salsas.
- **Ver la carta** por sección, **Lo más pedido**, **Arma tu roll** (con sus ingredientes), **Veggie**, **Algo
  económico** (con tope de precio), **Entrantes**, **Bebidas** y **Salsas**.
- **Mi pedido**: lo que lleva el cliente con el total y el botón para abrirlo y enviarlo.
- **Delivery**, **Horario**, **Formas de pago**, **Ubicación**, **Cupones**, **¿Cómo pido?**, **¿Cuánto demora?**,
  **Hablar con alguien** (WhatsApp) y **Empezar de nuevo**.
- Escribiendo también entiende: cambios de ingredientes, picante, boleta o factura, reservas, alergias, reclamos,
  «somos 4», «algo de 10 lucas» y la búsqueda por producto o ingrediente.

Todos esos botones responden con la carta, sin gastar IA. Con IA, además sugiere respuestas según la conversación.

- **Con IA** (recomendado): en Vercel → *Settings → Environment Variables* agrega `ANTHROPIC_API_KEY` con una clave
  de la API de Claude (console.anthropic.com → *API Keys*) y haz **Redeploy**. El asistente lee `carta.js` en cada
  publicación, así que lo que cambies en el panel (precios, stock, promos, horario) lo sabe solo. Usa
  `claude-haiku-5-5` (rápido y económico); se cambia con `ASISTENTE_MODELO`. Cada persona puede mandar hasta 25
  mensajes cada 10 minutos. Para revisar, abre **tu-pagina.vercel.app/api/asistente**: dice si la clave está puesta.
- **Sin clave** (o abriendo `index.html` directo): el chat igual funciona en *modo básico*: responde con la carta
  (horario, delivery, pagos, dirección, recomendaciones, piezas por persona, «algo barato», búsqueda por ingrediente)
  y lo que no sabe lo manda al WhatsApp.

Archivos: `js/asistente.js` (el chat y el modo básico), `api/asistente.js` (el servidor de la IA; la clave nunca llega
al navegador) y los estilos al final de `css/estilos.css`. En el panel `/admin` el chat no aparece.

## Panel de administración

Entra a **tu-pagina.vercel.app/admin**. El usuario es `asiansushi` (se cambia con `ADMIN_USER`) y la contraseña es la
que pongas en `ADMIN_PASSWORD`. El panel no aparece en ningún menú de la página ni en Google.

Ahí ves la página tal cual, y arriba tienes:
- **Stock**: un interruptor por producto y por ingrediente (salmón, camarón, palta…). Lo apagas y en la página sale
  **Agotado** y no se puede pedir; lo prendes cuando llega. Arriba dice qué está agotado y hay un buscador.
- **Cupones**: crear, cambiar, pausar o borrar códigos de descuento (porcentaje o monto, pedido mínimo, fechas, días,
  retiro o delivery, y si se muestran en la página). El cliente escribe el código en «Mi pedido», en **¿Tienes un
  cupón?**, debajo de «¿Cómo pagas?». El campo está siempre; si el código no existe o está pausado, no se aplica.
- **Promos**: como «Agregar servicio» en el panel de SanFelect. Cada promo tiene nombre, precio, qué trae, nota,
  foto, los días que se puede pedir (sin marcar, todos) y si sale en la portada. **Agregar promo** suma una nueva;
  con las flechas se ordenan y con el tacho se borran. También se pueden crear grupos de promos.
- **Todas las opciones**: lo anterior más **Carta y precios** (los demás productos: agregar, quitar, precios y
  fotos), **Local y WhatsApp** (WhatsApp, dirección, datos para transferencia y sectores de delivery) y **Horario**.
  Abajo, en *Más opciones*, queda **Opciones para elegir** (proteínas, rellenos, agregados).

**Lo estético no se cambia desde el panel**: los colores, los textos de cada parte de la página (portada, cómo
pedir, ubicación…) y el nombre del local. El panel no los muestra, la página no deja escribir encima y, aunque
alguien mande otros al servidor, `api/admin.js` guarda siempre los que ya estaban publicados (`tema`, `textos` y
`negocio.nombre`). Esos se cambian solo editando `carta.js` en el repositorio.

Además:
- Los botones amarillos **Editar** de cada parte, o el **+** de cada producto, abren lo suyo (el **+** de una promo
  abre Promos). Lo que se usa poco de cada producto (variantes, lo que elige el cliente) está en *Más opciones*.
- **Ver como cliente** muestra la página sin marcas de edición; **Celular** la muestra en tamaño de teléfono.
- Al tocar **Publicar**, los cambios se guardan en el repositorio y Vercel actualiza la página sola
  en uno o dos minutos.

### Configuración (una sola vez, en Vercel)

En el proyecto de Vercel: **Settings → Environment Variables**

| Nombre | Para qué sirve |
| --- | --- |
| `GITHUB_TOKEN` | Obligatoria. Clave de GitHub (*fine-grained token*) con permiso *Contents: Read and write* solo sobre este repositorio. Solo la usa el servidor; nunca llega al navegador. |
| `ADMIN_PASSWORD` | Obligatoria. Contraseña del panel. No hay contraseña de fábrica. |
| `ADMIN_USER` | Opcional. Usuario del panel (si no está: `asiansushi`). |
| `GITHUB_REPO` | Opcional. `usuario/repositorio`, si Vercel no lo detecta solo. |
| `ANTHROPIC_API_KEY` | Opcional. Clave de la API de Claude para que el asistente responda con IA. Sin ella responde en modo básico. |
| `GITHUB_BRANCH` | Opcional. Rama donde guarda el panel (si no está: la rama que Vercel publicó, o `main`). |

Después de agregar o cambiar una variable, hay que volver a publicar (**Deployments → Redeploy**).
Para revisar la configuración, abre **tu-pagina.vercel.app/api/admin**: dice si la clave de GitHub y la contraseña
están puestas (nunca las muestra).

## Cómo está armada la carta (`carta.js`)

```
secciones            → botones grandes de la carta: Promos, Sushi, Hamburguesas…
  categorias         → si una sección tiene más de una, aparecen como botones chicos (Bebidas, Jugos…)
    productos        → cada producto con su precio
personalizacion      → grupos que el cliente elige: punto de la carne, agregados, proteína de cada roll…
```

La sección con `"promos": true` (o con «promo» en el nombre) es la que el panel muestra en **Promos**.

**Productos**
- **Precio**: `"precio": 14500`, sin punto ni signo.
- **Variantes con otro precio** (simple/doble, mediana/grande): `"opciones": ["Simple", "Doble"]` y
  `"preciosOpciones": { "Simple": 6490, "Doble": 8490 }`. Sin `preciosOpciones`, todas cuestan lo mismo (sabores).
- **Qué incluye** (combos y promos): en `"detalle"`, una cosa por línea separada con `\n`:
  `"1 hamburguesa a elección\n1 papas fritas\n1 bebida en lata"`. Si todas las líneas son piezas o cortes
  (`"25 piezas frías\n25 piezas calientes"`), además suma las piezas.
- **Nota destacada**: `"nota": "Válido por tiempo limitado."`
- **Agotado**: `"disponible": false`.
- **Promo en la portada**: `"destacado": true` y una `"foto"`.
- **Fotos**: en `img/carta/`, con `"foto": "img/carta/mi-foto.webp"`. Si una categoría tiene al menos un producto
  con foto, se muestra en tarjetas; si no, en lista. Se ven mejor en formato horizontal (1000 × 725).
  Con `"tarjetas": true` en la categoría, va en tarjetas aunque no tenga fotos: queda el espacio de la imagen
  con el logo hasta que se suba la foto.
  Al agregar o cambiar una foto en `img/carta/`, corre `python3 herramientas/miniaturas.py`: crea la copia liviana
  de 600 px (`img/carta/600/`) que usan las tarjetas para cargar rápido. Si falta, la tarjeta usa la foto original.
- **Producto nuevo**: copia uno parecido y cambia `id` (no se puede repetir), `nombre`, `detalle` y `precio`.

**Opciones para elegir** (`personalizacion`)
- `"tipo": "elegir"`, `"min": 1`, `"max": 1` → el cliente elige una sí o sí (punto de la carne, salsa).
  Con `"porDefecto": true` en una opción, queda marcada de entrada.
- `"tipo": "extra"`, `"max": 10` → agregados opcionales, cada uno con su `"precio"` (tocino +$1.000).
- `"suma": { "gohan-agregados": 1 }` en una opción → al elegirla, el cliente elige uno más en ese otro grupo
  (la proteína «Vegetariano (2 agregados)» del gohan, el sushiburger, la sushi arepa y el sushipleto).
- `"porRoll": true` → se elige en cada roll; el producto dice cuántos rolls arma el cliente con `"rolls": 2`.
  Con `"gratis": 1` en una opción, la primera no se cobra y cada repetida sí (envoltura en palta o en queso).
- Para usar un grupo: `"personalizar": ["punto", "agregados"]` en la categoría (vale para todos sus productos)
  o en un producto (reemplaza al de la categoría; por ejemplo el pollo crispy solo lleva `["agregados"]`).
- La sección **Arma tu roll** de la página aparece sola si hay una categoría con grupos `porRoll`;
  si el local no vende sushi, desaparece junto con su enlace del menú.

**Cupones** (`cupones`)
- Cada cupón tiene un `"codigo"` (lo que escribe el cliente), un `"nombre"` (cómo se muestra), un `"tipo"`
  (`"porcentaje"` o `"monto"`) y un `"valor"` (`10` = 10%, o `2000` = $2.000).
- Condiciones opcionales: `"minimo"` (pedido mínimo), `"tope"` (descuento máximo de un porcentaje),
  `"desde"` y `"hasta"` (`"2026-12-31"`), `"dias"` (`["martes"]`), `"modo"` (`"retiro"` o `"delivery"`) y
  `"aplica"` (ids de categorías, por ejemplo `["hamburguesas"]`: el descuento se calcula solo sobre esos productos).
- `"activo": false` lo apaga sin borrarlo; `"visible": false` lo saca de la página, pero el código sigue funcionando
  (para redes, volantes o clientes frecuentes).
- Un cupón por pedido. Si el pedido no cumple una condición, el cliente ve por qué (por ejemplo «te faltan $2.300») y el
  descuento se aplica solo cuando la cumple. El mensaje de WhatsApp lleva el cupón, el descuento y el total.
- La página no tiene servidor de pedidos: los códigos se pueden ver en `carta.js` y no se puede limitar a «un uso por persona».
  Por eso cada cupón llega escrito en el pedido, para que el local lo confirme al responder.
- Si no hay cupones activos y visibles, la sección **Cupones** y su enlace del menú no aparecen.

**Local, horario y delivery**
- **Sucursales**: en `negocio`, `"sucursales": [{ "nombre": "Villa El Portal", "whatsapp": "56946324426",
  "whatsappVisible": "+56 9 4632 4426", "direccion": "…" }]`. Con dos o más, el cliente elige la sucursal al
  pedir y el pedido llega al WhatsApp de esa sucursal; en la portada y el pie aparecen todos los números.
  Se editan en el panel, en «Local y WhatsApp».
- **Formas de pago**: `"pagosDelivery": ["Efectivo", "Transferencia", "Edenred"]` (al recibir) y
  `"pagosRetiro": ["Efectivo", "Transferencia", "Tarjeta", "Edenred"]` (al retirar).
- **Delivery por sectores**: `"sectoresDelivery": [{ "nombre": "Ovalle", "detalle": "…", "precio": 2500 }, …]`. Al
  pedir delivery el cliente elige su sector; el valor se suma al total y va en el mensaje.
- **Productos de un día**: `"dias": ["lunes"]` en un producto: solo se puede pedir ese día (los otros días dice
  «Solo los lunes») y en la portada aparece solo ese día. `"unidad": "Sushiburger"` cambia «Roll 1, Roll 2…» por
  «Sushiburger 1, 2…» en los productos que se arman de a varios.
- **Horario**: `"lunes": "12:30-23:00"`, `"domingo": "cerrado"` o dos turnos `"12:00-15:30, 19:00-23:00"`.
  Con `"horarioConfirmado": false` se muestra como *horario referencial, por confirmar* y no bloquea pedidos;
  con `true` aparece *Abierto ahora / Cerrado ahora* y fuera de horario no se puede agregar.
- **Días sin atención** (feriados): `"diasCerrados": ["2026-12-25", "2027-01-01"]`.
- Lo que quede vacío no se muestra: sin `"direccion"` no aparece la sección de ubicación; sin `"instagram"`, no hay enlace.
- **Mapa**: `"mapa"` es el enlace del local en Google Maps y `"mapaBusqueda"` lo que se busca para mostrar el mapa.
- **Transferencia**: `"datosTransferencia": "Banco …\nCuenta …\nRUT …"` se muestra cuando el cliente elige ese pago.

Al editar a mano, cuida las comillas y las comas: cada elemento de una lista va separado por una coma.
Si la carta deja de aparecer, lo más probable es que falte o sobre una coma en el último cambio.

## Archivos

- `index.html`: estructura de la página y textos fijos (portada, arma tu roll, cómo pedir, ubicación).
- `carta.js`: datos del local, colores, horario, productos, precios y opciones para elegir.
- `css/estilos.css`: diseño (computador y celular). Los colores se cambian desde `carta.js` → `tema`.
- `js/app.js`: carta, buscador, pedido, horario de apertura y mensaje de WhatsApp.
- `js/tema.js`: aplica los colores de la marca.
- `img/`: logo e íconos; en `img/carta/` las imágenes de los productos y en `img/carta/600/` sus copias livianas.
- `herramientas/miniaturas.py`: crea esas copias livianas.
- `admin.html`: el panel de administración.
- `api/admin.js`: el servidor del panel (revisa la contraseña y guarda los cambios en GitHub).
- `js/asistente.js` y `api/asistente.js`: el asistente (chat) y su servidor de IA.
- `pruebas/`: el servidor de prueba y las pruebas del pedido y del panel.
- `fotos/`: las fotos que se suben desde el panel (se crea sola).
- `vercel.json` y `robots.txt`: configuración para que el panel no aparezca en Google.

## Cómo funciona el pedido

El cliente arma el pedido, escribe su nombre, elige la sucursal, retiro o delivery (con su sector y la dirección
escrita) y cómo paga. Al tocar **Enviar pedido por WhatsApp** se abre el WhatsApp de esa sucursal con un
mensaje ordenado: sucursal, modalidad, sector y dirección, forma de pago, productos con todo lo elegido, notas,
cupón y subtotal. El local responde confirmando disponibilidad y total. El pedido queda guardado en el navegador
del cliente aunque cierre la página.

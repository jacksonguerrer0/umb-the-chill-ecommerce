# Análisis de Requisitos — The Chill E-commerce Local

## 1. Definición del Problema

The Chill es un negocio local de granizados y bebidas frías que actualmente toma sus pedidos por WhatsApp, llamadas o cuando el cliente llega en persona. Ese esquema funciona, pero tiene límites claros: clientes que no pueden usar esos canales de forma independiente quedan excluidos, y el negocio pierde pedidos por no tener presencia digital propia.

El objetivo del proyecto es construir una SPA (Single Page Application) donde los clientes consulten el menú, personalicen sus productos y hagan pedidos sin fricciones — y donde Jackson, el administrador, gestione productos y pedidos desde el mismo sistema. El requisito de accesibilidad no es un añadido opcional: está en el núcleo del proyecto porque los usuarios identificados en el análisis incluyen personas con discapacidad visual y baja visión.

---

## 2. Roles de Usuario y Necesidades de Accesibilidad

### Rol 1 — Cliente con Discapacidad Visual (Sofía)

- **Descripción:** Cliente habitual de The Chill. Si la interfaz depende solo de imágenes sin texto alternativo, o usa el color como único indicador de estado, Sofía no puede usarla.
- **Necesidades de accesibilidad:**
  - Imágenes de productos con texto alternativo descriptivo, no genérico (ej: "Maracumango, tamaño grande", no "producto1.jpg").
  - Selectores de sabor y tamaño navegables con teclado, con el valor seleccionado anunciado al cambiar.
  - El carrito y el resumen del pedido en orden lógico dentro del DOM, no en el orden visual de la pantalla.
  - Confirmaciones y errores anunciados con `aria-live` sin que Sofía tenga que navegar hasta encontrarlos.
- **Criterios WCAG 2.2 AA aplicables:** texto alternativo en imágenes (1.1.1), orden de lectura lógico (1.3.2), anuncios de estado con `aria-live` (4.1.3), y nombre/rol/valor correctos en todos los controles interactivos (4.1.2).

---

### Rol 2 — Cliente con Baja Visión (Don Hernando)

- **Descripción:** Don Hernando usa el navegador con zoom al 150% y lee bien cuando el texto es grande y el contraste es alto. No usa lector de pantalla. El problema con él no es la semántica, sino el layout: si algo se rompe al ampliar, o si el contraste es bajo, se pierde.
- **Necesidades de accesibilidad:**
  - Texto base mínimo de 16px que escale sin romper el layout — nada de `overflow: hidden` que corte el contenido ampliado.
  - Contraste 4.5:1 como mínimo entre texto y fondo en todos los componentes, incluyendo estados hover y disabled.
  - Botones con área de clic suficientemente grande (mínimo 44×44 px según WCAG 2.2).
  - Indicador de foco grueso y con color contrastante, porque Don Hernando sí navega con teclado cuando la precisión del ratón le falla.
- **Criterios WCAG 2.2 relevantes:** 1.4.3 (contraste mínimo), 1.4.4 (cambio de tamaño de texto), 1.4.10 (reflow), 2.4.7 (foco visible), 2.4.11 (apariencia del foco — WCAG 2.2), 2.5.8 (tamaño del objetivo — WCAG 2.2).

---

### Rol 3 — Administrador del Negocio (Jackson)

- **Descripción:** Jackson es el dueño o encargado de The Chill. Necesita agregar o editar productos, cambiar precios, marcar disponibilidad y revisar pedidos recibidos. No tiene perfil técnico avanzado, así que el panel tiene que ser directo: poca fricción, mucho feedback.
- **Necesidades de accesibilidad / usabilidad:**
  - Formularios con etiquetas visibles y validación mientras escribe, no solo al enviar.
  - Acciones destructivas (eliminar producto) con diálogo de confirmación — Jackson va a cometer errores de clic, cualquiera los comete.
  - Feedback claro después de cada acción: guardado, error, actualización exitosa.
  - Navegación por teclado en el panel, porque es más ágil que el ratón cuando se gestiona muchos productos seguidos.
- **Criterios WCAG 2.2 AA relevantes:** 3.3.1 (identificación de errores), 3.3.2 (etiquetas e instrucciones), 3.3.4 (prevención de errores), 2.1.1 (teclado).

---

## 3. Historias de Usuario

### HU-01 — Ver Catálogo de Productos (Cliente)

**Como** cliente de The Chill,
**quiero** ver el catálogo de granizados y bebidas disponibles con su precio y descripción,
**para** decidir qué quiero pedir antes de personalizar mi bebida.

**Criterios de aceptación:**
- El catálogo muestra tarjetas de producto con imagen, nombre, descripción corta y precio.
- Se puede filtrar por categoría (granizados, bebidas frías, combos).
- La página carga el catálogo en menos de 3 segundos con conexión normal.
- Los productos agotados aparecen como "No disponible" pero siguen visibles.

**Criterios de accesibilidad WCAG 2.2 AA:**
- Cada imagen de producto tiene `alt` descriptivo, ej: "Granizado de fresa en vaso grande".
- Los filtros de categoría son navegables con teclado y anuncian el filtro activo con `aria-pressed` o `aria-selected`.
- El estado "No disponible" no se indica solo con color gris; siempre incluye texto visible, porque hay usuarios que no distinguen diferencias de color.

---

### HU-02 — Filtrar Productos por Categoría, Sabor o Tamaño (Cliente)

**Como** cliente,
**quiero** filtrar el menú por tipo de bebida, sabor o tamaño,
**para** encontrar rápido lo que busco sin revisar todo el catálogo.

**Criterios de aceptación:**
- Los filtros están visibles en la parte superior del catálogo.
- Al aplicar un filtro, el catálogo se actualiza sin recargar la página.
- El usuario puede limpiar los filtros con un botón "Ver todo".
- Si no hay resultados, muestra el mensaje "No encontramos productos con esos filtros."

**Criterios de accesibilidad WCAG 2.2 AA:**
- El cambio en el catálogo al filtrar se anuncia con `aria-live="polite"` — si Sofía aplica un filtro y el lector de pantalla no le dice que el resultado cambió, la funcionalidad no sirve para ella.
- Los botones de filtro tienen texto descriptivo visible, no solo íconos.
- El botón "Ver todo" tiene foco visible cuando se tabula.

---

### HU-03 — Personalizar un Granizado (Cliente)

**Como** cliente,
**quiero** elegir el tamaño y el sabor de mi granizado antes de agregarlo al carrito,
**para** pedir exactamente lo que quiero sin necesitar ir en persona.

**Criterios de aceptación:**
- La vista de detalle del producto muestra opciones de tamaño (pequeño, mediano, grande) y sabor.
- Al cambiar tamaño, el precio se actualiza automáticamente.
- El usuario puede seleccionar múltiples sabores si aplica (ej: granizado mixto).
- Existe un campo de "Notas del pedido" para instrucciones especiales.
- Botón "Agregar al carrito" activo solo cuando hay tamaño y sabor seleccionados.

**Criterios de accesibilidad WCAG 2.2 AA:**
- Los selectores de tamaño y sabor son completamente navegables con teclado.
- Cuando el precio cambia al elegir tamaño, el valor actualizado se anuncia con `aria-live="polite"` — de lo contrario, un usuario de lector de pantalla tendría que navegar manualmente para saber cuánto cuesta.
- Las opciones de sabor/tamaño tienen etiquetas visibles, no solo íconos de color.
- El botón "Agregar al carrito" usa `aria-disabled="true"` (no `disabled`) mientras la selección está incompleta, para que el lector de pantalla pueda anunciarlo aunque no esté activo.

---

### HU-04 — Agregar Productos al Carrito (Cliente)

**Como** cliente,
**quiero** agregar varios productos al carrito y ver el resumen antes de confirmar,
**para** hacer un pedido completo en una sola sesión.

**Criterios de aceptación:**
- El carrito muestra cada producto con nombre, personalización elegida, cantidad y precio unitario.
- El total se calcula automáticamente y se actualiza al cambiar cantidades.
- El cliente puede eliminar productos del carrito individualmente.
- El carrito persiste durante la sesión aunque el usuario navegue a otras vistas.

**Criterios de accesibilidad WCAG 2.2 AA:**
- El carrito tiene estructura de lista accesible con los roles ARIA correspondientes.
- Al agregar un producto, el contador del carrito se anuncia con `aria-live` — sin ese anuncio, un usuario ciego no sabe si el botón funcionó o no.
- Los botones de eliminar tienen `aria-label` descriptivo por producto: "Eliminar granizado de mango del carrito", no solo un ícono de papelera sin texto.
- Los inputs de cantidad son navegables con teclado.

---

### HU-05 — Confirmar y Enviar Pedido (Cliente)

**Como** cliente,
**quiero** revisar mi pedido y enviarlo con mi información de contacto,
**para** que el negocio lo prepare y me avise cuando esté listo.

**Criterios de aceptación:**
- La vista de confirmación muestra el resumen completo: productos, cantidades, precios y total.
- El formulario pide nombre, teléfono y opción de entrega (recoger en tienda o domicilio).
- Si es domicilio, pide la dirección de entrega.
- Al enviar, muestra confirmación con número de pedido y tiempo estimado.
- Si falla el envío, muestra un mensaje descriptivo con opción de reintentar.

**Criterios de accesibilidad WCAG 2.2 AA:**
- Todos los campos tienen `<label>` visibles encima del campo, no solo como placeholder.
- Los errores de validación aparecen en texto junto al campo afectado y están vinculados con `aria-describedby`, para que el lector de pantalla los asocie al campo correcto sin ambigüedad.
- El botón "Confirmar pedido" se deshabilita durante el envío para evitar doble submit.
- La confirmación exitosa se anuncia con `aria-live="polite"` para que el usuario no tenga que buscarla navegando.

---

### HU-06 — Navegación Accesible con Lector de Pantalla (Cliente con Discapacidad Visual)

**Como** cliente con discapacidad visual,
**quiero** que toda la tienda sea navegable con mi lector de pantalla y teclado,
**para** hacer mis pedidos de forma completamente independiente.

**Criterios de aceptación:**
- Existe un skip link al inicio de cada vista que lleva directamente al catálogo o contenido principal.
- Los landmarks ARIA están definidos: `<header>`, `<nav>`, `<main>`, `<footer>`.
- Las imágenes decorativas tienen `alt=""` y las de producto tienen alt descriptivo.
- Los modales (ej: confirmación de eliminar producto) atrapan el foco y se cierran con Escape.

**Criterios de accesibilidad WCAG 2.2 AA:**
- Las imágenes informativas tienen texto alternativo; las decorativas tienen `alt=""` explícito para que el lector de pantalla las ignore.
- El skip link permite saltar la navegación repetida en cada vista — sin este mecanismo, Sofía tendría que tabular por todo el menú cada vez que carga una página.
- Los landmarks ARIA definen regiones claras de la página para que el lector de pantalla pueda orientarse.
- Todos los elementos interactivos tienen nombre, rol y valor accesibles.

---

### HU-07 — Gestionar Productos y Pedidos (Administrador)

**Como** administrador de The Chill,
**quiero** agregar, editar y desactivar productos, y revisar los pedidos recibidos,
**para** mantener el menú actualizado y gestionar la operación del negocio sin depender de otra persona.

**Criterios de aceptación:**
- Panel con lista de productos: nombre, precio, categoría, disponibilidad.
- Formulario para crear/editar producto con validación en tiempo real.
- La acción de desactivar o eliminar producto muestra confirmación antes de ejecutarse.
- Vista de pedidos con filtro por estado (pendiente, en preparación, listo, entregado).
- Las operaciones muestran estado: cargando, éxito o error.

**Criterios de accesibilidad WCAG 2.2 AA:**
- Los errores del formulario aparecen en texto junto al campo, no solo con un borde rojo que alguien podría no ver.
- El modal de confirmación (antes de eliminar) atrapa el foco mientras está abierto y cierra con Escape — si el foco escapa del modal, el usuario puede activar acciones detrás de él sin querer.
- Los mensajes de éxito y error se anuncian con `aria-live` para que lleguen al lector de pantalla sin que el usuario tenga que buscarlos.
- Todo el panel es navegable con teclado.

---

## 4. Priorización MoSCoW

### Must Have (Obligatorio — sin esto no funciona el negocio)

| ID     | Funcionalidad                                        |
|--------|------------------------------------------------------|
| HU-01  | Ver catálogo de productos                            |
| HU-03  | Personalizar un granizado (tamaño y sabor)           |
| HU-04  | Agregar productos al carrito                         |
| HU-05  | Confirmar y enviar pedido                            |
| HU-06  | Navegación accesible con lector de pantalla          |
| HU-07  | Panel de administración de productos y pedidos       |

### Should Have (Importante — debe estar si el tiempo lo permite)

| ID     | Funcionalidad                                        |
|--------|------------------------------------------------------|
| HU-02  | Filtrar productos por categoría, sabor o tamaño      |

### Could Have (Deseable — mejora la experiencia)

| ID     | Funcionalidad                                        |
|--------|------------------------------------------------------|
| —      | Notificaciones de estado del pedido por WhatsApp     |
| —      | Modo oscuro accesible                                |
| —      | Sistema de favoritos / productos guardados           |
| —      | Reseñas o calificación de productos                  |

### Won't Have (Para esta versión, fuera del alcance)

| ID     | Funcionalidad                                        |
|--------|------------------------------------------------------|
| —      | Pasarela de pago en línea (PSE, tarjeta)             |
| —      | Módulo de inventario avanzado con alertas de stock   |
| —      | Aplicación móvil nativa                              |
| —      | Sistema de fidelización / puntos por compra          |

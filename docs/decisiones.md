# Decisiones de Diseño — The Chill E-commerce Local

## 1. Decisiones principales

### SPA para mejorar la experiencia

Se decidió diseñar el sistema como una **SPA** porque el usuario necesita moverse entre el catálogo, el detalle del producto y el carrito sin que la página recargue cada vez. Con eso el flujo de compra se siente más fluido, y el carrito mantiene su estado mientras el usuario sigue explorando productos.

Un punto importante: al cambiar de vista hay que gestionar bien el foco del teclado y anunciar los cambios para usuarios con lector de pantalla, porque en una SPA eso no ocurre solo.

---

### Colores frescos y relacionados con la marca

La paleta usa tonos azules y cyan porque van bien con la idea de bebidas frías y frescura que es lo que vende The Chill. Se complementa con naranja para precios, verde para disponibilidad y rojo para errores.

El punto clave es que esos estados no se comunican solo por color: siempre van acompañados de texto, porque si alguien tiene daltonismo o usa alto contraste, los colores solos no le dicen nada. Por eso cada estado tiene una etiqueta visible:

* "Disponible"
* "No disponible"
* "Error"
* "Pedido confirmado"

---

### Personalización antes de agregar al carrito

Antes de que un producto entre al carrito, el usuario tiene que elegir tamaño, cantidad y notas opcionales. La razón es simple: el negocio necesita saber exactamente qué preparar, y si eso se puede aclarar antes de confirmar, se evitan malentendidos en el pedido.

Además se muestra el total actualizado antes de agregar, para que el cliente sepa cuánto va a pagar antes de comprometerse.

---

### Carrito visible y persistente

El carrito está disponible durante toda la navegación. El usuario puede seguir revisando el catálogo sin perder lo que ya agregó, lo cual es especialmente importante si tiene conexión lenta o está usando el celular y la navegación es más lenta.

---

### Formulario claro para confirmar pedido

El formulario pide lo justo: nombre, celular, tipo de entrega y dirección. El campo de dirección solo aparece cuando el usuario elige "Domicilio", así no hay campos innecesarios que confundan.

Todos los campos tienen etiquetas visibles, no solo placeholders que desaparecen al escribir.

---

## 2. Principios de diseño aplicados

### Abstracción

Los colores, botones y estilos se definen una sola vez como elementos reutilizables. Si se quiere cambiar el color principal de la marca, se cambia en un solo lugar y se aplica en toda la interfaz, no pantalla por pantalla.

### Modularidad

La interfaz está dividida en partes independientes: navbar, cards de producto, botones, formulario de pedido, carrito y tabla administrativa. Cada una existe por su cuenta y se puede reutilizar donde haga falta.

### Separación de responsabilidades

Cada pantalla hace una sola cosa bien:

* El inicio presenta el negocio.
* El catálogo muestra los productos.
* El detalle permite personalizar el pedido.
* El carrito permite revisarlo y confirmarlo.
* El panel administrativo permite gestionar productos.

### Bajo acoplamiento

Los componentes están diseñados para no depender unos de otros más de lo necesario. Cambiar cómo luce una card de producto no debería romper nada en el formulario del carrito.

### Alta cohesión

Cada componente se ocupa de una sola cosa: la card muestra info del producto, el formulario recoge los datos del cliente, la tabla del admin organiza los registros. Nada hace dos cosas al mismo tiempo.

---

## 3. Patrones utilizados

| Patrón                   | Aplicación                                        |
| ------------------------ | ------------------------------------------------- |
| Catálogo con detalle     | El usuario ve productos y entra al detalle de uno |
| Flujo de compra          | Catálogo → Detalle → Carrito → Confirmación       |
| Progressive disclosure   | La dirección aparece solo si se elige domicilio   |
| Dashboard administrativo | El administrador gestiona productos y pedidos     |

---

## 4. Cualidades de software

### Accesibilidad

El diseño tiene navegación clara, botones con área suficiente, formularios con etiquetas visibles, mensajes de error ubicados junto al campo que los genera y contraste suficiente en texto y estados.

### Usabilidad

El flujo está pensado para que hacer un pedido tome la menor cantidad de pasos posible. No se le pide al usuario información que no se necesita.

### Modificabilidad

Como los componentes son reutilizables, agregar una nueva pantalla o cambiar el estilo de algo no implica tocar todo el proyecto.

### Interoperabilidad

El sistema funciona en navegador y sigue estándares web: HTML semántico, atributos ARIA donde son necesarios y criterios WCAG 2.2.

---

## 5. Relación con SWEBOK

El proyecto toca varias áreas de SWEBOK de forma concreta:

- **Requisitos:** se trabajaron con historias de usuario y priorización MoSCoW.
- **Diseño de software:** se aplicaron principios como cohesión, acoplamiento y separación de responsabilidades en la arquitectura de la interfaz.
- **Calidad:** se tomaron decisiones específicas de accesibilidad y usabilidad, no solo de estética.
- **Mantenimiento:** los componentes reutilizables y la documentación están pensados para que otra persona pueda entrar al proyecto y modificarlo sin tener que descifrar todo desde cero.

# Guion para Video Demo — The Chill E-commerce Local

**Duración objetivo:** 5 a 7 minutos
**Formato recomendado:** Grabación de pantalla (Figma en modo Present) + voz en off o cámara frontal.
**Herramienta sugerida:** OBS Studio, Loom, o grabación desde Zoom.

---

## Estructura del Video

| Segmento | Duración aprox. | Contenido |
|----------|-----------------|-----------|
| Introducción | 0:00 – 0:40 | Qué es The Chill y qué vamos a ver |
| El problema y los usuarios | 0:40 – 1:40 | Qué resuelve la SPA y quiénes la usan |
| Flujo de compra | 1:40 – 3:20 | Demostración del prototipo en Figma |
| Decisiones de accesibilidad | 3:20 – 4:40 | Qué se hizo y por qué |
| Componentes reutilizables | 4:40 – 5:30 | Estado real del prototipo y wireframes |
| Principios de ingeniería | 5:30 – 6:20 | Conexión con el curso |
| Cierre y validaciones | 6:20 – 7:00 | Qué falta por validar |

---

## Guion Completo

---

### [0:00 – 0:40] Introducción

> "Hola, soy Jackson. Esto es el trabajo de la actividad 2 del módulo 1: diseño de interfaz y navegación para una SPA accesible.
>
> El proyecto se llama The Chill. Es el diseño completo de un e-commerce local para un negocio de granizados y bebidas frías. No es una aplicación funcional todavía. Es el diseño de la interfaz: wireframes en papel, prototipo en Figma, diagramas y documentación, todo con enfoque en accesibilidad web bajo WCAG 2.2 nivel AA.
>
> El código y los documentos están en el repositorio en GitHub. El prototipo lo van a ver navegando conmigo en Figma. Empecemos."

---

### [0:40 – 1:40] El Problema y los Usuarios

> "El problema que resuelve The Chill es concreto. Muchos negocios locales de comida y bebidas toman pedidos por WhatsApp o en persona. Eso funciona hasta cierto punto, pero trae problemas: errores en los pedidos, horarios limitados, y barreras para clientes con discapacidades que no pueden usar esas vías de forma independiente.
>
> The Chill propone una plataforma digital propia donde el cliente puede ver el menú, personalizar su granizado y enviar el pedido desde el navegador, sin depender de nadie.
>
> Para este diseño definí cuatro perfiles de usuario con necesidades distintas.
>
> Sofía es una cliente con discapacidad visual que usa lector de pantalla. Necesita que las imágenes del catálogo tengan texto alternativo descriptivo, que los selectores de tamaño y sabor sean navegables con teclado, y que el precio actualice con anuncio de audio cuando cambia.
>
> Don Hernando es un cliente adulto con baja visión que usa el navegador con zoom al 150%. Necesita contraste fuerte en todos los textos, botones grandes y foco muy visible cuando tabula por la página.
>
> Miguel tiene conexión de datos débil. Necesita que la app sea liviana, que le diga qué está cargando y que si el pedido falla, lo explique con claridad y le permita reintentar sin perder lo que escribió.
>
> Y el cuarto perfil es el administrador del negocio: necesita un panel simple para gestionar productos y pedidos sin conocimientos técnicos."

---

### [1:40 – 3:20] Flujo de Compra — Demostración del Prototipo

*(Abrir Figma en modo Present)*

> "Voy a navegar por el prototipo para mostrar el flujo completo de compra.
>
> Empezamos en el landing de The Chill. Hay una sección hero con el nombre del negocio y los productos destacados. Al presionar Tab, el primer elemento que aparece es el skip link, que permite saltar directamente al contenido sin pasar por el navbar. Eso es fundamental para usuarios de teclado y lectores de pantalla.
>
> Desde aquí voy al catálogo haciendo clic en 'Ver menú'. En el catálogo están todas las bebidas con su imagen, nombre y precio. Puedo filtrar por categoría con los botones de la parte superior. Cada vez que filtro, el resultado se anuncia con aria-live: el lector de pantalla le dice al usuario cuántos productos están mostrando.
>
> Hago clic en un granizado para ver el detalle. Acá puedo elegir el tamaño —pequeño, mediano o grande— usando los radio buttons. Al cambiar el tamaño, el precio se actualiza en tiempo real. Ese precio tiene aria-live, así que el lector de pantalla lo anuncia automáticamente.
>
> También elijo el sabor. Cuando tengo tamaño y sabor seleccionados, el botón 'Agregar al carrito' se habilita. Antes de eso está deshabilitado con aria-disabled, pero el teclado puede seguir enfocándolo para que el lector de pantalla sepa que existe.
>
> Al agregar al carrito, el contador del navbar actualiza y anuncia el cambio.
>
> En el carrito reviso el pedido, lleno los datos de contacto, elijo si recojo en tienda o pido domicilio. Si elijo domicilio, el campo de dirección aparece y el foco se mueve automáticamente hacia él.
>
> Al confirmar, aparece la pantalla de confirmación con el número del pedido y el tiempo estimado."

---

### [3:20 – 4:40] Decisiones de Accesibilidad

> "Quiero explicar tres decisiones de accesibilidad que considero las más relevantes del diseño.
>
> La primera es el precio dinámico con aria-live. Cuando el usuario cambia el tamaño del granizado, el precio actualiza visualmente, pero también se anuncia con audio para lectores de pantalla. Sin ese aria-live, Sofía vería el precio cambiar pero no lo escucharía, y tendría que volver a enfocar el elemento para saberlo. Es un detalle pequeño que marca la diferencia.
>
> La segunda es el botón de 'Agregar al carrito' con aria-disabled en lugar de disabled. La diferencia es sutil pero importante: un botón con disabled no puede recibir foco, así que el lector de pantalla no lo lee y el usuario no sabe que existe. Con aria-disabled, el botón es enfocable y el lector puede anunciar que está presente pero requiere una acción del usuario primero.
>
> La tercera es el campo de dirección condicional. Solo aparece si el usuario elige domicilio. Cuando aparece, el foco se mueve automáticamente hacia ese campo. Sin ese manejo de foco, un usuario de teclado o lector de pantalla tendría que adivinar que apareció algo nuevo en la pantalla, porque visualmente no está mirando esa sección."

---

### [4:40 – 5:30] Componentes Reutilizables — Estado del Prototipo

*(Mostrar las pantallas disponibles en Figma, luego las fotos de wireframes)*

> "Quiero ser claro sobre el estado del prototipo.
>
> En Figma tengo dos pantallas completamente desarrolladas: el landing y el catálogo. Esas dos están con todos sus componentes y variantes: botones en sus estados default, hover, focus, disabled y loading; las cards de producto; los filtros; la navbar. Cada componente tiene sus variantes de estado para guiar tanto el diseño visual como la implementación futura.
>
> Las demás pantallas —el detalle del producto, el carrito, el formulario de pedido y la confirmación— están documentadas en wireframes físicos. Tengo las fotos de esos wireframes en el repositorio: tres fotografías que cubren las cinco pantallas del flujo. Eso está así porque el plan Starter de Figma tiene un límite de páginas, y decidí priorizar las pantallas que más componentes reutilizables tienen.
>
> Toda la lógica de componentes aplica igual al resto del flujo: si necesito cambiar el estilo del botón primario, lo cambio en un solo lugar y se actualiza en todo el prototipo."

---

### [5:30 – 6:20] Principios de Ingeniería de Software

> "Este trabajo no es solo diseño visual. Tiene una base de ingeniería de software.
>
> Primero, abstracción. En lugar de escribir el código de color en cada componente, usé Design Tokens: 'cyan-profundo', 'state-exito', 'body-base'. Es lo mismo que una variable CSS: el valor concreto queda abstraído detrás de un nombre con significado.
>
> Segundo, modularidad y bajo acoplamiento. El selector de tamaño no sabe nada del carrito. El carrito no sabe nada de los filtros del catálogo. Cada componente tiene su responsabilidad y no depende de los internos de los otros. Eso está documentado en el archivo decisiones.md del repositorio.
>
> Tercero, separación de responsabilidades. Los diagramas PlantUML muestran cómo el sistema se divide en capas: presentación, servicios y API. Cada capa tiene su función. Eso es lo que describe SWEBOK en el área de Software Design."

---

### [6:20 – 7:00] Cierre y Validaciones Pendientes

> "Para cerrar, quiero ser claro sobre lo que está hecho y lo que falta.
>
> El diseño está completo: wireframes, prototipo de las pantallas principales en Figma, diagramas PlantUML y toda la documentación. La accesibilidad real se valida cuando haya implementación HTML, con herramientas como axe DevTools o Lighthouse. Eso está marcado como pendiente en la documentación.
>
> También hice la validación informal del wireframe con un compañero usando la plantilla de role-playing que está en el repositorio.
>
> Eso es todo. El repositorio en GitHub tiene todo el material. Gracias."

---

## Consejos para Grabar

- Graba en un lugar sin ruido de fondo.
- Pon Figma en pantalla completa antes de empezar.
- No leas esto palabra por palabra. Conocés el proyecto; hablá con el guion como referencia.
- Si te equivocás, para, respira y seguí. Es más fácil cortar en edición que volver a grabar todo.
- Ensayá una vez completa antes de grabar en serio. Así sabés si estás dentro del tiempo.

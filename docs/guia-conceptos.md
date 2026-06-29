# Guía de Conceptos Técnicos — Diseño de Interfaz y Modelo de Navegación para SPA Accesible

**Proyecto:** The Chill — E-commerce local de granizados  
**Actividad:** Diseño de Interfaz y Modelo de Navegación para SPA Accesible  
**Programa:** Ingeniería de Software — UMB

---

## 1. SWEBOK — Software Engineering Body of Knowledge

### Qué es y por qué existe

SWEBOK es el intento de la IEEE Computer Society de responder una pregunta incómoda: ¿qué debería saber formalmente alguien que se llame ingeniero de software? No es un libro de texto ni un tutorial. Es una guía que delimita el cuerpo de conocimiento de la disciplina — lo que existe, cómo se organiza y qué términos son estándar.

Existe porque la ingeniería de software durante mucho tiempo fue un campo donde cada quien inventaba sus propios términos para los mismos conceptos. SWEBOK da un vocabulario compartido. Cuando en un equipo alguien dice "Requisito Funcional" o "Cohesión" o "Verificación vs. Validación", esos términos tienen un significado preciso que SWEBOK ayuda a establecer. Su versión actual (v4, 2024) organiza el conocimiento en áreas llamadas KAs (Knowledge Areas).

Para este proyecto las KAs más relevantes son cuatro:

### Software Requirements (Requisitos de Software)

Esta KA se ocupa de cómo identificar, analizar, especificar y gestionar lo que el sistema debe hacer. Distingue entre requisitos funcionales (qué hace el sistema) y no funcionales (bajo qué restricciones lo hace — rendimiento, seguridad, accesibilidad).

En The Chill esto aparece desde el primer paso: las historias de usuario no son solo "el usuario quiere ver granizados". Son especificaciones que incluyen criterios de aceptación de accesibilidad — que el formulario de checkout sea operable por teclado, que los errores de validación sean anunciados por lectores de pantalla. Esos no son detalles de implementación; son requisitos que deben especificarse antes de diseñar la interfaz.

### Software Design (Diseño de Software)

Esta KA cubre los principios que guían cómo estructurar una solución. Incluye conceptos como abstracción, modularidad, separación de responsabilidades, acoplamiento y cohesión. No es solo "cómo se ve el código" — es cómo se toman decisiones de arquitectura que determinan si el sistema es mantenible o un desastre en seis meses.

En The Chill el diseño de software aparece en la decisión de dividir la SPA en componentes (Catálogo, Carrito, Checkout, Perfil), en la definición de design tokens como capa de abstracción visual, y en la separación entre la lógica de negocio (calcular el total del carrito) y la presentación (cómo se muestra ese total en pantalla).

### Software Quality (Calidad de Software)

Esta KA no habla solo de testing. Habla de qué atributos de calidad importan y cómo verificarlos. Incluye la accesibilidad como atributo de calidad del producto, junto con usabilidad, mantenibilidad y confiabilidad. SWEBOK distingue entre verificación (¿construiste el sistema correctamente?) y validación (¿construiste el sistema correcto?).

En The Chill la accesibilidad WCAG 2.2 AA no es un capricho del profesor — es un atributo de calidad especificado como requisito no funcional. Verificarlo significa usar herramientas automáticas (axe, Lighthouse) y manuales (navegar solo con teclado, usar un lector de pantalla). Validarlo significa que usuarios reales con discapacidades puedan completar una compra.

### Software Maintenance (Mantenimiento de Software)

Esta KA reconoce que el software no termina cuando se entrega. El mantenimiento incluye corrección de defectos, adaptación a nuevos entornos y mejora de funcionalidad. En el contexto de diseño, esto se traduce en: ¿qué tan fácil es actualizar la paleta de colores de The Chill cuando la marca evolucione? Si los colores están hardcodeados en 47 componentes, es mantenimiento costoso. Si están en design tokens centralizados, es cambiar tres variables.

---

## 2. WCAG 2.2 Nivel AA

### Qué son y cómo se organizan

WCAG (Web Content Accessibility Guidelines) es el estándar internacional publicado por el W3C que define qué significa que un contenido web sea accesible. La versión 2.2, publicada en 2023, organiza sus criterios bajo cuatro principios, conocidos como POUR:

- **Perceptible:** La información y los componentes de la interfaz deben presentarse de formas que los usuarios puedan percibir. Un usuario ciego no puede percibir una imagen sin texto alternativo. Un usuario sordo no puede percibir un video sin subtítulos.
- **Operable:** Los componentes y la navegación deben ser operables. Un usuario que no usa ratón debe poder navegar todo con teclado. Un usuario con temblor en las manos necesita objetivos de toque suficientemente grandes.
- **Comprensible:** La información y el funcionamiento de la interfaz deben ser comprensibles. Los errores de formulario deben explicar qué salió mal y cómo corregirlo, no solo poner el borde en rojo.
- **Robusto:** El contenido debe ser suficientemente robusto para ser interpretado por tecnologías asistivas. Si el HTML está mal estructurado, un lector de pantalla no puede construir un árbol de accesibilidad funcional.

### Diferencia entre niveles A, AA y AAA

| Nivel | Descripción | Aplicabilidad |
|-------|-------------|---------------|
| A | Criterios mínimos. Sin estos, el contenido es inutilizable para algunos usuarios. | Obligatorio en cualquier contexto |
| AA | Estándar práctico. Elimina las barreras más comunes. Es lo que exigen leyes como ADA, EN 301 549, y es el objetivo de The Chill. | Meta del proyecto |
| AAA | Criterios avanzados. No siempre es posible cumplirlos para todo el contenido. | Deseable donde sea factible |

La diferencia no es solo "más difícil". AAA incluye criterios que a veces son imposibles de cumplir simultáneamente o que dependen del contexto (como proporcionar lenguaje de señas para todo el contenido de audio). Por eso el nivel AA es el estándar de facto para aplicaciones comerciales.

### Criterios clave para una SPA de e-commerce

**1.1.1 — Contenido No Textual (A)**  
Todo contenido no textual debe tener un texto alternativo. En The Chill, cada imagen de granizado necesita un `alt` descriptivo ("Granizado de mango con chamoy y chile, tamaño mediano"), no genérico ("imagen") ni vacío. Si es decorativa, `alt=""` para que el lector de pantalla la ignore.

**1.3.2 — Secuencia con Significado (A)**  
Si el orden en que se presenta la información importa, ese orden debe preservarse en el DOM. En la página de detalle de un granizado, si visualmente aparece "Precio: $8.000" antes de "Agregar al carrito", el DOM debe tener ese mismo orden — no lo contrario con CSS reordenando visualmente.

**1.4.3 — Contraste Mínimo (AA)**  
El texto normal necesita una relación de contraste de al menos 4.5:1 contra su fondo. El texto grande (18pt o 14pt negrita) necesita 3:1. El azul vibrante sobre blanco puede verse bonito y aún así fallar. La paleta de The Chill debe verificarse con herramientas como el Colour Contrast Analyser antes de implementarse.

**1.4.4 — Cambio de Tamaño de Texto (AA)**  
El texto debe ser legible al 200% de zoom sin pérdida de contenido. Si el layout usa unidades fijas en píxeles y se rompe al hacer zoom, falla. Las unidades `rem` y `em` son el camino correcto.

**1.4.10 — Reajuste (AA)**  
El contenido debe presentarse sin scroll horizontal en un viewport de 320px de ancho CSS. Esto es responsive design como requisito de accesibilidad, no solo de UX. Un usuario con baja visión que hace zoom hasta 400% en un teléfono tiene efectivamente 320px disponibles.

**2.1.1 — Teclado (A)**  
Toda funcionalidad debe ser operable por teclado. En The Chill: agregar al carrito, navegar entre categorías, completar el formulario de checkout, cerrar modales, seleccionar sabores — todo debe funcionar con Tab, Enter, Escape y las flechas.

**2.4.7 — Foco Visible (AA)**  
El indicador de foco de teclado debe ser visible. El patrón `outline: none` en CSS aplicado globalmente es una violación directa de este criterio. El diseño debe incluir un indicador de foco que sea perceptible — no tiene que ser el anillo azul del navegador, pero tiene que ser algo visible.

**2.4.11 — Apariencia del Foco (AA, nuevo en 2.2)**  
Refuerza 2.4.7 con requisitos más precisos: el indicador de foco debe tener un área mínima, una relación de contraste mínima y no debe estar completamente oculto. En The Chill los botones del carrito y los campos del formulario de checkout deben tener indicadores de foco que superen estos umbrales.

**2.5.8 — Tamaño del Objetivo (AA, nuevo en 2.2)**  
Los objetivos de toque/clic deben tener al menos 24×24 píxeles CSS. Esto es crítico en mobile. Los íconos pequeños de "eliminar ítem del carrito" que son apenas 16×16px fallan. La solución habitual es agrandar el área clicable con padding sin cambiar el ícono visible.

**3.3.1 — Identificación de Errores (A)**  
Si se detecta automáticamente un error en un campo de formulario, ese error debe describirse en texto. El borde rojo solo no es suficiente — necesita un mensaje como "El campo de teléfono solo acepta números" asociado al campo.

**3.3.2 — Etiquetas o Instrucciones (A)**  
Los campos de formulario deben tener etiquetas o instrucciones que expliquen qué se espera. No alcanza con el placeholder (que desaparece al escribir). Cada campo del formulario de checkout de The Chill necesita un `<label>` asociado.

**3.3.4 — Prevención de Errores (AA)**  
Para transacciones financieras, el usuario debe poder revisar, confirmar o corregir antes de confirmar. En The Chill esto se implementa con la pantalla de resumen antes del botón final de "Confirmar pedido".

**4.1.2 — Nombre, Función, Valor (A)**  
Todo componente de interfaz debe tener nombre accesible, función y estado disponibles para tecnologías asistivas. Un botón `<div class="btn">` sin role, sin nombre y sin manejo de teclado falla este criterio. Un `<button>Agregar al carrito</button>` lo cumple de forma nativa.

**4.1.3 — Mensajes de Estado (AA)**  
Los mensajes de estado (confirmaciones, alertas de error, notificaciones) deben poder ser determinados programáticamente sin recibir el foco. Cuando el usuario agrega un granizado al carrito en The Chill y aparece el mensaje "Ítem agregado", ese mensaje debe usar `aria-live` para ser anunciado al usuario de lector de pantalla sin interrumpir su flujo de navegación.

### Casos concretos: qué fallaría sin estos criterios

**Sofía** (joven usuaria con baja visión que usa zoom al 200% y alto contraste):
- Sin 1.4.10: el layout del catálogo de granizados se rompe horizontalmente al hacer zoom. Debe scrollear horizontalmente para ver los precios.
- Sin 1.4.3: los textos de las etiquetas de categoría sobre el fondo de imagen no tienen contraste suficiente y son ilegibles con su esquema de alto contraste.
- Sin 2.4.11: el indicador de foco es tan delgado que no puede seguir visualmente dónde está mientras navega con teclado.

**Don Hernando** (adulto mayor con movilidad reducida en manos, usa solo teclado y a veces lector de pantalla):
- Sin 2.1.1: no puede seleccionar el sabor del granizado porque el selector usa un widget de arrastrar y soltar sin alternativa de teclado.
- Sin 4.1.3: agrega un ítem al carrito pero no recibe ninguna confirmación auditiva. No sabe si la acción funcionó.
- Sin 3.3.1: el formulario de checkout rechaza su número de teléfono sin explicar qué formato se espera. Solo ve un campo en rojo.
- Sin 2.5.8: los botones "+" y "-" de cantidad de ítems son demasiado pequeños para activarlos con precisión dado su temblor.

---

## 3. ARIA — Accessible Rich Internet Applications

### Por qué existe y la regla de oro

HTML semántico nativo resuelve la mayoría de los problemas de accesibilidad sin ARIA. Un `<button>` ya tiene rol "button", ya es focusable, ya responde a Enter y Space, ya anuncia su nombre al lector de pantalla. Si usas `<div onclick="...">` en su lugar, tienes que agregar manualmente todo lo que `<button>` da gratis.

ARIA existe para los casos donde el HTML semántico no alcanza — principalmente widgets complejos que no tienen equivalente en HTML (pestañas, acordeones, árboles de selección, carruseles) y para comunicar estados dinámicos en aplicaciones que cambian sin recargar la página.

La regla fundamental es: **no uses ARIA si hay un elemento HTML nativo que haga lo mismo**. ARIA mal usado no es neutral — introduce errores de accesibilidad activos. Un `role="button"` en un `<div>` sin `tabindex="0"` y sin manejadores de teclado es peor que no tener nada porque el lector de pantalla anuncia "botón" pero el usuario de teclado no puede activarlo.

### Atributos clave en el proyecto The Chill

**`aria-live` y `aria-atomic`**  
Se usan en regiones que cambian dinámicamente. `aria-live="polite"` anuncia el cambio cuando el usuario termina lo que está haciendo. `aria-live="assertive"` interrumpe inmediatamente — reservar para errores críticos. `aria-atomic="true"` hace que el lector de pantalla anuncie toda la región, no solo el nodo que cambió.

En The Chill: el contador del carrito en el header usa `aria-live="polite"` para anunciar "3 ítems en el carrito" cuando se agrega uno nuevo. El mensaje de confirmación de pedido usa `aria-live="polite"`. Un error de pago podría usar `aria-live="assertive"`.

```html
<div aria-live="polite" aria-atomic="true" class="cart-status">
  3 ítems en el carrito
</div>
```

**`aria-disabled` vs `disabled`**  
Esta es una distinción que la mayoría de los desarrolladores no conoce hasta que la necesita.

| Atributo | Efectos |
|----------|---------|
| `disabled` (HTML nativo) | Elimina el elemento del orden de tabulación, impide activación, lo excluye del árbol de accesibilidad, no se envía en formularios |
| `aria-disabled="true"` | Anuncia el elemento como deshabilitado al lector de pantalla, pero mantiene el elemento en el orden de tabulación y en el árbol de accesibilidad |

¿Cuándo importa? Cuando tienes un botón "Confirmar pedido" deshabilitado porque el formulario tiene errores, y quieres que el usuario de teclado pueda llegar a ese botón y escuchar "Confirmar pedido, deshabilitado" — para que sepa que existe pero que necesita corregir algo antes. Con `disabled` puro, el usuario de solo-teclado ni siquiera sabe que el botón existe. En ese caso, `aria-disabled="true"` más prevención manual del click es la solución correcta.

**`aria-describedby`**  
Asocia un elemento con texto descriptivo adicional. Distinto de `aria-labelledby` (que nombra el elemento): `aria-describedby` agrega descripción suplementaria.

```html
<input type="tel" id="telefono" aria-describedby="tel-hint" />
<span id="tel-hint">Ingresa solo números, sin espacios ni guiones</span>
```

El lector de pantalla leerá: "Teléfono, campo de edición. Ingresa solo números, sin espacios ni guiones."

**`aria-label`**  
Proporciona un nombre accesible directamente en el elemento cuando no hay texto visible que sirva como etiqueta. Útil para íconos sin texto.

```html
<button aria-label="Eliminar granizado de mango del carrito">
  <svg><!-- ícono de basura --></svg>
</button>
```

Sin `aria-label`, el lector de pantalla leería solo "botón" sin contexto.

**`aria-pressed`**  
Para botones de alternancia (toggle). Comunica el estado activado/desactivado.

```html
<button aria-pressed="true">Filtro: Picante</button>
```

El lector de pantalla anuncia "Filtro: Picante, presionado" o "Filtro: Picante, no presionado". En The Chill, los filtros de categoría en el catálogo usan este patrón.

**`aria-selected`**  
Para elementos seleccionables dentro de widgets como tabs, listboxes o grids. Comunica cuál opción está actualmente seleccionada.

**`aria-busy`**  
Indica que una región está siendo actualizada. El lector de pantalla espera antes de leer el contenido actualizado.

```html
<section aria-busy="true" aria-label="Resultados de búsqueda">
  <!-- cargando... -->
</section>
```

En The Chill, cuando se filtra el catálogo, la región de resultados pone `aria-busy="true"` mientras carga y lo cambia a `false` cuando termina.

**`aria-required`**  
Comunica que un campo es obligatorio. Es complementario al atributo `required` de HTML — en muchos casos `required` solo es suficiente, pero en widgets customizados se necesita `aria-required`.

**Role landmarks**  
Los roles de landmark (`main`, `nav`, `header`, `footer`, `aside`, `search`) son quizá la forma más poderosa de mejorar la accesibilidad con poco esfuerzo. Permiten que los usuarios de lector de pantalla naveguen directamente a secciones de la página.

En The Chill el App Shell define: `<header>` con el logo y carrito, `<nav>` con la navegación principal, `<main>` donde se renderiza cada vista de la SPA, y `<footer>` con información de contacto. Los usuarios de lector de pantalla pueden saltar directamente a `<main>` sin escuchar toda la navegación cada vez.

---

## 4. SPA — Single Page Application

### Cómo funciona vs. una web tradicional

En una web tradicional multi-página, cada navegación descarga un documento HTML completo del servidor. El navegador destruye el estado anterior, procesa el nuevo documento, y el lector de pantalla reconoce el cambio de página porque el documento entero se reemplaza — esto resetea el foco al inicio y anuncia la nueva página automáticamente.

En una SPA, la aplicación carga una vez y actualiza el DOM dinámicamente. La URL puede cambiar (gracias al History API) pero el navegador nunca realiza una navegación real. Esto tiene consecuencias directas:

**Ventajas:**
- Navegación más rápida (sin redescarga de recursos estáticos)
- Experiencia más fluida (sin parpadeos de pantalla completa)
- Posibilidad de estado persistente entre vistas (el carrito no se pierde)
- Mejor para aplicaciones con mucha interactividad

**Problemas de accesibilidad específicos de las SPAs:**
- El foco no se resetea automáticamente cuando "cambias de página"
- Los lectores de pantalla no anuncian el cambio de vista automáticamente
- El historial de navegación del navegador puede desincronizarse si el router no lo gestiona bien
- El contenido dinámico no se anuncia a menos que se use `aria-live` explícitamente

### Por qué hay que gestionar el foco y los anuncios manualmente

Cuando Don Hernando navega de la vista del Catálogo a la vista del Carrito en The Chill usando solo teclado, si la SPA no hace nada especial, su foco queda exactamente donde estaba — probablemente en el botón del carrito del header que activó la navegación. El lector de pantalla no anunció "Carrito de compras" como lo haría con una navegación real.

La solución tiene dos partes:

1. **Mover el foco programáticamente** al `<h1>` de la nueva vista o a un elemento `<div tabindex="-1">` que actúe como receptáculo de foco.

```javascript
// Cuando el router cambia de vista
router.on('navigate', (newView) => {
  renderView(newView);
  // Mover foco al encabezado principal de la nueva vista
  document.querySelector('main h1')?.focus();
});
```

2. **Anunciar el cambio de vista** mediante un región `aria-live` o moviendo el foco a un elemento cuyo contenido el lector de pantalla leerá.

### Relación con el Router y el App Shell

El App Shell es la estructura fija que permanece entre navegaciones: header, nav, footer. El router intercepta los clicks en los enlaces de navegación, previene la navegación real del navegador, actualiza la URL mediante History API, y renderiza el componente correcto dentro de `<main>`.

En The Chill el App Shell incluye el header con el logo y el contador del carrito, la barra de navegación principal, y el `<main>` donde alternan las vistas de Catálogo, Detalle de Producto, Carrito y Checkout. El estado del carrito vive a nivel del App Shell para ser accesible desde cualquier vista.

---

## 5. Principios de Diseño de Software Aplicados a UI

### Abstracción: Design Tokens y Variables CSS

La abstracción en programación significa representar una propiedad o comportamiento de forma genérica para no depender de su implementación concreta. En UI esto aparece en los design tokens.

Un design token es una variable con nombre semántico que almacena un valor de diseño. En lugar de escribir `color: #1a73e8` en cada botón, defines `--color-primary: #1a73e8` y usas esa variable en todos lados. El token abstrae el valor concreto detrás de un nombre con significado.

La ventaja real: si The Chill decide cambiar su color primario de azul a verde lima para una campaña de verano, cambias una línea en el archivo de tokens. Sin tokens, buscas y reemplazas un hex en 30 archivos y rezas para no romper nada.

```css
:root {
  /* Primitivos - los valores concretos */
  --palette-cyan-400: #22d3ee;
  --palette-cyan-600: #0891b2;
  --palette-neutral-900: #111827;
  
  /* Semánticos - el significado, no el valor */
  --color-brand-primary: var(--palette-cyan-600);
  --color-text-primary: var(--palette-neutral-900);
  --color-interactive-focus: var(--palette-cyan-400);
  
  /* Espaciado */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
}
```

### Modularidad: Componentes Reutilizables

Un sistema modular divide el software en unidades independientes con interfaces bien definidas. En UI, esto son los componentes.

En The Chill, el componente `<ProductCard>` muestra un granizado individual. Ese mismo componente se usa en la vista de Catálogo, en los resultados de búsqueda y en la sección "También te puede gustar" del Detalle de Producto. Si hay un bug en cómo se muestra el precio, lo corriges en un lugar y se arregla en los tres contextos.

La modularidad implica que cada componente tiene una interfaz clara (sus props o parámetros), comportamiento predecible y no tiene efectos secundarios ocultos.

### Separación de Responsabilidades

El principio SoC (Separation of Concerns) dice que cada módulo debe ser responsable de un aspecto del problema, no de varios. En UI se materializan típicamente tres capas:

- **Presentación:** El HTML y CSS que define qué ve el usuario. No sabe cómo se obtienen los datos.
- **Lógica:** El JavaScript que maneja interacciones, validaciones, y coordina las otras capas. No sabe cómo se renderizan los píxeles.
- **Datos:** La capa que obtiene y persiste información (fetch a la API, localStorage para el carrito). No sabe cómo se presentan los datos.

En The Chill: la lógica de calcular el total del carrito (sumar precios, aplicar descuentos) vive separada del componente que muestra ese total. Si el día de mañana The Chill agrega un sistema de puntos, la lógica de cálculo cambia pero el componente de display no necesita saber nada al respecto.

### Bajo Acoplamiento y Alta Cohesión

**Acoplamiento** es el grado de dependencia entre módulos. Bajo acoplamiento significa que cambiar el módulo A no obliga a cambiar el módulo B.

**Cohesión** es el grado en que las responsabilidades de un módulo están relacionadas entre sí. Alta cohesión significa que todo lo que hace un módulo contribuye a un único propósito.

Ejemplo concreto en The Chill:

*Alto acoplamiento (mal):*
El componente `ProductCard` importa directamente el store del carrito, hace fetch a la API de productos, y también formatea la moneda. Si cambias la API, tienes que tocar `ProductCard`. Si cambias la lógica del carrito, tienes que tocar `ProductCard`. Está acoplado a todo.

*Bajo acoplamiento (bien):*
`ProductCard` recibe el producto como prop y emite un evento `addToCart`. No sabe nada de la API ni del store. El padre se encarga de conectar esas piezas. Cambiar la API no toca `ProductCard`.

*Baja cohesión (mal):*
Un módulo `utils.js` que contiene formateo de moneda, validación de formularios, manejo del carrito y peticiones HTTP. No tiene un propósito claro.

*Alta cohesión (bien):*
Un módulo `cartService.js` que solo sabe agregar, quitar y calcular el total del carrito. Todo lo que hace está relacionado con ese propósito.

### Progressive Disclosure

Es el principio de mostrar solo la información que el usuario necesita en el momento que la necesita, y revelar opciones adicionales cuando son relevantes.

En The Chill el ejemplo más claro es el formulario de checkout. El campo de "Dirección de entrega" solo aparece cuando el usuario selecciona "Entrega a domicilio". Si selecciona "Recoger en tienda", el campo no existe en el DOM (o está oculto con `display: none`, no solo `visibility: hidden`, para que no sea tabulable). Mostrar todos los campos siempre — incluyendo los que no aplican — es carga cognitiva innecesaria y viola progressive disclosure.

La implementación correcta en términos de accesibilidad: el campo de dirección tiene `hidden` cuando no aplica. Al seleccionar "Entrega a domicilio", se muestra y el foco se mueve al campo para que el usuario de teclado no tenga que buscarlo.

---

## 6. Historias de Usuario y MoSCoW

### Formato correcto de una historia de usuario

El formato estándar es:

> **Como** [tipo de usuario], **quiero** [acción], **para** [beneficio/objetivo].

La razón por la que importa el formato: el "para" es lo más importante y el más omitido. "Como usuario, quiero ver el precio de los granizados" no dice nada sobre por qué — y sin el porqué, no puedes evaluar si tu implementación resuelve el problema real. "Como cliente recurrente, quiero ver el precio antes de abrir el detalle del producto, para decidir rápidamente si está dentro de mi presupuesto" es una historia accionable que guía decisiones de diseño.

Los criterios de aceptación son las condiciones verificables bajo las cuales la historia se considera completa. Hay dos tipos que deben convivir:

**Criterios funcionales:** Describen qué hace el sistema.
- El precio del granizado se muestra en la tarjeta del catálogo.
- El precio incluye el símbolo de moneda y el formato correcto (ej: $8.000).

**Criterios de accesibilidad:** Describen cómo debe comportarse el sistema con tecnologías asistivas.
- La imagen del granizado tiene texto alternativo descriptivo.
- El precio es legible cuando el contraste de color se verifica con el fondo de la tarjeta (ratio mínimo 4.5:1).
- La tarjeta es activable por teclado usando Enter o Space.
- El nombre del granizado y el precio son anunciados por el lector de pantalla al enfocar la tarjeta.

Incluir criterios de accesibilidad en las historias de usuario no es opcional si WCAG AA es un requisito del producto. De lo contrario, la accesibilidad siempre queda para "después" y después nunca llega.

### MoSCoW para priorización

MoSCoW es un método de priorización de requisitos. El nombre es un acrónimo:

| Categoría | Significado | En The Chill |
|-----------|-------------|--------------|
| **Must Have** | Sin esto el producto no es lanzable. Funcionalidad crítica o requisito legal. | Catálogo de productos, carrito funcional, checkout con formulario válido, accesibilidad WCAG AA básica (4.1.2, 2.1.1, 1.4.3) |
| **Should Have** | Importante pero no bloqueante para el lanzamiento. Alta prioridad para el siguiente sprint. | Filtros por categoría, búsqueda de productos, mensajes de confirmación con aria-live |
| **Could Have** | Deseable si hay tiempo y recursos. No impacta negativamente si no está. | Historial de pedidos, favoritos, animaciones de transición entre vistas |
| **Won't Have** | Explícitamente fuera del alcance actual. No "no lo haremos", sino "no en esta versión". | Pago en línea integrado, sistema de reseñas, programa de lealtad |

El valor de hacer MoSCoW explícito es que fuerza conversaciones difíciles antes de empezar a construir, no durante. Si el cliente o el profesor no sabe que el historial de pedidos es un "Could Have", asumirá que está en el alcance y evaluará su ausencia como un defecto.

---

## 7. Wireframes y Prototipado

### Los tres niveles

**Wireframe en papel (lo-fi):** Bocetos a mano, sin color, sin fuente específica, sin imágenes reales. Cajas y líneas. El propósito es explorar la distribución de los elementos y el flujo de navegación sin invertir tiempo en detalles. Se pueden hacer 10 alternativas en una hora. El descarte es barato.

**Wireframe digital (mid-fi):** El mismo nivel de fidelidad bajo pero en herramienta digital (Figma, Balsamiq). Permite precisión en proporciones, facilita compartir y comentar en equipo, y se puede vincular para crear flujos navegables básicos. Los textos son representativos pero no definitivos. No hay colores de marca.

**Prototipo navegable (hi-fi):** Diseño con colores reales, tipografía definida, imágenes representativas, estados interactivos (hover, focus, error). Simula la experiencia real sin código. Es donde se validan las decisiones de diseño con usuarios antes de implementar.

### Por qué empezar en papel

El papel fuerza decisiones de estructura sobre decisiones de estética. Si diseñas directamente en Figma con colores y tipografía, el 80% de la conversación con el equipo y el cliente se va en "ese azul no es el correcto" en lugar de "¿tiene sentido que el filtro esté arriba del catálogo o al lado?".

El papel también es más honesto sobre la incertidumbre. Un wireframe en papel comunica "esto es una idea, no una decisión final". Un prototipo hi-fi comunica "esto está casi listo", lo que inhibe el feedback crítico.

### Decisiones de accesibilidad desde el wireframe

La accesibilidad no comienza en el código. Estas son decisiones que se toman en el wireframe:

- **Jerarquía de encabezados:** ¿El título de la página es un `h1`? ¿Las secciones tienen `h2`? Un wireframe con etiquetas de encabezado explícitas previene el uso de `<div>` como encabezados en el código.
- **Orden del DOM:** ¿El orden visual coincide con el orden lógico de lectura? Si el precio está visualmente a la derecha del nombre pero debería leerse después, el wireframe aclara esto.
- **Indicadores de foco:** El wireframe debe mostrar cómo se ve el foco en los elementos interactivos — no asumirlo.
- **Estados de error:** Los formularios del wireframe deben incluir el estado de error, no solo el estado normal. ¿Dónde aparece el mensaje de error? ¿Cómo de grande es?
- **Tamaño de objetivos táctiles:** En wireframes mobile, los botones e íconos deben tener el tamaño mínimo especificado (WCAG 2.5.8: 24×24px CSS).
- **Campos condicionales:** El wireframe debe mostrar ambos estados — con y sin el campo de dirección de entrega — para que quede claro que es un elemento dinámico, no un olvido.

---

## 8. Design Tokens y Design System

### Qué es un design token

Un design token es la unidad mínima de un design system: un nombre que representa una decisión de diseño. No es solo un valor — es un valor con significado y contexto.

La diferencia:
- `#22d3ee` es un valor de color. No dice nada sobre cuándo usarlo.
- `--color-brand-primary` es un token. Dice que es el color primario de marca. Cualquier cambio de marca actualiza ese token y se propaga a todo el sistema.

Los tokens se organizan en capas:

**Capa primitiva:** Los valores crudos. No se usan directamente en componentes.
```css
--cyan-500: #06b6d4;
--font-size-16: 1rem;
--space-4: 0.25rem;
```

**Capa semántica:** Tokens con significado de diseño. Son los que se usan en componentes.
```css
--color-interactive-default: var(--cyan-500);
--text-body-size: var(--font-size-16);
--component-padding-sm: var(--space-4);
```

**Capa de componente (opcional):** Tokens específicos de un componente.
```css
--button-background: var(--color-interactive-default);
--button-padding: var(--component-padding-sm) var(--component-padding-md);
```

### Relación con variables CSS

Las variables CSS (`custom properties`) son el mecanismo de implementación de los design tokens en la web. Los tokens son el concepto; las variables CSS son cómo se implementan.

La ventaja de las variables CSS sobre SCSS/LESS: son dinámicas en runtime. Esto permite theming sin recompilar — por ejemplo, cambiar a un tema de alto contraste modificando las variables en JavaScript o mediante una clase en el `<html>`.

### Implementación en The Chill

```css
/* tokens.css */
:root {
  /* Primitivos */
  --palette-cyan-600: #0891b2;
  --palette-orange-500: #f97316;
  --palette-neutral-50: #f9fafb;
  --palette-neutral-900: #111827;
  --palette-red-600: #dc2626;
  
  /* Semánticos - colores */
  --color-brand-primary: var(--palette-cyan-600);
  --color-brand-accent: var(--palette-orange-500);
  --color-surface-base: var(--palette-neutral-50);
  --color-text-primary: var(--palette-neutral-900);
  --color-feedback-error: var(--palette-red-600);
  
  /* Tipografía */
  --font-family-base: 'Inter', sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
  
  /* Espaciado (escala de 4px) */
  --space-1: 0.25rem;
  --space-2: 0.5rem;
  --space-4: 1rem;
  --space-6: 1.5rem;
  --space-8: 2rem;
  
  /* Radio de bordes */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-full: 9999px;
  
  /* Foco - accesibilidad */
  --focus-outline-color: var(--palette-cyan-600);
  --focus-outline-width: 3px;
  --focus-outline-offset: 2px;
}

/* Aplicación del foco consistente */
:focus-visible {
  outline: var(--focus-outline-width) solid var(--focus-outline-color);
  outline-offset: var(--focus-outline-offset);
}
```

El token `--focus-outline-color` garantiza que el indicador de foco sea visible y consistente en toda la aplicación — cumpliendo WCAG 2.4.7 y 2.4.11 desde una sola declaración.

---

## 9. Diagramas UML Relevantes

### Diagrama de Componentes

Un diagrama de componentes UML representa los componentes de software de un sistema y sus dependencias e interfaces. No muestra clases ni objetos — muestra módulos de software, qué interfaces exponen y cómo se conectan.

**Cuándo se usa:** Cuando necesitas comunicar la arquitectura de un sistema a nivel de módulos — qué existe, qué depende de qué, y cuáles son las fronteras entre partes del sistema. Es útil al inicio de un proyecto para acordar la estructura antes de implementar.

En The Chill, un diagrama de componentes mostraría:
- El componente `Router` que recibe URLs y decide qué vista renderizar.
- Los componentes de vista (`CatalogView`, `ProductDetailView`, `CartView`, `CheckoutView`).
- Componentes reutilizables (`ProductCard`, `CartItem`, `FormField`, `Button`).
- El `CartService` que maneja el estado del carrito.
- La conexión con la API externa de productos.

Las dependencias van de quien usa a quien es usado. `CatalogView` depende de `ProductCard` y de `CartService`. `ProductCard` no depende de nadie — es un componente hoja.

### Diagrama de Estados/Navegación

Un diagrama de estados UML (State Machine Diagram) representa los estados posibles de un sistema y las transiciones entre ellos. Un estado es una condición en la que el sistema puede existir. Una transición es un cambio de estado disparado por un evento.

**Diferencia con un flowchart:** Un flowchart representa un proceso o algoritmo — los pasos de una tarea. Un diagrama de estados representa el comportamiento reactivo de un sistema — cómo responde a eventos según en qué estado está. En un diagrama de estados, el mismo evento puede tener efectos diferentes dependiendo del estado actual.

En The Chill, el carrito puede modelarse como una máquina de estados:
- Estado `Vacío` → evento "agregar ítem" → Estado `Con ítems`
- Estado `Con ítems` → evento "eliminar todos" → Estado `Vacío`
- Estado `Con ítems` → evento "proceder al checkout" → Estado `En checkout`
- Estado `En checkout` → evento "confirmar pedido" → Estado `Pedido confirmado`
- Estado `En checkout` → evento "cancelar" → Estado `Con ítems`

Esto es más preciso que un flowchart porque captura que "agregar ítem" no tiene el mismo significado si el carrito está vacío (crea el carrito) que si ya tiene ítems (agrega uno más).

Para el modelo de navegación de la SPA, un diagrama de estados muestra qué vistas existen, qué interacciones del usuario disparan la transición entre vistas, y si hay estados intermedios (como un modal de confirmación).

### Diagrama de Actividades

Un diagrama de actividades UML es parecido a un flowchart pero con soporte para flujos paralelos (fork/join), particiones por actor (swimlanes) y excepciones. Representa el flujo de actividades para completar un proceso.

**Cuándo conviene sobre otros tipos:**
- Cuando el proceso involucra múltiples actores o sistemas que actúan en paralelo.
- Cuando hay ramas de decisión complejas.
- Cuando quieres modelar el flujo completo de una operación de negocio, no solo los estados del sistema.

En The Chill, el proceso de checkout se modela bien con un diagrama de actividades porque involucra al usuario (llena el formulario, confirma), al sistema (valida datos, calcula total), y potencialmente al servicio de pagos (procesa transacción). Un diagrama de estados del checkout no capturaría bien que el sistema puede estar procesando el pago (actividad asíncrona) mientras el usuario espera.

**Cuándo NO usar diagrama de actividades:** Para representar la estructura del software (usa componentes), para representar el comportamiento reactivo de un objeto (usa estados), para representar relaciones entre clases (usa clases). El diagrama de actividades modela procesos, no estructura.

---

*Este documento cubre los conceptos que serán evaluados en la actividad. Para cada concepto, el criterio de comprensión real no es la definición — es poder aplicarlo a una decisión concreta del proyecto The Chill.*

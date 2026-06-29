# Guía de Estilos — The Chill E-commerce Local

Esta guía define los elementos visuales principales del diseño de la SPA **The Chill E-commerce Local**. El objetivo es mantener una interfaz fresca, clara y fácil de usar en todas las pantallas.

## Paleta de colores

Los colores se eligieron pensando en bebidas frías y un estilo juvenil, con especial atención a que los textos sean legibles en cualquier contexto.

| Color            | HEX       | Uso                                                |
| ---------------- | --------- | -------------------------------------------------- |
| Azul principal   | `#0077B6` | Navbar, botones principales y acciones importantes |
| Azul oscuro      | `#005F8E` | Estados activos, hover o botones presionados       |
| Fondo claro      | `#F0F9FF` | Fondo general de las pantallas                     |
| Blanco           | `#FFFFFF` | Tarjetas, formularios y zonas de contenido         |
| Naranja          | `#CC4A00` | Precios, ofertas o elementos destacados            |
| Verde            | `#4A7C59` | Estados positivos o productos activos              |
| Rojo oscuro      | `#991B1B` | Errores o acciones de eliminación                  |
| Texto principal  | `#111827` | Títulos y contenido principal                      |
| Texto secundario | `#374151` | Descripciones y textos de apoyo                    |

Los estados importantes nunca se comunican solo con color. Siempre van acompañados de texto como "Activo", "Inactivo", "Disponible" o "Error".

## Tipografía

Se usa una fuente sin serifa, legible a distintos tamaños.

* Fuente principal: `Inter`, `Segoe UI`, Arial, sans-serif.
* Texto normal: mínimo `16px`.
* Títulos principales: entre `32px` y `40px`.
* Subtítulos: entre `20px` y `24px`.
* Texto secundario: `14px`, solo para información de apoyo.

## Espaciado

El diseño usa múltiplos de 8px para mantener orden visual y consistencia entre componentes.

* Espacio pequeño: `8px`.
* Espacio medio: `16px`.
* Separación entre secciones: `24px` o `32px`.
* Márgenes grandes: `48px` o `64px`.

## Componentes principales

La interfaz está construida con componentes reutilizables:

* Navbar con logo y carrito.
* Botones principales y secundarios.
* Cards de producto con imagen, nombre, precio y acción.
* Formulario de confirmación de pedido.
* Selector de tamaño y cantidad.
* Breadcrumbs para orientar al usuario.
* Tabla administrativa de productos.
* Mensajes de error, éxito o información.

## Estados de interacción

Cada botón y campo tiene estados visuales claros:

* **Default:** estado normal.
* **Hover:** cuando el usuario pasa el cursor.
* **Focus:** cuando navega con teclado.
* **Active:** cuando presiona el elemento.
* **Loading:** cuando se está procesando una acción.
* **Disabled:** cuando la acción no está disponible.

Ejemplo: el botón "Agregar al carrito" cambia a "Agregando…" mientras se procesa, para que el usuario sepa que algo está pasando.

## Accesibilidad

El diseño sigue algunas reglas básicas para funcionar bien para más gente:

* El contraste entre texto y fondo cumple con WCAG AA en todos los tamaños.
* Los estados (error, éxito, inactivo) siempre usan texto además de color.
* Los formularios tienen labels visibles, no solo placeholders.
* Los botones son lo suficientemente grandes para usarse en pantallas táctiles.
* Todo puede navegarse con teclado; el foco siempre es visible.
* Los errores se explican con texto claro, no solo con un borde rojo.
* Las imágenes importantes tienen texto alternativo descriptivo.

## Conclusión

Esta guía sirve de referencia para mantener el diseño coherente entre pantallas y facilitar la transición al código cuando llegue la etapa de implementación.

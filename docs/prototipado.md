# Prototipado en Papel — The Chill E-commerce Local

## Vistas diseñadas

Se dibujaron cinco vistas en papel, cubriendo el recorrido completo tanto del cliente como del administrador:

1. **Landing / Inicio:** logo del negocio, acceso al carrito, botón para ir al menú y una sección de productos destacados.
2. **Catálogo:** tarjetas de producto con filtros por categoría y acceso al detalle de cada ítem.
3. **Detalle y personalización:** selección de tamaño, cantidad, notas opcionales y botón para agregar al carrito con el total visible.
4. **Carrito / Confirmación:** resumen del pedido, datos del cliente, elección entre domicilio o recogida, y botón para confirmar.
5. **Panel administrativo:** tabla con los productos existentes, opción para agregar nuevos, editar o eliminar.

## Flujo principal

El recorrido pensado para el cliente es lineal y sin pasos innecesarios:

Inicio → Catálogo → Detalle del producto → Agregar al carrito → Confirmar pedido.

La idea es que alguien que entra por primera vez pueda hacer un pedido sin necesitar ayuda ni instrucciones adicionales.

## Consideraciones de accesibilidad

Desde el wireframe se tomaron algunas decisiones de accesibilidad para no tener que parcharlas después:

* Los botones tienen tamaño suficiente y etiqueta de texto, no solo íconos.
* Se incluye un breadcrumb para que el usuario sepa en qué parte del flujo está.
* Los campos del formulario tienen etiquetas visibles sobre el campo, no placeholders que desaparecen.
* Los mensajes de error aparecen junto al campo que los genera, no al fondo de la pantalla.
* Los estados importantes (disponible, no disponible, confirmado) se comunican con texto además del color.
* La navegación por teclado está contemplada en botones, formularios y tarjetas de producto.

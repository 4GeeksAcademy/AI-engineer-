# Mi Primer Proyecto Profesional Colaborativo 

Como estudiante principiante, este proyecto te enseñará a estructurar una aplicación web real combinando HTML semántico, Tailwind CSS, trabajo en equipo mediante Git y el uso responsable de la IA como asistente de programación.

## ⚠️ Regla de Oro Tecnológica (Restricción de Stack)

- Stack permitido: únicamente HTML y Tailwind CSS.
- Advertencia sobre la IA: como estás aprendiendo a dominar los fundamentos, prohíbe explícitamente a tu IA Copiloto utilizar frameworks avanzados como React, Vue, Next.js o JavaScript complejo.
- Cuando le pidas código a tu IA, indícale siempre: "Escribe este componente usando únicamente HTML plano y clases de Tailwind CSS".

## Clave de Éxito: Cómo Trabajar en Equipo con IA sin crear un "Frankenstein"

Si cada integrante del equipo usa la IA por su cuenta sin un criterio unificado, la web terminará pareciendo un collage desordenado. Para lograr un resultado homogéneo y profesional, utilicen exactamente esta plantilla de prompt sugerida con su IA antes de generar código:

### Prompt sugerido de Sistema

> "Estamos construyendo el prototipo web de una marca de ropa francesa elegante y moderna usando únicamente HTML y Tailwind CSS. Guíate estrictamente por las siguientes reglas de diseño para mantener la consistencia:
>
> - Paleta de colores: Primario: slate-900 (texto/botones principales), Secundario: amber-600 (destacados/ofertas), Fondo: stone-50, Bordes: stone-200.
> - Tipografía: usar sans-serif limpia (font-sans). Títulos en font-bold y texto base en text-slate-700.
> - Tarjetas de producto: bordes suaves (rounded-lg), sombra ligera (shadow-sm hover:shadow-md transition), padding interno de p-4.
> - Botones: primarios en bg-slate-900 text-white px-4 py-2 rounded-md hover:bg-slate-800. Secundarios con borde border border-slate-900 text-slate-900.
> - Componentes: NO uses JavaScript ni React. Todo debe ser HTML5 semántico puro con Tailwind CSS."

## Configuración Centralizada de Tailwind por CDN

Dado que usan HTML plano con el CDN de Tailwind, configuren un bloque `<script>` idéntico en el `<head>` de todos sus archivos HTML para compartir los mismos nombres y estilos:

```html
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    theme: {
      extend: {
        colors: {
          brandDark: '#0f172a',   // slate-900 equivalente
          brandAccent: '#d97706', // amber-600 equivalente
          brandBg: '#fafaf9',     // stone-50 equivalente
        }
      }
    }
  }
</script>
```

## Componentes Base Reutilizables

Definan juntos el diseño de la navbar, el footer y una tarjeta de producto base. Una vez aprobados por ambos, no los vuelvan a generar con IA; cópienlos y péguenlos idénticos en cada vista.

## 📑 Vistas que Debes Implementar (Y Enlazar Entre Sí)

Tu entrega consistirá en al menos 5 archivos HTML interconectados:

- Página de Inicio (`index.html`): navbar reutilizable, sección Hero, listados horizontales ("Nuevos lanzamientos" y "Más vendidos") y footer reutilizable.
- Catálogo (`catalog.html`): mantiene navbar y footer, barra de filtros por categoría/talla y cuadrícula de 4×5 productos (20 visibles).
- Detalle de Producto (`product.html`): diseño en dos columnas (foto grande a la izquierda; nombre, referencia, precio, tallas y botón de compra a la derecha) y sección de materiales/usos abajo.
- Carrito de Compras (`cart.html`): vista completa con listado de 3 productos de ejemplo, miniatura, precios, cantidades y cuadro de totalización con subtotal, impuestos y botón de compra.
- Formulario de Pago / Checkout (`checkout.html`): flujo guiado en 3 pasos: datos personales, dirección de envío y pago con tarjeta.

## 👥 Matriz de Tareas y Flujo de Git

Divídanse las páginas de forma lógica para no pisarse el trabajo:

- Desarrollador A: `index.html`, `catalog.html` y componentes globales base.
- Desarrollador B: `product.html`, `cart.html`, `checkout.html`.

### Flujo de Trabajo Profesional con Git

1. Configuración inicial: abre la plantilla base ([https://github.com/4GeeksAcademy/html-hello](https://github.com/4GeeksAcademy/html-hello)) en Codespaces o clónala, crea tu propio repositorio en GitHub, actualiza el remoto (`git remote set-url origin <tu-nueva-url>`) y sube la base.
2. Ramas independientes: nunca trabajen en la rama `main`. Cada quien cree su rama (por ejemplo: `git checkout -b feature/carrito`).
3. Commits ordenados: haz commits descriptivos frecuentes (por ejemplo: `git commit -m "feat: diseño responsive del carrito"`).
4. Pull Requests (PRs) de revisión cruzada: sube tu rama a GitHub y abre un PR. Tu compañero debe revisarlo obligatoriamente para verificar que mantenga los mismos estilos y clases de Tailwind acordados antes de hacer el merge a `main`.

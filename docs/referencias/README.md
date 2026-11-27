# Referencias de diseño

Esta carpeta contiene referencias visuales. Antes de modificar estilos, consultar
también [la guía de estilos](../guia-estilos.md). La guía es la fuente de verdad
para implementar y tiene prioridad sobre cualquier imagen o enlace.

## Uso de referencias

- Guardar imágenes nuevas aquí con nombres claros, por ejemplo:
  `tienda.png`, `formulario.png` o `resumen-movil.png`.
- Registrar debajo enlaces de Figma, Dribbble o sitios web utilizados como inspiración.
- Extraer únicamente colores, tipografía, espaciado, radios y patrones reutilizables.
- No copiar texto, logos, estructura HTML o JavaScript de una referencia.
- No cambiar `useState`, `useEffect`, `useReducer`, `useContext`, props, handlers,
  textos ni archivos de `src/context/`.

## Enlaces conocidos

- [Adobe Spectrum](https://spectrum.adobe.com/): principios generales de tokens,
  escala de espaciado y componentes. No usar como sustituto visual directo.

## Paleta activa

| Nombre práctico | Hex       | Uso válido |
| --------------- | --------- | ---------- |
| Crema | `#F2E8CF` | Fondo principal de la página |
| Crema clara | `#FFFBF2` | Superficie de tarjetas, paneles y formularios |
| Mostaza | `#E1AD01` | Acción principal y acento |
| Mostaza oscura | `#C79800` | Estado hover de la acción principal |
| Olivo | `#8A9A5B` | Éxito e información positiva, preferentemente en fondos/bordes |
| Olivo oscuro | `#5C6838` | Texto de éxito sobre superficies claras |
| Café | `#6B4226` | Texto secundario, bordes fuertes y foco |
| Carbón | `#2E2E2E` | Texto principal y títulos |
| Rojo | `#A4302A` | Acciones destructivas, como Eliminar |
| Blanco | `#FFFFFF` | Texto sobre el rojo |

## Mapeo obligatorio a tokens semánticos

| Función | Token semántico |
| ------- | --------------- |
| Fondo de página | `--color-fondo` |
| Tarjetas y superficies | `--color-superficie` |
| Texto principal | `--color-texto` |
| Texto secundario | `--color-texto-suave` |
| Acción principal | `--color-acento` |
| Hover principal | `--color-acento-hover` |
| Texto sobre acento | `--color-texto-sobre-acento` |
| Éxito/fondo positivo | `--color-exito` |
| Texto de éxito | `--color-exito-texto` |
| Acción destructiva | `--color-peligro` |
| Texto sobre peligro | `--color-texto-sobre-peligro` |
| Borde suave | `--color-borde` |
| Borde fuerte | `--color-borde-fuerte` |
| Foco visible | `--color-foco` |

Las reglas completas, incluidos tokens tipográficos, espaciado, radios, sombras,
estados interactivos y ejemplos, están en [docs/guia-estilos.md](../guia-estilos.md).

## Reglas de implementación para IA

- Usar solo tokens semánticos en componentes: por ejemplo
  `var(--color-texto)`, nunca `var(--carbon)` ni hexadecimales sueltos.
- Mantener la lógica React exactamente como está; el diseño puede cambiar clases y
  CSS, pero no puede mover o alterar estado, datos, props, eventos o textos.
- Solo crear un contenedor adicional si ayuda al layout y no obliga cambios lógicos.
- Usar clases BEM por componente y clases reutilizables `.btn`, `.input`, `.card`
  y variantes disponibles en `src/App.css`.
- Aplicar `:hover`, `:focus-visible` y `:disabled` a elementos interactivos.
- Usar espaciado móvil primero y múltiplos de 4px mediante los tokens `--espacio-*`.
- No usar estilos inline, `!important`, fuentes externas no aprobadas ni colores nuevos.
- Si se necesita un color o valor nuevo, agregarlo primero a `src/index.css` como
  token y documentarlo antes de usarlo.

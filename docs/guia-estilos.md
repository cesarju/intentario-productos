# Guía de estilos

Reglas para crear o modificar estilos en este proyecto (React + Vite, CSS plano).
Sirve para cualquier persona o asistente de IA.

**Diseño de referencia:** inspirado en los principios de [Adobe Spectrum](https://spectrum.adobe.com/)
(tokens en capas, escala de 4px, botones redondeados, foco visible) con una paleta
propia de tonos tierra. No se copian logos, fuentes ni componentes de Adobe:
solo se toman sus principios.

## 0. Contrato para asistentes de IA

Esta es la fuente de verdad visual del proyecto. Antes de tocar estilos:

1. Implementar **solo apariencia**: CSS y atributos `className`.
2. Preservar nombres de componentes, props, estados, reducers, contextos, eventos,
   handlers y textos existentes.
3. Usar únicamente los tokens semánticos descritos abajo. No introducir colores,
   tamaños, radios o sombras literales si existe un token equivalente.
4. No inventar una paleta nueva ni sustituir los tonos tierra sin una actualización
   explícita de este documento y de `src/index.css`.
5. No extraer decisiones visuales de un ejemplo aislado: seguir la jerarquía y las
   variantes documentadas.
6. Después del cambio, verificar móvil y escritorio, estados `hover`, `focus-visible`,
   `disabled`, contraste y ausencia de cambios en la lógica.

**Orden de precedencia:**

1. `docs/guia-estilos.md`
2. `src/index.css` (tokens existentes)
3. `src/App.css` (patrones reutilizables)
4. CSS propio del componente

Si esta guía, los tokens o un requisito gráfico entran en conflicto, detenerse y
preguntar en lugar de asumir o elegir una variante que cambie la lógica.

## 1. Alcance (obligatorio)

- Solo modificar archivos `.css`, atributos `className` y líneas `import './X.css'`.
- Se permite reemplazar estilos inline (`style={{...}}`) por clases CSS.
- Se permite agregar un `<div>`, `<section>` o `<span>` contenedor **solo** si es
  necesario para el diseño, sin mover ni cambiar la lógica.
- **NO** modificar: `useState`, `useEffect`, `useReducer`, `useContext`, `useRef`,
  `useMemo`, props, handlers (`onClick`, `onChange`), textos, ni la carpeta `src/context/`.
- Si un cambio de estilo requiere cambiar lógica, **detenerse y preguntar**.

## 2. Dónde va cada cosa

| Archivo                               | Contenido                                                                                                                      |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `src/index.css`                       | Tokens (`:root`), reset y estilos base de etiquetas (`body`, `h1`, `h2`, `button`, `input`).                                   |
| `src/App.css`                         | Solo clases **reutilizables** por 2 o más componentes (`.btn`, `.input`, `.card`, ver sección 5) y el layout general de `App`. |
| `src/components/NombreComponente.css` | Estilos propios de ese componente. Se importa en su `.jsx`: `import './ProductItem.css'`.                                      |

### ¿Dónde pongo una clase nueva?

1. ¿Ya existe una clase en `App.css` que sirve? → **Reutilizarla.**
2. ¿La usará solo este componente? → `NombreComponente.css`.
3. ¿La usarán varios componentes? → `App.css`.
4. Si una clase de componente empieza a repetirse en otro → **moverla a `App.css`**.

> Importante: en Vite, importar un `.css` en un componente **no lo aísla**; todas las
> clases son globales. Por eso usamos nombres BEM con el nombre del componente.

## 3. Tokens de diseño (en capas, como Spectrum)

Spectrum organiza los valores en capas. Aquí usamos dos:

1. **Primitivos**: la paleta cruda (`--crema`, `--mostaza`...). Dicen _qué color es_.
2. **Semánticos**: el uso (`--color-fondo`, `--color-acento`...). Dicen _para qué sirve_.

**Regla:** los componentes **solo usan tokens semánticos**, nunca primitivos.
Así, para cambiar el tema basta con reasignar los semánticos en `:root`.

### Paleta

| Primitivo   | Hex       | Rol principal                                         |
| ----------- | --------- | ----------------------------------------------------- |
| `--crema`   | `#F2E8CF` | Fondo de la página                                    |
| `--mostaza` | `#E1AD01` | Acento: acción principal (botón "Agregar", "Comprar") |
| `--olivo`   | `#8A9A5B` | Éxito / información positiva (solo fondos y bordes)   |
| `--cafe`    | `#6B4226` | Texto secundario, bordes de inputs, foco, hover       |
| `--carbon`  | `#2E2E2E` | Texto principal y títulos                             |

Colores derivados (necesarios para contraste o estados, fuera de la paleta base):

| Primitivo          | Hex       | Por qué existe                                                          |
| ------------------ | --------- | ----------------------------------------------------------------------- |
| `--crema-claro`    | `#FFFBF2` | Superficie de tarjetas sobre el fondo crema                             |
| `--mostaza-oscuro` | `#C79800` | Hover del botón acento                                                  |
| `--olivo-oscuro`   | `#5C6838` | Texto de éxito (el olivo base no tiene contraste suficiente como texto) |
| `--rojo`           | `#A4302A` | Acciones destructivas ("Eliminar"); la paleta no tiene rojo             |

### Contraste (WCAG AA: 4.5:1 texto normal, 3:1 texto grande y bordes)

| Combinación                         | Contraste | Uso                            |
| ----------------------------------- | --------- | ------------------------------ |
| `--carbon` sobre `--crema`          | 11.1:1 ✅ | Texto principal                |
| `--cafe` sobre `--crema`            | 7.1:1 ✅  | Texto secundario               |
| `--carbon` sobre `--mostaza`        | 6.6:1 ✅  | Texto de botón acento          |
| `--carbon` sobre `--mostaza-oscuro` | 5.1:1 ✅  | Hover del botón acento         |
| `--olivo-oscuro` sobre `--crema`    | 4.9:1 ✅  | Texto de éxito                 |
| blanco sobre `--rojo`               | 6.9:1 ✅  | Botón eliminar                 |
| blanco sobre `--mostaza`            | 2.3:1 ❌  | **No usar**                    |
| `--mostaza` sobre `--crema`         | 1.7:1 ❌  | **No usar como texto**         |
| `--carbon` sobre `--olivo`          | 4.4:1 ⚠️  | Solo texto grande o en negrita |

### Código de los tokens (`src/index.css`)

```css
:root {
  /* ===== 1. Primitivos (paleta) ===== */
  --crema: #f2e8cf;
  --crema-claro: #fffbf2;
  --mostaza: #e1ad01;
  --mostaza-oscuro: #c79800;
  --olivo: #8a9a5b;
  --olivo-oscuro: #5c6838;
  --cafe: #6b4226;
  --carbon: #2e2e2e;
  --rojo: #a4302a;
  --blanco: #ffffff;

  /* ===== 2. Semánticos (usar estos en los componentes) ===== */
  --color-fondo: var(--crema);
  --color-superficie: var(--crema-claro);
  --color-texto: var(--carbon);
  --color-texto-suave: var(--cafe);
  --color-acento: var(--mostaza);
  --color-acento-hover: var(--mostaza-oscuro);
  --color-texto-sobre-acento: var(--carbon);
  --color-exito: var(--olivo);
  --color-exito-texto: var(--olivo-oscuro);
  --color-peligro: var(--rojo);
  --color-texto-sobre-peligro: var(--blanco);
  --color-borde: rgb(107 66 38 / 0.2); /* café al 20%: tarjetas, separadores */
  --color-borde-fuerte: var(--cafe); /* inputs y botón secundario */
  --color-foco: var(--cafe);

  /* ===== Tipografía ===== */
  /* Spectrum usa Adobe Clean (licenciada). Alternativa libre: Source Sans 3. */
  --fuente:
    "Source Sans 3", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --texto-sm: 0.875rem; /* 14px */
  --texto-md: 1rem; /* 16px */
  --texto-lg: 1.25rem; /* 20px */
  --texto-xl: 1.75rem; /* 28px */
  --texto-2xl: 2.25rem; /* 36px */
  --peso-normal: 400;
  --peso-negrita: 700;
  --peso-titulo: 800;
  --interlineado: 1.5;
  --interlineado-titulo: 1.3;

  /* ===== Espaciado: múltiplos de 4px (como Spectrum) ===== */
  --espacio-xs: 0.25rem; /* 4px  */
  --espacio-sm: 0.5rem; /* 8px  */
  --espacio-md: 1rem; /* 16px */
  --espacio-lg: 1.5rem; /* 24px */
  --espacio-xl: 2rem; /* 32px */
  --espacio-2xl: 3rem; /* 48px */

  /* ===== Tamaño de controles (botones, inputs) ===== */
  --alto-control: 2.5rem; /* 40px */

  /* ===== Bordes, radios y sombras ===== */
  --borde: 1px;
  --borde-control: 2px;
  --radio-sm: 4px;
  --radio: 8px;
  --radio-lg: 16px;
  --radio-pill: 999px; /* botones redondeados tipo Spectrum */
  --sombra: 0 1px 4px rgb(46 46 46 / 0.12);
  --sombra-hover: 0 4px 12px rgb(46 46 46 / 0.16);
  --transicion: 130ms ease-in-out;
}
```

Si se necesita un valor nuevo, **primero agregarlo como token** y luego usarlo.

**Fuente (opcional):** para usar Source Sans 3 hay que cargarla en `index.html`
desde Google Fonts. Si no se carga, se usa la fuente del sistema automáticamente.

## 4. Nombres de clases (BEM)

- **Bloque** = nombre del componente en kebab-case: `.product-item`
- **Elemento** = parte del bloque: `.product-item__title`, `.product-item__price`
- **Modificador** = variante: `.product-item--destacado`, `.btn--peligro`

| Componente           | Bloque             |
| -------------------- | ------------------ |
| `ProductForms.jsx`   | `.product-form`    |
| `ProductList.jsx`    | `.product-list`    |
| `ProductItem.jsx`    | `.product-item`    |
| `ProductSummary.jsx` | `.product-summary` |

## 5. Componentes reutilizables (`src/App.css`)

Inspirados en los componentes de Spectrum. Se crean una vez y se reutilizan.

| Clase              | Aspecto                                                                           | Uso en el proyecto                 |
| ------------------ | --------------------------------------------------------------------------------- | ---------------------------------- |
| `.btn`             | Base: alto `--alto-control`, radio pill, negrita, transición                      | Todos los botones                  |
| `.btn--acento`     | Fondo mostaza, texto carbón                                                       | "Agregar", "Comprar"               |
| `.btn--secundario` | Fondo transparente, borde café, texto café                                        | "Mostrar/Ocultar", "+", "-"        |
| `.btn--peligro`    | Fondo rojo, texto blanco                                                          | "Eliminar"                         |
| `.input`           | Alto `--alto-control`, borde café 2px, radio `--radio`, fondo superficie          | Inputs del formulario              |
| `.card`            | Fondo superficie, borde suave, radio `--radio-lg`, sombra, padding `--espacio-lg` | Resumen, formulario, cada producto |
| `.badge--exito`    | Fondo olivo, texto carbón en negrita, radio pill                                  | Etiquetas como "En stock"          |

Reglas de estado (obligatorias en todo elemento interactivo):

- `:hover` → cambia a la variante "hover" del color o usa `--sombra-hover`.
- `:focus-visible` → `outline: 2px solid var(--color-foco); outline-offset: 2px;`
- `:disabled` → `opacity: 0.5; cursor: not-allowed;`

Ejemplo de base:

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--espacio-sm);
  min-height: var(--alto-control);
  padding: 0 var(--espacio-lg);
  border: var(--borde-control) solid transparent;
  border-radius: var(--radio-pill);
  font: inherit;
  font-weight: var(--peso-negrita);
  cursor: pointer;
  transition:
    background-color var(--transicion),
    box-shadow var(--transicion);
}

.btn:focus-visible {
  outline: 2px solid var(--color-foco);
  outline-offset: 2px;
}

.btn--acento {
  background-color: var(--color-acento);
  color: var(--color-texto-sobre-acento);
}

.btn--acento:hover {
  background-color: var(--color-acento-hover);
}
```

## 6. Reglas generales

- Sin estilos inline (`style={{...}}`).
- Mobile-first: estilos base para móvil y luego `@media (min-width: 768px)`.
- Layout con `flexbox` o `grid` y `gap`; no usar `float`.
- Espacios siempre en múltiplos de 4px (usar los tokens `--espacio-*`).
- Unidades: `rem` para texto y espacios; `px` solo para bordes y radios.
- Respetar la tabla de contraste de la sección 3.
- Mucho espacio en blanco y jerarquía clara: un solo botón acento por bloque.
- No usar `!important`.

## 7. Usar un diseño de referencia

Cuando se entregue una nueva referencia (captura, Figma, una web, etc.):

1. **Guardar la referencia** en `docs/referencias/` (ej: `docs/referencias/tienda.png`).
2. **Extraer los tokens** de la referencia: colores, fuente, tamaños, espaciados,
   radios y sombras. Actualizar **solo** los tokens de `:root` en `src/index.css`
   (primero los primitivos y luego, si hace falta, cómo se asignan los semánticos).
3. **Verificar el contraste** de las nuevas combinaciones y actualizar la tabla de la sección 3.
4. **Identificar patrones repetidos** (tarjetas, botones, inputs) y crearlos en `App.css`.
5. **Estilar cada componente** en su propio `.css` siguiendo la sección 2.
6. **No copiar** la estructura HTML de la referencia si implica cambiar lógica;
   adaptar el diseño al JSX existente (ver sección 1).
7. Al terminar, listar: tokens cambiados, archivos creados/modificados y clases nuevas.

## 8. Ejemplo

✅ Correcto

```jsx
// ProductItem.jsx
import "./ProductItem.css";
<div className="product-item card">
  <h2 className="product-item__title">{product.nombre}</h2>
  <button className="btn btn--peligro" onClick={() => handleDeleteItem(product.id)}>
    Eliminar
  </button>
```

```css
/* ProductItem.css */
.product-item {
  display: flex;
  align-items: center;
  gap: var(--espacio-md);
}
.product-item__title {
  font-size: var(--texto-lg);
  color: var(--color-texto);
}
```

❌ Incorrecto

```jsx
<div style={{ display: "flex", gap: "1rem" }}>
  <h2 style={{ color: "#2E2E2E", fontSize: 20 }}>{product.nombre}</h2>
```

```css
/* ❌ usa un primitivo y un hex suelto en vez de tokens semánticos */
.product-item__title {
  color: var(--carbon);
  background: #f2e8cf;
}
```

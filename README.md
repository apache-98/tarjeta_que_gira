# README — Tarjeta giratoria (Flip Card) con HTML y CSS

Este repositorio contiene un ejemplo simple y explicado paso a paso para crear una **tarjeta que gira en 3D** mostrando su parte frontal y trasera usando únicamente **HTML y CSS**. Ideal para usar en portfolios, cards de productos, o componentes UI.

---

## 📁 Estructura del proyecto

```
flip-card-css/
├─ index.html
├─ css/
│  └─ styles.css
└─ README.md
```

> Puedes adaptar la estructura a frameworks (React, Vue, Svelte) pero este README se centra en una implementación estática para comprender el efecto.

---

## 🔧 Archivos y capas (qué contiene cada archivo)

### `index.html`

Contiene el marcado HTML con las capas necesarias para el efecto:

* **`.flip-card`**: contenedor que añade *perspective* (profundidad) para la vista 3D.
* **`.flip-card-inner`**: elemento interno que rota (aquí aplicamos la transformación `transform` y la transición `transition`).
* **`.flip-card-front`** y **`.flip-card-back`**: las dos caras de la tarjeta. La trasera está rotada 180° para aparecer correctamente cuando el `inner` gira.

Ejemplo mínimo de `index.html`:

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Flip Card CSS</title>
  <link rel="stylesheet" href="css/styles.css" />
</head>
<body>
  <main>
    <div class="flip-card">
      <div class="flip-card-inner">
        <div class="flip-card-front">
          <!-- Contenido frontal -->
          <h2>Frente</h2>
          <p>Información breve</p>
        </div>
        <div class="flip-card-back">
          <!-- Contenido trasero -->
          <h2>Reverso</h2>
          <p>Más detalles aquí</p>
        </div>
      </div>
    </div>
  </main>
</body>
</html>
```

### `css/styles.css`

Contiene las reglas que crean la ilusión 3D y controlan cómo se muestra cada cara.

Ejemplo de `styles.css` comentado:

```css
/* Contenedor que define la perspectiva 3D */
.flip-card {
  width: 300px;
  height: 200px;
  perspective: 1000px; /* clave para ver el efecto 3D */
  margin: 40px auto;
}

/* Capa que gira. Aquí aplicamos la transición y el estilo 3D */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transition: transform 0.8s; /* duración del giro */
  transform-style: preserve-3d; /* mantener las caras en 3D */
}

/* Gira la tarjeta en el eje Y al hacer hover */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Estilos compartidos por ambas caras */
.flip-card-front,
.flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden; /* oculta la cara trasera cuando está girada */
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.15);
}

/* Cara frontal */
.flip-card-front {
  background: linear-gradient(135deg,#1e88e5,#42a5f5);
  color: white;
}

/* Cara trasera: rotada 180° para aparecer correctamente */
.flip-card-back {
  background: linear-gradient(135deg,#e53935,#ef5350);
  color: white;
  transform: rotateY(180deg);
}
```

---

## 🧭 Explicación de las propiedades clave

* `perspective`: da la profundidad 3D. Valores más bajos = efecto más pronunciado.
* `transform-style: preserve-3d`: mantiene a los hijos en espacio 3D para que ambas caras se rendericen correctamente.
* `backface-visibility: hidden`: evita que la cara trasera (o frontal) se vea cuando está orientada hacia atrás.
* `transform: rotateY(180deg)`: rota en el eje Y para el efecto de volteo. También podrías usar `rotateX` para volteo vertical.
* `transition: transform 0.8s`: suaviza la animación. Ajusta duración y `timing-function` a tu gusto.

---

## ⚙️ Variantes comunes

1. **Giro al hacer clic (mobile-friendly)**

   * Añade una clase `.is-flipped` en JS sobre `.flip-card-inner` al hacer clic, en vez de usar `:hover`.

   Ejemplo básico en JS:

   ```js
   const card = document.querySelector('.flip-card');
   card.addEventListener('click', () => {
     card.querySelector('.flip-card-inner').classList.toggle('is-flipped');
   });
   ```

   Y en CSS reemplazar el selector `:hover` por:

   ```css
   .flip-card-inner.is-flipped { transform: rotateY(180deg); }
   ```

2. **Giro automático (loop)**

   * Usa `@keyframes` para animarlo continuamente.

   ```css
   @keyframes autoFlip {
     0%, 49% { transform: rotateY(0deg); }
     50%, 100% { transform: rotateY(180deg); }
   }
   .flip-card-inner { animation: autoFlip 6s infinite; }
   ```

3. **Soporte para varias tarjetas**

   * Envuelve cada tarjeta en un grid y aplica el mismo patrón por tarjeta.

---

## ✅ Accesibilidad y buenas prácticas

* Asegúrate de que el contenido de la **parte trasera** sea accesible por teclado (por ejemplo, permite activar el flip con Enter/Space cuando la tarjeta tenga `tabindex="0"`).
* Si usas la variante con `:hover`, provee también la variante por `click` para dispositivos táctiles.
* Evita poner información crítica solo en la parte trasera (o duplica información clave) porque algunos lectores o usuarios pueden no activar el giro.
* Añade `aria-pressed` o estados ARIA si el giro representa una acción.

Ejemplo sencillo para keyboard + ARIA:

```html
<div class="flip-card" tabindex="0" role="button" aria-pressed="false">
  <!-- ... -->
</div>
```

Y en JS actualizar `aria-pressed` cuando se haga click o keydown.

---

## 📦 Comandos útiles (crear repo, ver en local, desplegar)

### Inicializar repositorio Git y subir a GitHub

```bash
# en la carpeta del proyecto
git init
git add .
git commit -m "Add flip card example"
# crea el repo en GitHub y conecta remoto (reemplaza URL)
git remote add origin git@github.com:TU_USUARIO/flip-card-css.git
git branch -M main
git push -u origin main
```

> Si no sabes crear el repo remoto desde CLI, puedes crearlo desde la web de GitHub y luego copiar el `git remote add` que te da la UI.

### Servirlo localmente (opciones)

* Usando `live-server` (instalación global con npm):

```bash
npm install -g live-server
live-server
```

* Usando `serve`:

```bash
npm install -g serve
serve .
```

* Abrir `index.html` directamente en el navegador (funciona, pero algunas funciones relacionadas con rutas o fetch no funcionarán si las hicieras).

### Publicar en GitHub Pages

1. En la configuración del repo (Settings > Pages) elige la rama `main` y la carpeta `/root` o la carpeta `docs/` si la usas.
2. Guarda y GitHub generará la URL `https://TU_USUARIO.github.io/flip-card-css/` en unos minutos.

---

## 🔍 Depuración & problemas comunes

* **No se ve el efecto 3D**: revisa que el padre tenga `perspective` y el hijo `transform-style: preserve-3d`.
* **Se ve texto al revés**: asegúrate de `backface-visibility: hidden` en ambas caras.
* **La tarjeta no gira en móvil**: `:hover` no funciona en touch — usa `click` + JS.
* **Saltos raros en la animación**: revisar `position` y `display`; mejor usar `position: absolute` en las caras y `position: relative` en el contenedor interno.

---

## 🎨 Personalización rápida

* Cambia `perspective` para más o menos profundidad.
* Ajusta `transition` para velocidad o `cubic-bezier` para curvas personalizadas.
* Añade sombras, gradientes, imágenes de fondo o iconos.

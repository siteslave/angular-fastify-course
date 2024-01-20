# Add `TailwindCSS`

```shell
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

Edit file `tailwind.config.js`

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{html,ts}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Edit file `src/style.css`
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

# Add `daisyUI`

```shell
npm i -D daisyui@latest
```

Edit file `tailwind.config.js`

```javascript
module.exports = {
  //...
  plugins: [require("daisyui")],
}
```

Testing

```html
<button
  class="inline-block cursor-pointer rounded-md bg-gray-800 px-4 py-3 text-center text-sm font-semibold uppercase text-white transition duration-200 ease-in-out hover:bg-gray-900">
  Button
</button>
```

# Theme

Edit file `tailwind.config.js`
```javascript
module.exports = {
  //...
  daisyui: {
    themes: ["light", "dark", "cupcake"],
  },
}
```

Edit file `index.html`
```html
<html data-theme="cupcake"></html>
```

Edit file `app.component.html`

```html
<!-- https://daisyui.com/components/hero/ -->

<div class="hero min-h-screen bg-base-200">
    <div class="hero-content text-center">
        <div class="max-w-md">
            <h1 class="text-5xl font-bold">Hello there</h1>
            <p class="py-6">
                Provident cupiditate voluptatem et in. Quaerat fugiat ut
                assumenda excepturi exercitationem quasi. In deleniti eaque aut
                repudiandae et a id nisi.
            </p>
            <button class="btn btn-primary">Get Started</button>
        </div>
    </div>
</div>

```

Link to daisyUI theme https://daisyui.com/docs/themes/
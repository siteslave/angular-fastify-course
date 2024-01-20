# Add font

File `index.html`

```html
<head>
   <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Kanit:wght@300&display=swap"
      rel="stylesheet"
    />
</head>
```

file `style.css`

```css
@layer base {
    html {
        font-family: "Kanit", sans-serif;
        font-size: 1rem;
    }
}
```
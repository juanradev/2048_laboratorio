## Module 6: Styling HTML5 by Using CSS3


#### Lab: Styling Text and Block Elements by Using CSS3

1. Exercise 1: Styling the Navigation Bar

en cada html hemos añadido la clase active a cada elemento <a> "seleccionado" de cada página

```html
<nav class="page-nav">
    <div class="container">
        <a href="/index.htm" class="active">Home</a>
        <a href="/about.htm">About</a>
        <a href="/schedule.htm">Schedule</a>
        <a href="/register.htm">Register</a>
    </div>
</nav>
```


Modificacion del fichero /styles/nav.css
modifca nav con la clase page-nav  background-color: negro , 6rem de de ancho y tamaño de fuente
```css
  nav.page-nav {
      background-color: #1d1d1d;
      line-height: 6rem;
      font-size: 1.7rem;
  }
````
 
 div container display flexible
```css
    nav.page-nav .container {
        display: -ms-flexbox;
        display: flex;
    }
````

nav.page-nav a
```css
nav.page-nav a {
    display: block;
    min-width: 9rem;
    padding: 0 1.8rem; /* padding-top padding-right padding-bottom padding-left*/
    border-right: 1px dotted #3d3d3d;
    text-decoration: none; /*text-decoration: overline   line-through  underline  none; */
    text-transform: uppercase;
    text-align: center;
    color: #c3c3c3;
    text-shadow: 0 1px 0 #000;   /* text-shadow: h-shadow v-shadow blur-radius color*/ 
}
````


```css
    nav.page-nav a:first-child {
        border-left: 1px dotted #3d3d3d;
    }
````

el hover lo ponenos en negro
```css
nav.page-nav a:hover {
    color: #e4e4e4;
    background-color: black;
}
````

la activa
```css
nav.page-nav .active {
    color: #fff;
    background: -ms-linear-gradient(#c95656, #8d0606);
    background: linear-gradient(#c95656, #8d0606);
}
````

sobreescribimos hover para la activa
```css
nav.page-nav .active:hover {
    /* Override hover effect for active page link */
    color: #fff;
}
````
y añadimos las esquinas para la activa

```css
    nav.page-nav .active:before {
        position: absolute;
        top: 6rem;
        display: block;
        height: 0;
        width: 0;
        margin-left: -1.9rem;
        border-top: 2rem solid #8d0606;
        border-right: 6.5rem solid transparent;
        content: "";
    }
````


```css
    nav.page-nav .active:after {
        position: absolute;
        display: block;
        height: 0;
        width: 0;
        margin-left: 4.3rem;
        border-top: 2rem solid #8d0606;
        border-left: 6.5rem solid transparent;
        content: "";
    }
````




2. Exercise 2: Styling the Register Link

Modificamos /styles/header.css

el html es el siguiente
```html
<header class="page-header">
    <div class="container">
        <h1>ContosoConf</h1>
        <p class="tag-line">A two-track conference on the latest HTML5      developments</p>

        <a class="register" href="/register.htm">
            Register<br />
            <span class="free">&#183; Free &#183;</span>                
        </a>
    </div>
</header>
```

con ese css ponemos el link en la posicion concreta y modificamos el color y la sombra
```css
 header.page-header .register {
      display: block;
      position: absolute;
      top: 2rem;
      right: 3.5rem;
      width: 16rem;
      height: 10rem;
      padding-top: 6rem;
      font-size: 2.7rem;
      text-align: center;
      text-decoration: none;
      text-transform: uppercase;
      text-shadow: 0 1px 0 #000;
      color: #fff;
  }
```

y añadimos para que sea un circulo
```css
      background: -ms-linear-gradient(#a80000, #740404);
      background: linear-gradient(#a80000, #740404);
      -ms-border-radius: 100%;
      border-radius: 100%;
      -ms-transform: rotate(6deg);
      transform: rotate(6deg);
```

el hover para que el color background sea otro degradado
```csss
  header.page-header .register:hover {
      background: -ms-linear-gradient(#bc0101, #8c0909);
      background: linear-gradient(#bc0101, #8c0909);
  }
```

Para poner el dotted en el circulo hacemos un div con  border: 3px dotted #740404;
y le hacemos circulo border-radius: 100%;
```csss
header.page-header .register:before {
    display: block;
    position: absolute;
    top: -.7rem;
    right: -.7rem;
    height: 16.8rem;
    width: 16.8rem;
    content: "";
    border: 3px dotted #740404;
    -ms-border-radius: 100%;
    border-radius: 100%;
}
```

3. Exercise 3: Styling the About Page

Por último modificamos el about.html añadiendo css de text en /styles/pages/about.css


```csss
/* selecciona todos los   <section>  donde el parent es un <article>    donde el parent es una clase about */
tres columnas , rem de separacion y justificado
.about > article > section {
    column-count: 3;
    column-gap: 5rem;
    text-align: justify;
}
```

Modificamos el estilo de la primera linea
```csss
.about p:first-child:first-letter {
    font-size: 300%;
    float: left;
    margin: 0 0.5rem 0 0;
    line-height: .8;
    color: #aaa;
}
```

Identamos 
```csss
.about p {
    text-indent: 3rem;
}
```
Pero sobreescrimos para el primer parrafo
```csss
.about p:first-child {
    /* Prevents text indenting after drop cap */
    text-indent: 0;
    margin-top: 0;
}
```

y modficamos el blockquote
```csss

  .about blockquote {
      font-size: 1.2em;
      padding: 0 0 0 6rem;
      margin: 0;
      font-style: italic;
      position: relative;
  }
}
```
añadiendo delante las comillas 
```csss

  .about blockquote:before {
      content: '\201C';
      position: absolute;
      font-size: 10rem;
      font-family: serif;
      left: 0;
      top: -1rem;
      line-height: 1;
  }
}
```
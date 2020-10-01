### Laboratorio: Implementación de una interfaz de usuario adaptable

Modificar el estilo de about.html al imprimir

una vez comprobada la impresión queremos eliminar de ella

el menu de navegacion, el footer, el header y hacerlo en una unica columna

Para ello creamos la hoja de estilo print.css que despues añadiremos a about.html

_about.html_
```html 
 <nav class="page-nav"><div class="container">
 <header class="page-header"><div class="container">
   <section class="page-section about">
            <article class="container">

 <footer class="page-footer"><div class="container">
```

modificamos entonces print.css
```css
nav.page-nav,
header.page-header,
footer.page-footer {
    display: none;
}

.container {
    padding: 0;          // mo paddin no max-width
    max-width: none;     // al final solo afecta al texto pues los otros elementos ya estan a node
}


.about > article > section {
    column-count: 1;    /// una unica columna 
}
````

y por aultimo lo añadimos al html con el tag media="print"
```html
<link href="styles/print.css" rel="stylesheet" type="text/css" media="print"/>
```


Por úlimo vamos a adaptar el menu y el botón de registro según el tamaño de la pantalla
Como no se porque no puedo reducir a menos de 500px modifico la condicion a 
@media screen and (max-width: 500px) 
```csss
/* Styles for smaller browsers e.g. phones and tablets */

@media screen and (max-width: 960px) {
    
    nav.page-nav .container {
        display: -ms-flexbox;
        display: flex;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
        -ms-flex-pack: center;
        flex-pack: center;
    }
    
    nav.page-nav a {
        border: 1px dotted #3D3D3D;
        margin: .5rem;
        padding: 0 .8rem;
    }
    
    nav.page-nav .active:before,
    nav.page-nav .active:after {
        display: none;
    }
    
}

@media screen and (max-width: 500px) {
    nav.page-nav .container {
       
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    nav.page-nav .active:before,
    nav.page-nav .active:after {
        display: none;
    }

    nav.page-nav a {
        border: 1px dotted #3d3d3d;
        margin: .5rem;
    }
}

@media screen and (max-width: 720px) {
    header.page-header {
        height: auto;
    }

        header.page-header .register {
            display: none;
        }

        header.page-header h1 {
            font-size: 3rem;
        }
}
'''
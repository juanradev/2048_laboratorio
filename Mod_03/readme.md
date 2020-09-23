## Laboratorio Modulo 03 Introducción a JavaScript

##### Alumno: Juan Ramón Díaz Fernández
###### fecha 23/09/2017
###### dificultad: ninguna

###### tips:
1. para implentar la condicion si pertenece a una pista utilizar arraytraks.indexOf(1) >= 0 
2. recordar addEventListener  track1CheckBox.addEventListener("click", displaySchedule, false);
3. textContent vs createTextNode
```javascript
    li= document.createElement("li");
    li.textContent = session.title;
    var h = document.createElement("H1");
    var t = document.createTextNode("Hello World");
    h.appendChild(t); 
```



Partimos de la solución de  Allfiles\Mod03\Labfiles\Starter\Exercise 1 básicamente es la misma que la subida del Mod02
a la que se le ha añadido la página schedule.htm que sólo coniene el nav, el header y el footer
con respecto a las páginas about e index además de añadir el link a schedule.htm han añadido clases tipo class="page-footer", etc..
asi como han encapsulado el contenido en diferentes div class="container" (recordar la regla css)
con lo cual dan una apariencia más homogenea
``` css
.container {
    padding: 0 1rem;
    max-width: 94rem;
    /* Horizontally center containers */
    margin: 0 auto;
}
```

#### Ejercicio 1: Displaying Data Programmatically
1 Modificar la pagina schedule añadiendo una ul que será rellena de forma automatica cuando se carga la página a partir de un array de objetos que se encuentra en el fichero scripts/pages/schedule.js 
este array tiene la forma siguiente
```javascript
const schedule = [
      {
          "id": "session-1",
          "title": "Registration",
          "tracks": [1, 2] /* es una lista de las traks a las que pertenece , en este caso a las dos listas */
      },...
     {
          "id": "session-3",
          "title": "Diving in at the deep end with Canvas",
          "tracks": [1]  /*observa que si solo pertenece a la pista 1 solo tiene el elemento 1*
      }, ....
      {
         "id": "session-4",
         "title": "New Technologies in Enterprise",
         "tracks": [2]  /*observa que si solo pertenece a la pista 2 solo tiene el elemento 2*
     } ...
]
````

Para identificar mejor los elementos  a este array lo voy a renombra con const array_schedule
y a la ul del DOM <ul id="ul_schedule"></ul>

el codigo seria el siguiente
```javascript
 const list = document.getElementById("ul_schedule"); /* con la constante list identifico a la ul del dom */

function createSessionElement(session) {
    // TODO: Task 3 - Create a <li> element for the session.
    //       Add the session title as the <li> text content
    //       Return the <li> element
    const li = document.createElement("li");
    li.textContent = session.title;
    return li;
};  /* esta funcion devuelve un elemento li creado 
     -- le pasamos como paramentro session y le añadimos en el textContent la propiedad title 
     como existe no da un error pero me sorprende que intellengece me permita seleccionarlo
  */


function clearList() {
    while (list.firstChild) {
        list.removeChild(list.firstChild);
    }
} /* esta función ya estaba implementada lo que hace es ir eleminando uno a uno todos los elementos de la lista */

function displaySchedule() {
    clearList();
    
}
    // TODO: Task 4 - Loop through the schedule array
    //       Create session elements
    //       Append the elements to the list  

function displaySchedule() {
    clearList();
    
    // TODO: Task 4 - Loop through the schedule array
    //       Create session elements
    //       Append the elements to the list  
    for (let i = 0; i < array_schedule.length; i++)
        list.appendChild(createSessionElement(array_schedule[i]));
    }  /** borramos la lista y recorremos el array añadiendo el elemento que nos creamos con createSessionElement 
       a la que pasamos como parametro el elemento en el que estamos del array */


displaySchedule(); // el script finaliza con una llamada a esta funcion



/* por lo que solo quedaría añadir el js al final del body de la pagina htm
<script src="/scripts/pages/schedule.js" type="text/javascript"></script>
 */
```
#### Ejercicio 2:  Handling Events

se trata de añadir dos checkbox Track1 y trak2 de tal manera que solo se muestren las sessiones del track1 si este check esta seleccionado y del trak2 si esta seleccionado el 2

Añadiremos los checkbox
Como ambos estan seleccionados deberan aparecer todas las sessiones cuando carguemos la página
```html
<input type="checkbox" id="show-track-1" checked="checked"/><label      for="show-track-1">Track 1</label>
<input type="checkbox" id="show-track-2" checked="checked"/><label      for="show-track-2">Track 2</label>
````
En el js deberemos referenciar cada uno con un getelementid
```javascript
const track1CheckBox = document.getElementById("show-track-1");
.........
```
Le añadimos el escuchador de eventos  con addEventListener para que cuando haga click llame a la funcion displaySchedule
```javascript
 track1CheckBox.addEventListener("click", displaySchedule, false);
.........
```

y modificamos la funcion displaySchedule  para que despues de borrar
recorra el array y solo pinte la session si pertenece a la pista1 y check1 esta seleccionado ó si pertenece a la psita2 y chek2 esta seleccionado
```javascript
function displaySchedule() {
    clearList();
    
    // TODO: Task 4 - Loop through the schedule array
    //       Create session elements
    //       Append the elements to the list  pero añadimos la condicion de las pistas
    
    for (let i = 0; i < array_schedule.length; i++) {
        const pistasdelasesion = array_schedule[i].tracks;
        if ((pistasdelasesion.indexOf(1) >= 0 && track1CheckBox.checked) || (pistasdelasesion.indexOf(2) >= 0 && track2CheckBox.checked) )
                list.appendChild(createSessionElement(array_schedule[i]));
    }
}
```
recuerda que el objeto 
tiene la propiedad tracks 
"tracks": [1, 2] /* es una lista de las traks a las que pertenece , en este caso a las dos listas */
"tracks": [1]  /*observa que si solo pertenece a la pista 1 solo tiene el elemento 1*
"tracks": [2]  /*observa que si solo pertenece a la pista 2 solo tiene el elemento 2*

por la tanto para saber si esta en la pista 1 
const pistasdelasesion = array_schedule[i].tracks;
pistasdelasesion.indexOf(1) >= 0 

y para saber si esta en la pista 2 
pistasdelasesion.indexOf(1) >= 0 
despues ya añadir and con su checkbox.checked en el  or 



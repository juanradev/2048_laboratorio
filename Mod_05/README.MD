## LABORATORIO Module 5: Communicating with a Remote Server


La práctica consiste en refactorizar la pagina schedule modificando el archivo schedule.js de tal manera que la carga de la lista de secciones se haga llamando a un servidor

El controlador ScheduleController devolverá el siguiente array de objetos

```javasript
 schedule = new[]
            {
                new ScheduleItem
                {
                    id = "session-1",
                    title = "Registration",
                    subTitle = "Get your name badge and goody bag",
                    speakerName = null,
                    start = "08:30",
                    end = "08:55",
                    tracks = new[] {1, 2},
                    room = "A",
                    starCount = RandomStarCount()
                },
````

#### tarea 1 GET a traves del XMLHttpRequest

llenaremos la lista mediante la siguiente función

```javascript
function downloadSchedule() {
    const request = new XMLHttpRequest();
    request.open("GET", "/schedule/list", true); 
    request.onreadystatechange = function () {
        if (request.readyState === 4) {
            const response = JSON.parse(request.responseText);
            response.schedule.forEach(function (element) {
                schedule.push(element);
            });
            displaySchedule();
        }
    };
    request.send();
}
```
https://www.w3schools.com/js/js_ajax_http.asp


El formato de XMLHttpRequest es sencillo


```javascript
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("demo").innerHTML = this.responseText;
    }
  };
  xhttp.open("GET", "ajax_info.txt", true);
  xhttp.send();
``` 
creamos el XMLHttpRequest asignadoselo a la constante request
request.open(metodo (POST,GET;...),url, [async (false indica que debe esperar la respuesta],[user], [password]); 

  if (request.readyState == 4 && request.status == 200)

0	UNSENT	Se ha creado el cliente. open()aún no llamado.
1	OPENED	open() ha sido llamado.
2	HEADERS_RECEIVED	send() se ha llamado y los encabezados y el estado están disponibles.
3	LOADING	Descargando; responseTextcontiene datos parciales.
4	DONE	La operación está completa.

request.status

Respuestas informativas (100–199),
Respuestas satisfactorias (200–299),
Redirecciones (300–399),
Errores de los clientes (400–499),
y errores de los servidores (500–599).




En nuestro caso convertimos la respuesta a un json y la pasamos al nuestra lista de sesiones
```javascript
const response = JSON.parse(request.responseText);  la respuesta la parsea a JSON


response.schedule.forEach(function (element) {
                schedule.push(element);
            }); 

        /* o  
for (i = 0; i < response.schedule.length; i++)
schedule.push(response.schedule[i]);
*/
``` 

al llamar a  displaySchedule(); mostramos la lista


la siguiente tarea es controlar que pueda fallarnos la petición

```javascript
function downloadSchedule() {
            const request = new XMLHttpRequest();
            request.open("GET", "/schedule/list", true);
            request.onreadystatechange = function () {
                if (request.readyState === 4) {
/* metemos el codigo en un try/catch
controlamos que request.status === 200 en caso  alert(response.message);
esto no es un fallo sino que puede ser 400 url no encontrada etc
pero el catch  es otro alert("Schedule list not available.");
*/*

                    try {
                        const response = JSON.parse(request.responseText);
                        if (request.status === 200) {
                            response.schedule.forEach(function (element) {
                                schedule.push(element);
                            });
                            displaySchedule();
                        } else {
                            alert(response.message);
                        }
                    } catch (exception) {
                        alert("Schedule list not available.");
                    }
                }
            };
            request.send();
        }

```

Probamos el if con
 request.open("GET", "/schedule/list?fail", true);
ya que elcontrolador devuelve
  Response.StatusCode = 503;
   return Json(new { message = "Service currently unavailable." }, JsonRequestBehavior.AllowGet);



#### tarea 2 POST a traves del XMLHttpRequest

```javascript

function saveStar(sessionId, isStarred) {
    // TODO: Create an XMLHttpRequest that POSTs to "/schedule/star/{sessionId}"
    //       The request body must have the content type "application/x-www-form-urlencoded"
    //       e.g. "starred=true" or "starred=false"
    //       The response contains a JSON object "{ starCount: <number> }"
    //       If the star count is more than 50, warn the user about this being a busy session.
    const request = new XMLHttpRequest();
    request.open("POST", "/schedule/star/" + sessionId, true);
    if (isStarred) {
        request.onreadystatechange = function () {
            if (request.readyState === 4 && request.status === 200) {
                const response = JSON.parse(request.responseText);
                if (response.starCount > 50) {
                    alert("This session is very popular! Be sure to arrive early to get a       seat.");
                }
            }
        };
    }

    request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    const data = "starred=" + isStarred;
    console.log(data);
    request.send(data);


}
````


el formato de un post tambien es senccillo
```javascript
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("demo").innerHTML = this.responseText;
    }
  };
  
  xhttp.open("POST", "demo_post2.asp", true);
  xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
  xhttp.send("fname=Henry&lname=Ford");
}
```


open (POST, ulr, [async], [user], [password])
setRequestHeader(header, value)	Adds HTTP headers to the request
send (data)  data es una cadena con el fromato que nos pida la api en el ejemplo "starred=" + isStarred;

/* el idssesion se lo paso en "/schedule/star/" + sessionId*/


y tambien me devuelve una respuesta 





fuera del envio de datos esta el tratamiento de las stars


el html es el siguiente

```html 
 <ul id="schedule">
<li class="started">
<a href="#" class="start">
<span>titulo</span>
</li>
</ul>
```

```javascript

function handleListClick(event) {
    const isStarElement = event.srcElement.classList.contains("star");
    if (isStarElement) {
        event.preventDefault(); // Stop the browser following the clicked <a> element's href.

        const listItem = event.srcElement.parentNode;
        if (listItem.classList.contains("starred")) {
            listItem.classList.remove("starred");
            saveStar(listItem.sessionId, false);
        } else {
            listItem.classList.add("starred");
            saveStar(listItem.sessionId, true);
        }
    }
}

const list = document.getElementById("schedule");
list.addEventListener("click", handleListClick, false);
```

lo que hace es cuando hacemos click en la lista 
guardamos en isStarElement el elemento <a> (revisa el html>)
const isStarElement = event.srcElement.classList.contains("star");
event.preventDefault(); // Stop the browser following the clicked <a> element's href.
 const listItem = event.srcElement.parentNode; /* ahora el padre el elemento li*
si contiene la clase "starred"
la quita y hace el post con false
si no
la pone y hace el post con true


Pero va más allá el css 
la imagen son tres estrellas verticuales de diferente color y de 15 * 45 cada una tiene un heigth de 15*
cuando se carga esta en  background-position: 0 0;


en el hover     background-position: 0 -15px;

pero su esta started (anulando tambien el hover)

 background-position: 0 -30px;


```css
/* Style for the schedule page */

.star {
    display: inline-block;
    width: 15px;
    height: 15px;
    cursor: pointer;
    background-image: url(../images/stars.png);
    background-position: 0 0;
}

.star:hover {
    background-position: 0 -15px;
}

.starred .star,
.starred .star:hover {
    background-position: 0 -30px;
}

```




#### tarea 3 fecth 



Por último refactorizamos con fecth y la respuesta  no hacemos parse sino directamente lo convertimos a json con json()

código más corto y legible 

´```javascript

const downloadSchedule = async () => {
    // await response of fetch call
    let response = await fetch("/schedule/list");

    // checking response is ok
    if (response.ok) {
        // transform body to json
        let data = await response.json();
        schedule  = data.schedule;
        displaySchedule();
    }
    else
        alert("Schedule list not available.");
}

/*
function downloadSchedule() {
   
    const request = new XMLHttpRequest();
    request.open("GET", "/schedule/list", true);
    //  request.open("GET", "/schedule/list?fail", true);
    request.onreadystatechange = function () {
        if (request.readyState === 4) {
            try {
                const response = JSON.parse(request.responseText);
                if (request.status === 200) {
                    response.schedule.forEach(function (element) {
                        schedule.push(element);
                    });
                    displaySchedule();
                } else {
                    alert(response.message);
                }
            } catch (exception) {
                alert("Schedule list not available.");
            }
        }
    };
    request.send();  
}
*/
````

````javascript


const saveStar = async (sessionId, isStarred) => {

    const headers = new Headers({
        "Content-Type": "application/x-www-form-urlencoded"
    });
    const options = {
        method: 'POST',
        headers: headers,
        body: "starred=" + isStarred
    };

    const response = await fetch("/schedule/star/" + sessionId, options);

    if (isStarred) {
        if (response.ok) {
            const data = await response.json();
            if (data.starCount > 50)
                alert("This session is very popular! Be sure to arrive early to get a seat.");
        }
    }
}

/*
function saveStar(sessionId, isStarred) {
    // TODO: Create an XMLHttpRequest that POSTs to "/schedule/star/{sessionId}"
    //       The request body must have the content type "application/x-www-form-urlencoded"
    //       e.g. "starred=true" or "starred=false"
    //       The response contains a JSON object "{ starCount: <number> }"
    //       If the star count is more than 50, warn the user about this being a busy session.
    const request = new XMLHttpRequest();
    request.open("POST", "/schedule/star/" + sessionId, true);
    if (isStarred) {
        request.onreadystatechange = function () {
            if (request.readyState === 4 && request.status === 200) {
                const response = JSON.parse(request.responseText);
                if (response.starCount > 50) {
                    alert("This session is very popular! Be sure to arrive early to get a       seat.");
                }
            }
        };
    }

    request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    const data = "starred=" + isStarred;
    console.log(data);
    request.send(data);

}
*/
````
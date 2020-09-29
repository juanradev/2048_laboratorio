### Módulo 7: Creación de objetos y métodos mediante JavaScript



se trata de refactorizar el codigo de la lista de sesiones mediante un objeto
partimos de el siguiente código en schedule.js

```javascipt

import { LocalStarStorage } from "../LocalStarStorage.js";
import { ScheduleItem } from "../ScheduleItem.js";
const element, localStarStorage;

async function startDownload() {
    // await response of fetch call
    let response = await fetch("/schedule/list")
    // transform body to json
    let data = await response.json();

    // checking response is ok
    if (response.ok) {
        downloadDone(data);
    } else {
        downloadFailed();
    }
}

function downloadDone(responseData) {
    addAll(responseData.schedule);
}

function downloadFailed() {
    alert("Could not retrieve schedule data at this time. Please try again later.");
}

function addAll(itemsArray) {
    itemsArray.forEach(add); // TODO: When refactoring this, add the `this` argument to `forEach`.
}

function add(itemData) {
    const item = new ScheduleItem(itemData, localStarStorage);
    element.appendChild(item.element);
}
const scheduleList = new ScheduleList(
    document.getElementById("schedule"),
    new LocalStarStorage(localStorage)
);
scheduleList.startDownload();
```

este codigo lo que hacía rellenar el elemento schedule (ademas de inicialiar localStorage temas siguiente)


lo que vamos a hacer es pasar todo el código a objeto 
de tal manera que será así
```javascript

import { LocalStarStorage } from "../LocalStarStorage.js";
import { ScheduleList } from "../ScheduleList.js"; // importamos el objeto


const scheduleList = new ScheduleList(      // llamamos al constructor 
    document.getElementById("schedule"),
    new LocalStarStorage(localStorage)
);
scheduleList.startDownload();               // llemanos
````

el tratamiento de la lista pasaría a la implementación del objeto 
comparar con el código anterior

````javascript
import { ScheduleItem } from "./ScheduleItem.js";  // importamos la clase ScheduleItem

export class ScheduleList {
    constructor(element, localStarStorage) {             // contructor
        this.element = element;
        this.localStarStorage = localStarStorage;
    }

    async startDownload() {                            // fectch que devuelve la lista de sesiones
        // await response of fetch call
        let response = await fetch("/schedule/list")
        // transform body to json
        let data = await response.json();

        // checking response is ok
        if (response.ok) {                                  // si ok 
            this.downloadDone(data);
        } else {                                            // si fail
            this.downloadFailed();
        }
    }

    downloadDone(responseData) {
        this.addAll(responseData.schedule);         // añadira los elementos 
    }

    downloadFailed() {
        alert("Could not retrieve schedule data at this time. Please try again later.");
    }

    addAll(itemsArray) {
        itemsArray.forEach(this.add, this);     // es llamada por downloadDone el array y por cada elemento llama a add
    }

    add(itemData) {
        const item = new ScheduleItem(itemData, this.localStarStorage);  // llama a la clase ScheduleItem
        this.element.appendChild(item.element);
    }


}
```

### Laboratorio: Creación de páginas interactivas con API HTML5.


#### Objetivos
Después de completar esta práctica de laboratorio, podrá:

- Agrega un video a una página HTML.
- Cree páginas interactivas mediante una operación de arrastrar y soltar.
- Leer archivos mediante la API de archivos.
- Obtenga la ubicación del usuario mediante la API de geolocalización.

##### Agregar un video a una página HTML.

recordamos las propiedades de un elemento video html
```` html
<video src 
       width 
       height
       poster=   src_image
       autoplay= true | false 
       muted= true | false 
       controls= true | false 
       loop=true | false  >
</video>
````
ejemplo javascript de creacción de un elemento de video
```javascript
function createVideoElement(nameOfVideoFile) {
    // Create a video object and set its properties.
    const newVideo = document.createElement("video");
    newVideo.src = nameOfVideoFile;
    newVideo.loop = true;
    newVideo.autoplay = true;
    newVideo.controls = true;
    newVideo.poster = "ImageLoading.png";
    
    // Add the video object to an existing element on the web page.
    const hostElem = document.getElementById("videoDir");
    hostElem.appendChild(newVideo);
}
```` 
#### Tareas
##### 1. Agregar video 
En la página index.htm tenemos lo siquiente
```html
<section class="video">
    <h2>Video from last year</h2>
    <video src="http://ak.channel9.msdn.com/ch9/265b/9a76fccd-941e-4285-ad00-9ea200aa265b/MIX09KEY01_high_ch9.mp4"></video>
    <div class="video-controls" style="display: none">
        <button class="video-play">Play</button>
        <button class="video-pause">Pause</button>
        <span class="video-time"></span>
    </div>
</section>

<script src="/scripts/pages/video.js" type="module"></script>

````

El control del video está en /scripts/pages/video.js

```` javascript
//  eventos

video.addEventListener("loadeddata", ready, false);
video.addEventListener("timeupdate", updateTime, false);
playButton.addEventListener("click", play, false);
pauseButton.addEventListener("click", pause, false);


//controles

const videoSection = document.querySelector(".video");
const video = videoSection.querySelector("video");
const controls = videoSection.querySelector(".video-controls");
const playButton = videoSection.querySelector(".video-play");
const pauseButton = videoSection.querySelector(".video-pause");
const time = videoSection.querySelector(".video-time");

// function ready ---> controls.style.display = "block";
// function updateTime --->  time.textContent = formatTime(video.currentTime);
/* function play  --> 
                    video.play();
                    playButton.style.display = "none";
                    pauseButton.style.display = ""; */
/* function pause()--> 
                    video.pause();
                    playButton.style.display = "";
                    pauseButton.style.display = "none";
 */
 
// 

```` 


##### Using the Geolocation API to Report the User's Current Location

Objeto navigator.geolation

   a. To One-shot position request
        invoke the navigator.geolocation.getCurrentPosition() method.

   b1. To Repeated position updates:
        invoke the navigator.geolocation.watchPosition() method. (This method returns a watch ID value. )

   b2. To stop receiving position updates:
        invoke navigator.geolocation.clearWatch(), and pass the watch ID as a parameter.

ejemplos :


```javascript

    navigator.geolocation.getCurrentPosition(myPositionCallbackFunction,
                                         myPositionErrorCallbackFunction,
                                         {enableHighAccuracy: true, timeout: 5000} );


    const watchID = navigator.geolocation.watchPosition(myPositionCallbackFunction,                                                                                
                                     myPositionErrorCallbackFunction, 
                                     {enableHighAccuracy: true, maximumAge: 10000} );

    navigator.geolocation.clearWatch(watchID)   ;

```
```javascript


GeolocationCoordinates 
accuracy: 1003
altitude: null
altitudeAccuracy: null
heading: null
latitude: 40.665088
longitude: -3.7715967999999997
speed: null
```

codigo de localitation.js

```javascript

import { distanceInMiles } from "../geometry.js";

const conferenceLocation = {
    latitude: 47.6097,  // decimal degrees
    longitude: 122.3331 // decimal degrees
};

const maximumDistanceInMilesFromConferenceToShowVenue = 10;

const distanceElement = document.getElementById("distance");
const travelSection = document.querySelector("section.travel");
const venueSection = document.querySelector("section.venue");

function distanceFromConference(coords) {
    return Math.floor(distanceInMiles(coords, conferenceLocation));   /// ../geometry.js calcula la distancia entre dos coordenadas 
};

function showDistanceMessage(distance) {
    const message = "You are " + distance + " miles from the conference";
    distanceElement.textContent = message;
};

function moveVenueSectionToTop() {
    travelSection.parentNode.insertBefore(venueSection, travelSection);
};

function updateUIForPosition(position) {
    // TODO: Calculate the distance from the conference
    const distance = distanceFromConference(position.coords);

    console.log("tu posicion es:")
    console.log( position.coords); /* puedes ver el formato del objeto GeolocationCoordinates */


    showDistanceMessage(distance);
    const isNearToConference = distance < maximumDistanceInMilesFromConferenceToShowVenue;
    if (isNearToConference) {
        moveVenueSectionToTop();
    }
};

function error() {
    distanceElement.textContent = "Could not detect your current location.";
};

// TODO: Get current position from the geolocation API.
//       Call updateUIForPosition for success and error for failure.
navigator.geolocation.getCurrentPosition(updateUIForPosition, error);

```



##### Drag and drop imagenes

En la pagina http://localhost:55981/speaker-badge.htm tenemos 
```javascript
<img style="width: 300px; height: 300px; border: 1px solid #000"/>
<script src="/scripts/pages/speaker-badge.js" type="module"></script>
```

este modulo es 
```javascript
import { SpeakerBadgePage } from "../SpeakerBadgePage.js";

const badgeElement = document.querySelector(".badge");
new SpeakerBadgePage(badgeElement);
````
por lo que habra que mirar SpeakerBadgePage.js


lo interesante aqui es la carga del file mediante promesas y la captura de los ventos 
y que no hay ningún elemento draggable 
(con tiempo revisar)


````javascript

export class SpeakerBadgePage {
    constructor(element) {
        this.imageElement = element.querySelector("img");  // imagen pasada en el constructor

        // TODO: Add event listeners for element "dragover" and "drop" events.
        //       handle with this.handleDragOver.bind(this) and this.handleDrop.bind(this)

        element.addEventListener("dragover", this.handleDragOver.bind(this), false);
        element.addEventListener("drop", this.handleDrop.bind(this), false);
    }

    handleDragOver(event) {
        event.stopPropagation();
        event.preventDefault();
        event.dataTransfer.dropEffect = 'copy'; // Makes the browser display a "copy" cursor.
    }

    handleDrop(event) {
        event.stopPropagation();
        event.preventDefault();

        const files = event.dataTransfer.files; //  Get the files from the event <--------------------
     

        if (files.length == 0) return;

        const file = files[0];
       //       Check the file type is an image
        if (this.isImageType(file.type)) { 
        //       Use this.readFile to read the file, then display the image
        //       (Note that this.readFile returns a Promise, so chain ((file)=> this.displayImage(file)) using the "then" method.)
            this.readFile(file).then((file) => this.displayImage(file));
        } else {
            alert("Please drop an image file.");
        }
    }

    isImageType(type) {
        const imageTypes = ["image/jpeg", "image/jpg", "image/png"];
        return imageTypes.indexOf(type) >= 0;
    }

    readFile(file) {
        // Return a new promise.
        return new Promise(function (resolve, reject) {

            const reader = new FileReader(); // Create a new FileReader

            // TODO: Assign a callback function for reader.onload

            // TODO: In the callback use resolve([fileDataUrl]); to return the file data URL.

            // TODO: Start reading the file as a DataURL
            reader.onload = function (loadEvent) {
                const fileDataUrl = loadEvent.target.result;
                resolve([fileDataUrl]);
            };

   

            reader.readAsDataURL(file);

        });
    }

    displayImage(imageUrl) {
        this.imageElement.src = imageUrl;
    }
}

````


Para recordar como se utilizaba el reader.....
````javascript

/*
	<input type="file" id="theBinaryFile" onchange="onLoadBinaryFile()" />
	<img id="theImage"></img>
*/
const onLoadBinaryFile =()  => {
		const theFileElem = document.getElementById("theBinaryFile");  
		// Get the File object selected by the user (and make sure it is an image file).
		if (theFileElem.files.length != 0 && theFileElem.files[0].type.match(/image.*/)) {
			// Create a FileReader and handle the onload and onerror events.
			const reader = new FileReader();
			reader.onload = (e) => {
				const theImgElem = document.getElementById("theImage");
				theImgElem.src = e.target.result;
			};
			reader.onerror = (e) => {
				alert("cannot load binary file");
			};
			// Read the binary file.
			reader.readAsDataURL(theFileElem.files[0]);
		}
		else {
			alert("Please select a binary file");
		}
};
		
/*
	<input type="file" id="theTextFile" onchange="onLoadTextFile()" /> 
	<textarea id="theMessageArea" rows="30" cols="40"></textarea>
*/

const onLoadTextFile() => {
		const theFileElem = document.getElementById("theTextFile");
		// Get the File object selected by the user, and make sure it is a text file.
		if (theFileElem.files.length != 0 && theFileElem.files[0].type.match(/text.*/)) {
			// Create a FileReader and handle the onload and onerror events.
			const reader = new FileReader();
			reader.onload = (e) = > {
				const theMessageAreaElem = document.getElementById("theMessageArea");
				theMessageAreaElem.value = e.target.result;
			};
			reader.onerror = (e) => {
				alert("cannot load text file");
			};
			// Read text file (the second parameter is optional - the default encoding is "UTF-8").
			reader.readAsText(theFileElem.files[0], "ISO-8859-1");
		} else {
			alert("Please select a text file");
		}
}
```

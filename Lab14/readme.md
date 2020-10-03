## lab14 webworker

para que cuando hagamos el drop llamamemos a la funcion de cambio de grises


añadimos al handleDrop(event)
.then((file) => grayscaleImage(file))

````javascript
export class SpeakerBadgePage {

    constructor(element) {
        this.canvas = element.querySelector("canvas");
        this.busyElement = element.querySelector(".busy");

        this.speakerId = this.canvas.getAttribute("data-speaker-id");
        this.speakerName = this.canvas.getAttribute("data-speaker-name");
        this.canvas.addEventListener("dragover", this.handleDragOver.bind(this));
        this.canvas.addEventListener("drop", this.handleDrop.bind(this));

        this.drawBadge();
    }
    ...........................


 handleDrop(event) {
        event.stopPropagation();
        event.preventDefault();

        const files = event.dataTransfer.files;
        if (files.length == 0) return;

        // More than one file could have been dropped, we'll just use the first.
        const file = files[0];
        if (this.isImageType(file.type)) {
            this.busy();
            // TODO: Add grayscaleImage into the processing pipeline.
            this.readFile(file)
                .then((file) => this.loadImage(file))
                .then((file) => grayscaleImage(file))
                .then((file) => this.drawBadge(file))
                .then((file) => this.notBusy(file));
        } else {
            alert("Please drop an image file.");
        }
    }
```` 

recordar el canvas
```html
 <section class="page-section badge">
            <div class="container">
                <h1>Create your speaker badge for ContosoConf</h1>
                <canvas
                    width="500" 
                    height="200" 
                    style="border: 1px solid #888"
                    data-speaker-id="234724"
                    data-speaker-name="Mark Hanson">
                </canvas>
                <div class="busy">
                    <p>Creating badge. Please wait...</p>
                </div>
            </div>
        </section>
````


la funcion grayscaleImage es la que se encarga de crear el webworker 

retorna una promesa por eso .then((file) => grayscaleImage(file))

define el worker en "/scripts/grayscale-worker.js"

crea el handle (lo que hace es repintar el canvas)

arma el escuchador del mensaje

y manda el mensaje (imagenoriginal)

```javascript
export function grayscaleImage(image) {
    // Converts a colour image into gray scale.

    // Return a new promise.
    return new Promise(function (resolve, reject) {   
        const canvas = createCanvas(image);
        const context = canvas.getContext("2d");
        const imageData = getImageData(context, image);

        // TODO: Create a Worker that runs /scripts/grayscale-worker.js
        const worker = new Worker("/scripts/grayscale-worker.js");
        const handleMessage = function (event) {
            // Update the canvas with the gray scaled image data.
            context.clearRect(0, 0, canvas.width, canvas.height);
            context.putImageData(event.data.done, 0, 0);

            // Returning a Promise makes this function easy to chain together with other deferred operations.
            // The canvas object is returned as this can be used like an image.
            resolve(canvas);
        };
        worker.addEventListener("message", handleMessage.bind(this));
        worker.postMessage(imageData);

       
    });
};
'''

el webworker estará en /scripts/grayscale-worker.js

recibe la imagen 
lo pasa [] (data)
y en un bucle por cada pixels (de 4 en cuatro) llama a la funcion grayscalePixel
esta funcion se me va un poquillo (con tiempo la miro)

cuando acaba el bucle manda el post con done:imagen


````javascript
addEventListener("message", function (event) {
    const imageData = event.data;
    let pixels = imageData.data;
    for (let i = 0; i < pixels.length; i += 4) {
        grayscalePixel(pixels, i);
    }
    postMessage({ done: imageData });
});

function grayscalePixel(pixels, index) {
    /// <summary>Updates the pixel, starting at the given index, to be gray scale.</summary>

    const brightness = 0.34 * pixels[index] + 0.5 * pixels[index + 1] + 0.16 * pixels[index + 2];

    pixels[index] = brightness; // red
    pixels[index + 1] = brightness; // green
    pixels[index + 2] = brightness; // blue
};
````


referencias al script
```html
        <script src="/scripts/grayscale.js" type="module"></script>
        <script src="/scripts/pages/speaker-badge.js" type="module">
````
![alt text](./Captura.PNG "Captura.PNG") 
## Laboratorio

#### dibujar un svg

comprobar que existe el svg y el enlace a 

```hmtl
 <script src="/scripts/pages/location-venue.js" type="text/javascript"></script>

<svg viewBox="-1 -1 302 102" width="100%" height="230">
    <!-- Room A -->
    <g id="room-a" class="room">
        <rect fill="#fff" x="0" y="0" width="100" height="100"></rect>
        <text x="13" y="55" font-weight="bold" font-size="20">ROOM A</text>
    </g>
    <!-- Room B -->
                            
    <!-- The outline of the building -->
    <polyline fill="none" stroke="#000" points="135,95 140,100 0,100 0,0 100,0 100,80 130,80 130,70 110,70 110,30 190,30 190,70 170,70 170,80 200,80 200,0 300,0 300,100 160,100 165,95"></polyline>
    <text x="150" y="55" font-size="12" style="text-anchor: middle">Registration</text>
</svg>
```

![alt text](./svg01.png "The incomplete venue map")

agremamos el rect del room b

```html
<g id="room-b" class="room">
      <rect fill="#fff" x="200" y="0" width="100" height="100"/>
      <text x="213" y="55" font-weight="bold" font-size="20">ROOM B</text>
</g>

<!-- la clase room
.room {
    cursor: pointer;
}
-->
```` 


queremos que cunado haga click en cada room visualice su información correspondiente

```html
<div id="room-a-info" style="display: none">
    <h2>Room A</h2>
    <p>Capacity: 250</p>
    <p>Popular sessions in here:</p>
    <ul>
        <li>Diving in at the deep end with Canvas</li>
        <li>Real-world Applications of HTML5 APIs</li>
        <li>Transforms and Animations</li>
    </ul>
</div>

<div id="room-b-info" style="display: none">
    <h2>Room B</h2>
    <p>Capacity: 230</p>
    <p>Popular sessions in here:</p>
    <ul>
        <li>Building Responsive UIs</li>
        <li>Getting to Grips with JavaScript</li>
        <li>A Fresh Look at Layouts</li>
    </ul>
</div>
  ```
  modificamos location-venue.js para añadir el evento al click
  ```javascript
  let currentInfoDiv = document.getElementById("instruction");

function showRoomInfo(roomId) {
    const infoDiv = document.getElementById(roomId + "-info");
    if (currentInfoDiv) {
        currentInfoDiv.style.display = "none";
    }
    infoDiv.style.display = "block";
    currentInfoDiv = infoDiv;
};

    // TODO: Get the room elements in the svg element. 
    // const rooms = ;
const rooms = document.querySelectorAll(".room");

    // TODO: Add a click event listener for each room element.
    //       Call the showRoomInfo function, passing the clicked element's id property.

for (let i = 0; i < rooms.length; i++) {
    const room = rooms[i];
    room.addEventListener("click", function () { showRoomInfo(this.id); });  // parametro buglet?¿
}

```` 


tamnien retocamos el room:hover
```css
    .room:hover rect {
        fill: #b1f8b0;
    }
```

#### dibujar un canvas


en speaker-badge.htm tecleamos el canvas

```html

    <section class="page-section badge">
        <div class="container">
            <h1>Create your speaker badge for ContosoConf</h1>
            <!-- TODO: Add canvas here -->

            <canvas width="500"
                    height="200"
                    style="border: 1px solid " #888"
                    data-speaker-id="234724"
                    data-speaker-name="Mark Hanson">
            </canvas>

        </div>
    </section>
```` 

Para pintar en un canvas hay que hacerlo a traves de la API en javascript speakerBadgePage.js


```javascript

export class speakerBadgePage {

    constructor(element) {
        this.canvas = element.querySelector("canvas"); // canvas <------------------------------------

        this.speakerId = this.canvas.getAttribute("data-speaker-id");
        this.speakerName = this.canvas.getAttribute("data-speaker-name");
        this.canvas.addEventListener("dragover", this.handleDragOver.bind(this));
        this.canvas.addEventListener("drop", this.handleDrop.bind(this));

        this.drawBadge();
    }

    handleDragOver(event) {
        event.stopPropagation();
        event.preventDefault();
        event.dataTransfer.dropEffect = 'copy'; // Makes the browser display a "copy" cursor.
    }

    handleDrop(event) {
        event.stopPropagation();
        event.preventDefault();

        const files = event.dataTransfer.files;
        if (files.length == 0) return;

        // More than one file could have been dropped, we'll just use the first.
        const file = files[0];
        if (this.isImageType(file.type)) {
            this.readFile(file)
                .then((file) => this.loadImage(file))
                .then((file) => this.drawBadge(file));
        } else {
            alert("Please drop an image file.");
        }
    }

    drawBadge(image) {
        // TODO: Get the canvas's (this.canvas) context and assign to this.context
        this.context = this.canvas.getContext("2d");

        // TODO: Draw the following by calling the helper methods of `this`
        //       background
        //       top text
        //       speaker name
        //       image (or placeholder if no image)
        //       bar code (passing this.speakerId)
        this.drawBackground();
        this.drawTopText();
        this.drawSpeakerName();
        if (image) {
            this.drawSpeakerImage(image);
        } else {
            this.drawImagePlaceholder();
        }
        this.drawBarCode(this.speakerId)
    }

    drawBackground() {
        // TODO: Fill the canvas with a white rectangle
        this.context.fillStyle = "white";
        this.context.fillRect(0, 0, this.canvas.width, this.canvas.height);
    }

    drawSpeakerImage(image) {
        // TODO: Draw the image on the canvas
        //       Draw only the center square of the image
        //       Draw at:
        //       x, y = 20, 20
        //       w, h = 160, 160
        const size = Math.min(image.width, image.height);
        const sourceX = image.width / 2 - size / 2;
        const sourceY = image.height / 2 - size / 2;
        this.context.drawImage(image, sourceX, sourceY, size, size, 20, 20, 160, 160);
    }

    drawImagePlaceholder() {
        this.context.strokeStyle = "2px #888";
        this.context.strokeRect(20, 20, 160, 160);
        this.context.font = "12px sans-serif";
        this.context.textBaseline = "middle";
        this.context.textAlign = "center";
        this.context.fillStyle = "black";
        this.context.fillText("Drag your profile photo here", 100, 100);
    }

    drawTopText() {
        this.context.font = "20px sans-serif";
        this.context.fillStyle = "black";
        this.context.textBaseline = "top";
        this.context.textAlign = "left";
        this.context.fillText("ContosoConf 2013 Speaker:", 200, 20);
    }

    drawSpeakerName() {
        // TODO: Draw this.speakerName on the canvas
        //       x, y = 200, 60
        //       font = 40px sans-serif
        //       fill style = black
        //       text baseline = top
        //       text align = left
        this.context.font = "40px sans-serif";
        this.context.fillStyle = "black";
        this.context.textBaseline = "top";
        this.context.textAlign = "left";
        this.context.fillText(this.speakerName, 200, 60);
    }

    drawBarCode(text) {
        text = "*" + text + "*"; // Wrap in "*" deliminators.
        const encodings = {
            "0": "bwbWBwBwb",
            "1": "BwbWbwbwB",
            "2": "bwBWbwbwB",
            "3": "BwBWbwbwb",
            "4": "bwbWBwbwB",
            "5": "BwbWBwbwb",
            "6": "bwBWBwbwb",
            "7": "bwbWbwBwB",
            "8": "BwbWbwBwb",
            "9": "bwBWbwBwb",
            "*": "bWbwBwBwb"
        };
        let x = 200, y = 140, height = 40, thick = 6, thin = 2;
        for (let charIndex = 0; charIndex < text.length; charIndex++) {
            const code = encodings[text[charIndex]];
            for (let stripeIndex = 0; stripeIndex < code.length; stripeIndex++) {
                if (stripeIndex % 2 === 0) {
                    this.context.fillStyle = "black";
                } else {
                    this.context.fillStyle = "white";
                }
                const isWideStripe = code.charCodeAt(stripeIndex) < 91;
                if (isWideStripe) {
                    this.context.fillRect(x, y, thick, height);
                    x += thick;
                } else {
                    this.context.fillRect(x, y, thin, height);
                    x += thin;
                }
            }

            if (charIndex < text.length - 1) {
                // Space between each
                this.context.fillStyle = "white";
                this.context.fillRect(x, y, thin, height);
                x += thin;
            }
        }
    }

    isImageType(type) {
        const imageTypes = ["image/jpeg", "image/jpg", "image/png"];
        return imageTypes.indexOf(type) === 0;
    }

    readFile(file) {
           // Return a new promise.
        return new Promise(function (resolve, reject) {

            const reader = new FileReader();

            reader.onload = function (loadEvent) {
                const fileDataUrl = loadEvent.target.result;

                resolve([fileDataUrl]);
            };

            reader.readAsDataURL(file);
        });
    }

    loadImage(imageUrl) {
        // Return a new promise.
        return new Promise(function (resolve, reject) {
            const image = new Image();

            image.onload = function () {
                resolve(image);
            };

            image.src = imageUrl; // This starts the image loading
        });
    }
}
```

![alt text](./marc.png "marc")
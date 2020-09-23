## Laboratorio Modulo 04  Creating Forms to Collect and Validate User Input

##### Alumno: Juan Ramón Díaz Fernández
###### fecha 23/09/2017
###### dificultad: ninguna


Partimos de la solución de  Allfiles\Mod04\Labfiles\Starter\Exercise 1 que es similar a la solucion del modulo 3
Pero le hemos añadido la pagina de register.htm y registered.htm añadir como su correspondiente css y js 

El ejercicio consiste en crear un formulario de registro y hacer validaciones en el lado del cliente
No obstante también hay implementada un valiadación del lado del servidor ya que  el submit hace una llamada al controlodor RegistrationController
Si el modelo es erroneo nos lanzará una vista Error.cshtml en la cual nos presentará los errores
Si el modelo es correcto nos redireccionara a registered.htm 
Para comprobar que funciona no deberá pintar Error.cshtml en caso de error puesto que los erroeres los controlamos en cliente
aunque siempre es buena práctica comprobar en servidor (además de que hay datos que solo se pueden comprobar en servidor por ejemplo usuario ya existe)


### Exercise 1: Creating a Form and Validating User Input by Using HTML5 Attributes
1. añadir a register.htm el formulario
2. hacerlo más user-friendly atributos placeholder y autofocus
3. añadir atributos required (WebsiteUrl es no requerida)
4. Validacion de la password al menos 5 numeros o letras para ello pattern="[a-zA-Z0-9]{5,} y title="At least 5 letters and numbers" 


### Exercise 2: Validating User Input by Using JavaScript
Vamos a verificar si las passwords son iguales
vamos a añadir el ebento input de las passwords la funcion checkpasswords (esto lo hacemos cuando creemeos la página)

```javascript
    passwordInput.addEventListener("input", checkPasswords, false);
    confirmPasswordInput.addEventListener("input", checkPasswords, false);
```
y la funcion checkpasswords llamara a setCustomValidity dejando CustomValidity en blanco si ok o con el mensaje de error si no ok
Es importante saber que el submit del formulario no se envia si alguna CustomValidity != ""
como se hace en los inputs siempre esta actualizada cuando hacemos click
```javascript
const checkPasswords = function () {
    // TODO: Compare passwordInput value to confirmPasswordInput value
    const iguales = (passwordInput.value === confirmPasswordInput.value);
    // TODO: If passwords don't match then display error message on confirmPasswordInput (using setCustomValidity)
    if (iguales)
        confirmPasswordInput.setCustomValidity("");
    else
        confirmPasswordInput.setCustomValidity("vaya! las contraseñas no son iguales");
    // TODO: If passwords do match then clear the error message (setCustomValidity with empty string)
};
```


### añadir estilo cuando elemento no es valido

```css
.register form.submission-attempted input:invalid {
    background-color: #f9b2b2;
    outline: none;
}
```

en teoria con input:invalid  valdría pero recordar que los campos estan vacios cuando se carga la página (es decir estan invalid)
por eso form.submission-attempted

si te fijas en el js añadirmos al evento click de <button type="submit">Register</button> la funcion formSubmissionAttempted
que lo que hace es añadir la clase submission-attempted al formulario
es decir la marcamos como que el usuario ya ha dado aceptar al menos una vez así 
```css
form.submission-attempted input:invalid /* valdría para aplicar el estilo */
```



```javascript
const submitButton = form.querySelector("button");

const formSubmissionAttempted = function() {
    form.classList.add("submission-attempted");
};

const addSubmitClickEventListener = function() {
    submitButton.addEventListener("click", formSubmissionAttempted, false);
};
```


nota .register no haría falta es que el formulario esta dentro de una <section class="page-section register">
asi queda .register form.submission-attempted input:invalid
los input:ivalid
dentro de un form con la clase submission-attempted
dentro de algun elemento con la clase register


<p align="center"><img src="./img/keyboard.jpg" height="200" alt=" " /></p>
<h1 align="center">Keyboard </h1> 
<h4 align="right">Sep 23</h4>
<img src="https://img.shields.io/badge/OS-Windows%2011-blue">
<img src="https://img.shields.io/badge/OS-Linux%20GNU-yellowgreen">
<img src="https://img.shields.io/badge/Node%20-V18.7.0-green">
<img src="https://img.shields.io/badge/Python%20-V3.9.2-orange">

<br>

Code snippets to read keyboard Python and Nodejs

<br>

# NodeJS
## KeyPress Package

> :warning: **Warning:** Funciona solo en primer plano, en segundo plano no detecta el presionar de ninguna tecla.
> 
Lectura simple de de teclado (Demo)
```
/**
 * Keyboard Control Demo on Linux / windows
*/

var keypress = require('keypress');
 
// make `process.stdin` begin emitting "keypress" events
keypress(process.stdin);

console.log("keyboard");
 
// listen for the "keypress" event
process.stdin.on('keypress', function (ch, key) {
  console.log('got "keypress"', key.name);
  if (key && key.ctrl && key.name == 'c') {
    process.stdin.pause();
  }else{
    if (key.name == "w" || key.name == "W") {
        console.log("Hello World");   
    }
  }
});
 
process.stdin.setRawMode(true);
process.stdin.resume();

```

<br>

keyboard control
```
/**
 * Keyboard Control Basic on Linux / windows
*/


var keypress = require('keypress');
 
// make `process.stdin` begin emitting "keypress" events
keypress(process.stdin);

console.log("keyboard");
 
// listen for the "keypress" event
process.stdin.on('keypress', function (ch, key) {
  //console.log('got "keypress"', key);
  if (key.ctrl && key.name == "c") {
    process.exit();
  }

  if (key.name == "right") {
    console.log("right");
  }

  if (key.name == "left") {
    console.log("left");
  }

  if (key.name == "up") {
    console.log("up");
  }

  if (key.name == "down") {
    console.log("down");
  }

  if (key.name == "escape") {
    console.log("esp");
  }

  if (key.name == "space") {
    console.log("space");
  }

});
 
process.stdin.setRawMode(true);
process.stdin.resume();
```
<br>

Modulo para lectura de teclado
```
 wating...
```

<br>

## Readline Package

Leyendo cadena de caracteres (Demo)
```
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
  });
  
  readline.question('Who are you?', name => {
    console.log(`Hey there ${name}!`);
    readline.close();
  });

```
<br>

Bucle que lee una cadena de caracteres hasta la palabra sea  “exit”
```
const readline = require('readline');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

function readInput() {
  rl.question('Ingresa una palabra: ', (answer) => {
    if (answer.toLowerCase() === 'exit') {
      console.log('Saliendo de la aplicación...');
      rl.close();
    } else {
      console.log('Palabra ingresada:', answer);
      readInput();
    }
  });
}

console.log('Ingresa "exit" para cerrar la aplicación.');
readInput();
```

### Modulo para lectura de cadenas desde el teclado
keyboardInput.js
```
const readline = require('readline');

function readKeyboardInput(callback) {
  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
  });

  rl.question('Ingresa una palabra: ', (answer) => {
    rl.close();
    callback(answer);
  });
}

module.exports = { readKeyboardInput };

```

main.js
```
const { readKeyboardInput } = require('./keyboardInput');

function startApp() {
  let shouldExit = false;

  function handleInput(answer) {
    const lowerCaseAnswer = answer.toLowerCase();
    if (lowerCaseAnswer === 'exit') {
      console.log('Saliendo de la aplicación...');
      shouldExit = true;
    } else if (lowerCaseAnswer === 'perro' || lowerCaseAnswer === 'gato' || lowerCaseAnswer === 'raton') {
      console.log('Has ingresado un animal:', answer);
    } else {
      console.log('Palabra ingresada:', answer);
    }

    if (!shouldExit) {
      console.log('Ingresa "exit" para salir.');
      readKeyboardInput(handleInput);
    }
  }

  console.log('La aplicación está en un bucle infinito. Ingresa "exit" para salir.');
  readKeyboardInput(handleInput);
}

startApp();
```

<br>

<br>

# Leer los argumentos de un archivo Nodejs

## read arguments Standard Method (no library)
example: node app.js one two three

app.js
```
var argv = process.argv.slice(2);
var program_name = process.argv[0]; //value will be "node"
var script_path = process.argv[1]; //value will be "yourscript.js"
var first_value = process.argv[2]; //value will be "first_value"
var second_value = process.argv[3]; //value will be "second_value"
console.log("All args: ", args );
console.log("second argument: ", second_value);
```

> :memo: **Note:** Con ```var args = process.argv.slice(2);``` se omiten que muestre el comando usado y la ruta del archivo en el array.  Pero igual permanecen en el array[0], array[1].

Ejemplo mostrando todo el array
```
const args = process.argv;
var program_name = process.args[0]; //value will be "node"
var script_path = process.args[1]; //value will be "yourscript.js"
var first_value = process.args[2]; //value will be "first_value"
var second_value = process.args[3]; //value will be "second_value"
console.log("All args: ", args );
console.log("second argument: ", second_value);
```

<br>

## Leyendo los argumentos como si fuesen un JSON (with library)
sample: node app.js -i 100 -y 200 -z 300

app.js
```
var argv = require('minimist')(process.argv.slice(2));
console.log("All args: ", argv );
console.log("z args: ", argv.z );

```
<br>

## Buscar comandos en un archivo JSON, usando el argumento como palabra de busqueda
Example: node app.js ls / node app.js mkdir <br>

data.json
```
{
    "ls": "enumera el contenido del directorio que desee, archivos y otros directorios anidados.",
    "pwd": "confirmar en qué directorio se está trabajando actualmente.",
    "mkdir": " permite crear una o más carpetas en tu directorio de trabajo actual.",
    "rm": "eliminar archivo."
}
```

app.js
```
var value = process.argv[2];


const fs = require('fs');

// Ruta al archivo JSON
const filePath = 'data.json';

//console.log(value);

// Palabra que queremos buscar
const targetWord = value;

// Lee el archivo JSON
fs.readFile(filePath, 'utf8', (err, data) => {
  if (err) {
    console.error('Error al leer el archivo JSON:', err);
    return;
  }

  try {
    // Convierte el contenido del archivo a un objeto JavaScript
    const jsonData = JSON.parse(data);

    // Itera sobre las propiedades (claves) del objeto
    for (const key in jsonData) {
      if (key === targetWord) {
        console.log(`comando: '${key}': ${jsonData[key]}`);
      }
    }
  } catch (error) {
    console.error('Error al analizar el contenido JSON:', error);
  }
});

```

<br>

<br>


# Python
```
wating...
```





<br>

<br>

---
Copyright &copy; 2022 [carjavi](https://github.com/carjavi). <br>
```www.instintodigital.net``` <br>
carjavi@hotmail.com <br>
<p align="center">
    <a href="https://instintodigital.net/" target="_blank"><img src="./img/developer.png" height="100" alt="www.instintodigital.net"></a>
</p>



# keyboard


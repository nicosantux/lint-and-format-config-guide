# Automatizando las configuraciones con Yeoman.

Yeoman es una herramienta que nos permite crear generadores de código de una manera rápida y relativamente sencilla. [Documentación](https://yeoman.io/authoring/)

## Primeros pasos con Yeoman.

Antes de comenzar a crear nuestro primer generador, necesitamos instalar Yeoman como una dependencia global.

```bash
npm i -g yo
```

Para empezar con nuestro primer generador, crearemos una carpeta en la que iniciaremos un proyecto de Node.js. Nuestro `package.json` debe verse de la siguiente manera:

```json
{
  "name": "generator-midas",
  "version": "0.0.0",
  "description": "Midas Generator",
  "files": ["generators"],
  "keywords": ["yeoman-generator"],
  "dependencies": {
    "yeoman-generator": "5.9.0"
  }
}
```

Es muy importante que el nombre de nuestro paquete comience con el prefijo `generator-` para que yeoman identifique al paquete como un generador disponible.

## Estructura de carpetas.

Una vez finalizado con nuestro `package.json`, debemos seguir la siguiente estructura de carpetas.

```
├───package.json
└───generators/
    ├───app/
    │   └───index.js
    │   └───templates/
    └───sub-generator-1/
    │   └───index.js
    │   └───templates/
    └───sub-generator-2/
        └───index.js
        └───templates/
```

El punto de entrada al generador es `generators/app/index.js`. Si queremos crear subgeneradores, podemos crear carpetas en el mismo nivel que la carpeta `app`. Cada generador o subgenerador puede tener su carpeta de plantillas en la que tenga archivos que se crearán en la carpeta en la que se ejecute el generador.

## Creando el primer generador.

En este ejemplo, vamos a crear un generador para automatizar nuestras configuraciones de ESLint y Prettier. Un generador es una clase que extiende de la clase `Generator`. Si bien podemos crear métodos con cualquier nombre, Yeoman recomienda la siguiente estructura que se ejecutará de manera secuencial:

1. initializing
2. prompting
3. configuring
4. default
5. writing
6. conflicts
7. install
8. end

_Aclaración: no es necesario que en cada generador se definan todos estos metodos._

Ahora veamos cómo se vería nuestro primer generador:

```js
const Generator = require("yeoman-generator");

module.exports = class extends Generator {
  initializing() {
    this.log(`
    ==================================================================
                      Welcome to Midas-Generator!
    ==================================================================`);
  }

  async prompting() {
    this.answer = await this.prompt([
      {
        choices: ["npm", "pnpm", "yarn"],
        message: "Which package manager does your project use?",
        name: "packageManager",
        type: "list",
      },
      {
        default: true,
        message: "Would you like to add lint scripts?",
        name: "addScripts",
        type: "confirm",
      },
    ]);
  }

  writing() {
    if (this.answer.addScripts) {
      const pkgJson = {
        scripts: {
          lint: "eslint .",
          ["lint:fix"]: "eslint . --fix --ext .js,.jsx,.ts,.tsx,.cjs,.mjs",
          format: "prettier . --check",
          ["format:fix"]: "prettier . --write",
        },
      };

      this.fs.extendJSON(this.destinationPath("package.json"), pkgJson);
    }

    this.fs.copy(
      this.templatePath(".eslintrc.json"),
      this.destinationPath(".eslintrc.json"),
    );

    this.fs.copy(
      this.templatePath(".eslintignore"),
      this.destinationPath(".eslintignore"),
    );

    this.fs.copy(
      this.templatePath(".prettierrc"),
      this.destinationPath(".prettierrc"),
    );

    this.fs.copy(
      this.templatePath(".prettierignore"),
      this.destinationPath(".prettierignore"),
    );
  }

  install() {
    this.spawnCommandSync(this.answer.packageManager, [
      this.answer.packageManager === "npm" ? "install" : "add",
      "-D",
      "-E",
      "@typescript-eslint/eslint-plugin",
      "@typescript-eslint/parser",
      "eslint",
      "eslint-config-prettier",
      "eslint-plugin-import",
      "eslint-plugin-promise",
      "eslint-plugin-react",
      "eslint-plugin-react-hooks",
      "prettier",
    ]);
  }
};
```

Yeoman nos provee de ciertos métodos para poder agilizar la creación del generador. Si tienes alguna duda, puedes consultar la [documentación de la API de Yeoman](https://yeoman.github.io/generator/). Ahora hagamos un paso a paso de lo que sucede en el código:

1. Hacemos un `require` de `yeoman-generator` y exportamos una clase que extiende de Generator.
2. Creamos el método `initializing` que muestra un mensaje de bienvenida.
3. Creamos el método `prompting` para interactuar con el usuario. Yeoman utiliza internamente [Inquirer.js](https://github.com/SBoudrias/Inquirer.js)
4. Creamos el método `writing` en el que recuperamos la respuesta obtenida del usuario para agregar los scripts de formato y lint en nuestro `package.json` y copiamos en la carpeta de destino los archivos de configuración como `.eslintrc.json`, `.prettierrc`, etc
5. Creamos el método `install` en el que recuperamos la respuesta obtenida del usuario para instalar las dependencias con el gestor de paquetes que su proyecto utiliza.

## Nuestro generador en acción.

Para probar nuestro generador, tenemos dos opciones:

1. Correr el comando `npm link` en la carpeta raiz de nuestro proyecto instalara nuestro generador de manera global como si fuese un paquete publicado en `npm`. Para quitar el paquete correr el comando `npm unlink generator-midas`.
2. Publicar nuestro paquete para que pueda ser utilizado desde cualquier lugar. En el caso de publicar el paquete, deberás instalarlo de forma global en tu computadora. Ej: `npm i -g generator-midas`.

Correr el siguiente comando:

```bash
yo midas
```

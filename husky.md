# Husky.

Husky es un paquete que nos sirve para utilizar de una manera más sencilla los git hooks. [Documentación](https://typicode.github.io/husky/getting-started.html)

## ¿Cómo instalar husky en mi proyecto?

Para instalar Husky en el proyecto, debemos ejecutar el siguiente comando:

```bash
npx husky-init && npm install
```

Este comando nos activa los git hooks y nos crea la carpeta `.husky` con un archivo de ejemplo `.pre-commit`

## ¿Cómo agregar un git hook?

En este ejemplo, vamos a ver cómo crear un git hook para que, al hacer un commit, se valide que sigue las normas de conventional commits.

### Instalar Commitlint.

Commitlint es un paquete que nos ayuda a que nuestros commits sigan ciertas reglas. Para utilizarlo, vamos a instalar dos dependencias de desarrollo.

```bash
npm i -D -E @commitlint/cli @commitlint/config-conventional
```

Necesitamos crear un archivo `.commitlintrc.json` que extienda la configuración de conventional commits. En el caso de que querramos modificar reglas, las agregamos en la sección `rules`. [Reglas commitlint](https://commitlint.js.org/#/reference-rules)

```json
{
  "extends": ["@commitlint/config-conventional"],
  "rules": {
    "subject-case": [2, "always", "lower-case"],
    "body-max-line-length": [2, "always", 72]
  }
}
```

En nuestro `package.json`, agregamos el siguiente script:

```json
{
  "scripts": {
    "commitlint": "commitlint --edit"
  }
}
```

### Crear nuestro hook commit-msg.

Una vez que ya dejamos configurado `commitlint`, vamos a ejecutar el siguiente comando para agregar nuestro git hook:

```bash
npx husky add .husky/commit-msg "npm run commitlint ${1}"
```

En este comando, le decimos a Husky que agregue dentro de la carpeta `.husky` el hook `commit-msg` y que este contenga el comando `npm run commitlint ${1}`. Con esto, cada vez que escribamos un commit, se verificará si cumple con las reglas establecidas.

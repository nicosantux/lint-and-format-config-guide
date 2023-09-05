# ESLint.

ESLint es un proyecto de código abierto que nos ayuda a detectar y corregir problemas en nuestro código. [Documentación](https://eslint.org/docs/latest/use/getting-started)

## ¿Cómo instalar ESLint en mi proyecto?

Para iniciar una configuración de ESLint, haremos lo siguiente:

```bash
npx eslint --init
```

o bien:

```bash
npm init @eslint/config
```

Con el CLI de ESLint, vamos a poder crear una configuración a partir de reglas populares (standard, Airbnb, etc.) o configurar desde cero nuestras preferencias. Al finalizar, nos preguntará si deseamos instalar las dependencias necesarias y nos creará nuestro archivo `.eslintrc`

## ¿Cómo ejecutar ESLint?

Para poder analizar nuestro código con ESLint, podemos ejecutar el siguiente comando:

```bash
npx eslint .
```

Esto hará que ESLint se ejecute en la carpeta en la que estemos posicionados y nos liste los errores (también sirve para CI). Si queremos que intente corregir los errores listados, usamos el siguiente comando:

```bash
npx eslint . --fix --ext .tsx,.ts
```

De esta manera, le estamos indicando a ESLint que intente corregir los errores en todos los archivos de la carpeta raíz con las extensiones `.tsx` y `.ts`. Ahora que sabemos esto, podemos crear los siguientes scripts en nuestro `package.json`:

```json
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint . --fix --ext .tsx,.ts"
  }
}
```

## ¿Cómo ignoramos carpetas y/o archivos?

Para esto, podemos crear en nuestra carpeta raíz un archivo llamado `.eslintignore` que funciona exactamente como un `.gitignore`, pero para que ESLint no se ejecute en dichas carpetas/archivos.

## Paquetes de ESLint recomendados.

- eslint-config-prettier (si utilizamos Prettier, este paquete es fundamental)
- eslint-plugin-import

## Ejemplo de una configuración de ESLint para React + Typescript.

```json
{
  "env": {
    "browser": true,
    "es2023": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "prettier"
  ],
  "plugins": ["react", "import", "@typescript-eslint"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "rules": {
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",
    "no-console": "warn",
    "eqeqeq": "error",
    "@typescript-eslint/no-unused-vars": [
      "error",
      {
        "args": "after-used",
        "ignoreRestSiblings": false,
        "argsIgnorePattern": "^_.*?$"
      }
    ],
    "import/order": [
      "warn",
      {
        "groups": [
          "type",
          "builtin",
          "object",
          "external",
          "internal",
          "parent",
          "sibling",
          "index"
        ],
        "pathGroups": [
          {
            "pattern": "~/**",
            "group": "external",
            "position": "after"
          }
        ],
        "newlines-between": "always"
      }
    ],
    "react/self-closing-comp": "warn",
    "react/jsx-sort-props": [
      "warn",
      {
        "noSortAlphabetically": false
      }
    ],
    "padding-line-between-statements": [
      "warn",
      {
        "blankLine": "always",
        "prev": "directive",
        "next": "*"
      },
      {
        "blankLine": "always",
        "prev": "import",
        "next": ["block-like", "export", "const", "let"]
      },
      {
        "blankLine": "always",
        "prev": "*",
        "next": "block-like"
      },
      {
        "blankLine": "always",
        "prev": "block-like",
        "next": "*"
      },
      {
        "blankLine": "always",
        "prev": "*",
        "next": "return"
      },
      {
        "blankLine": "always",
        "prev": ["const", "let", "var"],
        "next": "*"
      },
      {
        "blankLine": "any",
        "prev": ["const", "let", "var"],
        "next": ["const", "let", "var"]
      }
    ]
  }
}
```

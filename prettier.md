## Prettier.

Prettier es un formateador de código que se puede integrar con ESLint. [Documentación](https://prettier.io/docs/en/)

### ¿Cómo instalar Prettier en mi proyecto?

Para instalar Prettier en nuestro proyecto, tenemos que ejecutar el siguiente comando:

```bash
npm i -D -E prettier
```

De esta manera, estamos instalando Prettier como una dependencia de desarrollo y en su versión exacta.

### ¿Cómo configurar las reglas de Prettier?

Para configurar las reglas de Prettier, crearemos el archivo `.prettierrc` y colocaremos las reglas que queremos que nuestro proyecto siga. [Opciones Prettier](https://prettier.io/docs/en/options)

#### Ejemplo de un `.prettierrc`.

```json
{
  "arrowParens": "always",
  "bracketSameLine": false,
  "bracketSpacing": true,
  "endOfLine": "auto",
  "jsxSingleQuote": true,
  "printWidth": 100,
  "quoteProps": "as-needed",
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "useTabs": false
}
```

### ¿Cómo ignoramos carpetas y/o archivos?

Al igual que ESLint, Prettier tiene su propio archivo para ignorar carpetas/archivos y se crea en nuestro directorio raíz un archivo `.prettierignore`

### ¿Cómo ejecutamos Prettier?

Para utilizar Prettier, podemos usar los siguientes comandos:

```bash
npx prettier . --check
```

El comando con el flag `--check` ejecutará Prettier en nuestra carpeta raíz y verificará que todos los archivos tengan el formato correcto (ideal para nuestro CI).

```bash
npx prettier . --write
```

El comando con el flag `--write` ejecutará Prettier en nuestra carpeta raíz, verificará que tengan el formato correcto y, de no ser así, aplicará las reglas de formato que tenemos configuradas. Ahora podemos crear los siguientes scripts en nuestro `package.json`

```json
{
  "scripts": {
    "format": "prettier . --check",
    "format:fix": "prettier . --write"
  }
}
```

### ¿Qué hago con las reglas que tienen conflictos con ESLint?

Esto es un problema común al querer integrar Prettier y ESLint. La documentación de Prettier nos recomienda instalar `eslint-config-prettier`.

```bash
npm i -D -E eslint-config-prettier
```

Una vez instalado, tenemos que agregar lo siguiente en nuestro `.eslintrc.json`:

```json
{
  "extends": ["prettier"]
}
```

Ahora vamos a ejecutar el siguiente comando para verificar en nuestra carpeta raíz qué reglas están en conflicto entre ESLint y Prettier:

```bash
npx eslint-config-prettier ./*.*
```

_Recomiendo que las reglas de formato se las dejemos a Prettier y que ESLint se ocupe en su mayoría de buscar errores_

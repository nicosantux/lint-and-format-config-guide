# Configurar Visual Studio Code para utilizar ESLint y Prettier.

Para poder tener una buena experiencia de desarrollo utilizando ESLint y Prettier en vscode, te recomiendo que instales los siguientes plugins:

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

## Agregar una configuración al workspace.

Visual Studio Code nos permite crear una carpeta en la raiz de nuestro proyecto llamada `.vscode` a la que le podemos agregar configuraciones que apliquen a ese proyecto. De esta manera al estar trabajando en equipo nos podemos garantizar de que todxs estemos utilizando la misma configuración.

### Agregar configuración de Prettier y ESLint al workspace.

Crearemos dentro de `.vscode` un archivo llamado `settings.json` con la siguiente configuración:

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

De esta manera haremos que cada vez que guardemos un archivo se de formato al archivo guardado y se ejecute el fix de ESLint.

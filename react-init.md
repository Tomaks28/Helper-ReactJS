# Initialisation d'un projet ReactJS version 17 - Update 11 Mai 2021

1) Créer le répertoire du projet

```zsh
mkdir <NOM_DU_PROJET>
cd <NOM_DU_PROJET>
git init
```

Créer les fichiers et répertoires suivants

```
touch .gitignore
mkdir src
mkdir build
```

2) Ajouter dans **.gitignore** 

```
build
node_modules
```

3) Créer un fichier **package.json**

```
npm init -y
```

4) Créer un fichier **index.html**

```zsh
touch src/index.html
```

et copier le contenu suivant 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NOM_PROJET</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

5) Installer les dépendances de React
```zsh
npm install react react-dom
```

6) Installer les dépendances typescript

```zsh
npm install -D typescript @types/react @types/react-dom
```

Créer le fichier **tsconfig.json**

```js
{
  "compilerOptions": {
    "target": "ES2020" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "ESNext" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "moduleResolution": "node" /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */ /* Type declaration files to be included in compilation. */,
    "lib": [
      "DOM",
      "ESNext"
    ] /* Specify library files to be included in the compilation. */,
    "jsx": "react-jsx" /* Specify JSX code generation: 'preserve', 'react-native', 'react' or 'react-jsx'. */,
    "noEmit": true /* Do not emit outputs. */,
    "isolatedModules": true /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */,
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking of declaration files. */,
    "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */,
    "resolveJsonModule": true
    // "allowJs": true /* Allow javascript files to be compiled. Useful when migrating JS to TS */,
    // "checkJs": true /* Report errors in .js files. Works in tandem with allowJs. */,
  },
  "include": ["src/**/*"]
}
```

7) Créer le point d'entrée de notre application

```zsh
touch src/index.tsx
touch src/App.tsx
```

Copier dans le fichier **index.tsx**

```tsx
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(<App />, document.getElementById("root"));
```

Copier dans le fichier **App.tsx**

```tsx
export default function App() {
  return <></>;
}
```

8) Installer le plugin Babel

Babel sert à transpiler le code javascript moderne par du code compréhensible par le navigateur

```zsh
npm install -D babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/plugin-transform-runtime
```

Copier dans le fichier **.babelrc**

```json
{
  "presets": [
    "@babel/preset-env",
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ],
    "@babel/preset-typescript"
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "regenerator": true
      }
    ]
  ]
}
```

9) Installation et configuration de webpack 5

Webpack sert à bundle tout le code dans le fichier index.html dans le dossier build

```
npm install -D webpack webpack-cli webpack-dev-server html-webpack-plugin style-loader css-loader 
```

Créer un fichier **webpack**

```zsh
touch webpack/webpack.common.js
```

Copier le code suivant. Explication :
- il faut spécifier le fichier d'entrée de notre application (index.tsx)
- il faut mettre dans l'ordre, les resolveurs (tsx,ts,js)
- webpack doit utiliser **babel-loader** pour tous les fichiers (ts|js)x et exclure node_modules
- webpack doit utiliser **style-loader** et **css-loader** pour tous les fichiers css
- webpack doit utiliser **asset/ressource** pour tous les fichiers images
- webpack doit utiliser **asset/inline** pour tous les fichiers svg
- tout doit être généré dans le fichier bundle.js dans le dossier build
- il faut utiliser le plugin webpack pour injecter le fichier bundle.js dans le fichier index.html

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: path.resolve(__dirname, '..', './src/index.tsx'),
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  module: {
    rules: [
      {
        test: /\.(ts|js)x?$/,
        exclude: /node_modules/,
        use: [
          {
            loader: 'babel-loader',
          },
        ],
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: 'asset/resource',
      },
      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: 'asset/inline',
      },
    ],
  },
  output: {
    path: path.resolve(__dirname, '..', './build'),
    filename: 'bundle.js',
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, '..', './src/index.html'),
    }),
  ],
  stats: 'errors-only',
}
```

Créer un fichier **declaration.d.ts** à la racine

```ts
declare module "*.png"
declare module "*.svg"
```

Créer un fichier **webpack/webpack.dev.js** et copier 

```js
const webpack = require('webpack')
const ReactRefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin')

module.exports = {
  mode: 'development',
  devtool: 'cheap-module-source-map',
  devServer: {
    hot: true,
    open: true,
  },
  plugins: [
    new ReactRefreshWebpackPlugin(),
    new webpack.DefinePlugin({
      'process.env.name': JSON.stringify('Vishwas'),
    }),
  ],
}
```


10) scripts dans package.json

Ajouter le script suivant dans le fichier package.json

```json
{
"scripts": {
"start": "",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
}
```

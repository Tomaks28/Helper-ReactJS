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

# bbl-azure-serverless
* 12@13 Retour d'expérience Serverless sur Azure

La présentation se trouve dans le fichier presentation.markdown

## Setup

L'affichage des slides utilise le framework de présentation
[RevealJS](https://revealjs.com/)
ajouté en tant que submodule git au projet.

Pour récupérer le code source

```bash
$ git clone --recurse-submodules https://github.com/yvzn/bbl-azure-serverless.git
$ cd bbl-azure-serverless
```

## Démarrage

Pour lancer le serveur (par exemple avec NodeJS)

```bash
$ npx http-server -p 8080
```

Autre solution (avec Python)

```bash
$ python -m http.server 8080
```

Puis ouvrir <http://127.0.0.1:8080/?showNotes=separate-page> dans le navigateur

Ou pour la version imprimable <http://127.0.0.1:8080/?print-pdf&showNotes=separate-page>

## Licensing

Cette présentation utilise du code source libre sous licence MIT (RevealJS)
ou BSD (Highlight.js) ainsi que des images libres sous licence Creative Commons (CC0)



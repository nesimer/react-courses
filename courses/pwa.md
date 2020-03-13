# Les Progessive Web App

---

### Kezako?

---

C'est un site Internet ayant intégré des fonctionnalités jusqu’alors réservées aux applications mobiles

---

Le terme a été créé par Frances Berriman et Alex Russell (ingénieur chez Google) pour désigner des pages web pouvant être utilisées comme des applications natives, ou presque

---

C'est des applications légères et rapides mais n'ayant pas les mêmes accès au device qu'une application native

C'est un bon compromis pour se concentrer sur l'essentiel

---

Tous l'aspect synchronisation et connectivité est géré par des services workers qui travaillent en arrière-plan et qui chargent les données quand le reseau le permet et la stocke en mémoire pour la restituer

---

Sur notre précédente appli pokemon, juste une manipulation pour la transformer en PWA de base 😃

---

Dans le dossier `public`, éditer le fichier `manifest.json` en changeant le `name` et `short_name` (possibilité de modifier les icones aussi)

Puis dans l'`index.js`,

```js
serviceWorker.unregister(); //old

serviceWorker.register(); //new
```

---

Builder votre app et livré là sur `surge`

Via votre smartphone aller sur votre app et attendez de voir apparaître un bandeau proposant de l'ajouter à l'accueil 😉 🎉

---

> Simple, non?

---

React nous a tout préparés à l'avance mais le grand principe c'est la présence du `manifest.json`

Les navigateurs le reconnaissent et savent qu'il s'agit alors d'une PWA

---

Attention par contre au découpage du code et au lazy loading pour que l'app reste performante

---

You turn !

---

Créer une application react PWA qui affiche une carte et sur laquelle on peut ajouter/supprimer des points géographiques

- Elle doit avoir un écran d'accueil avec un lien vers l'écran avec la map ( 🤔 Router ?)
- Elle doit avoir un thème règlable de n'importe où sur l'application ( 🤔 Context ? )
- Sur l'écran de map, on doit avoir 2 composants, une map et un bandeau au dessus de la map qui affiche le nombre de markers posés ( 🤔 State Complex ?)

Faites le sérieusement vous êtes filmés 📸 !

😱 Euh notés 💯 !

🏁 💥

---

[Correction](https://github.com/millehorde/map-pwa-react)

# Découverte de React

---

Sommaire:
- Introduction
- Hello world
- Composants dynamiques et réutilisables
- Utiliser les hooks d'états
- Fetch d'une api
- Deployer son application

---

## Introduction

----

Sujet vaste 😰

Plusieurs technologies viennent amplifier l'apprentissage (Webpack, Redux, CSS in JS, TS, etc...)

----

##### Objectifs

Découvrir les bases de React pour être autonome

----

##### Nous aborderons

- Ce qu'est React et pourquoi cela vaut la peine de s'y pencher 😉
- Comment obtenir quelque chose sur une page avec des composants et du JSX
- Passage de données dans nos composants avec des `props`
- Rendre nos composants React interactifs entre eux avec des states et des hooks
- Consommer une api avec des hooks d'effets (`useEffect`)
- Deployer sa première app 🎉

----

##### ⚠️ Nous ne verrons ici que du code JS vanilla

> Savoir utiliser l'outil seul avant de le plugger avec d'autres 😉

---

### What is React ? 🤔

----

`React = UI Lib by Facebook`

=> Créer des applications web avec des composants interactifs

----

Composant React = morceaux de contenu et de logique réutilisables sur une page web

> Pour les HTML lovers, un composant est comme un tag HTML custom

----

Un composant est écrit comme une vraie fonction JS et ça en est !

React utilise une syntaxe proche du HTML mais qui est transformée en JS par un compilateur.

```jsx
const MyCustomDiv = ({text}) => {
  return <div style={{ backgroundColor: 'red' }}>
    {text}
  </div>
}
```

----

Une page React est composée de composants imbriqués (tout comme le HTML).

Ces composants peuvent à la fois contenir d'autres composants ou des éléments natifs HTML (`div`, `button`, etc...)

----

Un des fondements de base de React (mis en avant dès sa sortie en 2013) est l'idée de flux de données à sens unique (**One-way data flow**).

Les données ne sont transmises que d'un composant parent vers ses enfants et pas disponibles ailleurs.

> React est explicite et restrictif de ce point de vue.

---

### Ce qu'il faut savoir ... 😎

React est une bibliothèque, mais en plus de son lot de méthodes à disposition, il embarque aussi une nouvelle syntaxe: le `JSX`

Sans maitrise du JS, le code JSX peut être difficile à lire (différence entre _React things_ et le pure JS)

----

En matière de style, React s'adopte aussi bien avec des feuilles CSS classiques qu'avec des libs plus orientés CSS-in-JS

----

> Si vous avez un problème en React dites-vous que d'autres l'ont eu aussi, Google is your friend 😉

---

Entrons dans le vif du sujet avec un petit "hello world !" des familles 👍

----

1. Créer un nouveau dossier 📁 `helloWorld`

2. Se placer à l'intérieur avec votre terminal

3. Faites `yarn init` ou `npm init` (Ne répondez pas aux diverses questions on s'en moque)

4. Ajouter les dépendances `react` et `react-dom`

5. Créer un fichier vide 📄 `index.jsx`

----

6. Ecrire ce bout de code dans ce fichier:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

const App = () => {
  return <div>Hello World!</div>;
}

ReactDOM.render(<App/>, document.querySelector('#root'));
```

----

Et maintenant ?

----

`node index.jsx` ?

----

NON 😱

Mais quoi alors??

----

React est destiné à l'affichage de page web donc à être exécuté dans un environnement browser.

Il faut donc que ce code soit converti en js utilisable dans une page html.

----

Pour cela on va utiliser quelques outils 😉

[Webpack](https://webpack.js.org/) et [Babel](https://babeljs.io/) 🎉

----

7. Ajouter les dépendances suivantes `webpack webpack-cli webpack-dev-server html-webpack-plugin @babel/core babel-loader @babel/preset-env @babel/preset-react` en mode dev
8. Créer un dossier 📁 `src` et déplacer le fichier `index.jsx` à l'intérieur et le renommer `index.js`
9. Ajouter cette configuration webpack dans un fichier `webpack.config.js` à la racine du projet

----

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // the output bundle won't be optimized for production but suitable for development
  mode: 'development',
  // the app entry point is /src/index.js
  entry: path.resolve(__dirname, 'src', 'index.js'),
  output: {
  	// the output of the webpack build will be in /dist directory
    path: path.resolve(__dirname, 'dist'),
    // the filename of the JS bundle will be bundle.js
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
      	// for any file with a suffix of js or jsx
        test: /\.jsx?$/,
        // ignore transpiling JavaScript from node_modules as it should be that state
        exclude: /node_modules/,
        // use the babel-loader for transpiling JavaScript to a suitable format
        loader: 'babel-loader',
        options: {
          // attach the presets to the loader (most projects use .babelrc file instead)
          presets: ["@babel/preset-env", "@babel/preset-react"]
        }
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  // add a custom index.html as the template
  plugins: [new HtmlWebpackPlugin({ template: path.resolve(__dirname, 'src', 'index.html') })]
};
```

----

10. Créer un fichier `index.html` dans `src` et y mettre :

```html
<html>
  <head>
    <title>Hello world App</title>
  </head>
  <body>
  <div id="root"></div>
  <script src="bundle.js"></script>
  </body>
</html>
```

----

11. Nettoyons un peu le code et mettre dans le fichier `index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom';
import HelloWorld from './components/HelloWorld';

ReactDOM.render(<HelloWorld/>, document.querySelector('#root'));
```

----

12. Créer un dossier `components` puis un fichier `HelloWorld.jsx` avec:

```jsx
import React from 'react'

const HelloWorld = () => {
  return <div>Hello World!</div>;
}

export default HelloWorld
```

----

Et maitenant? Que vais-je faire? 🎵

----

13. Une petite mise à jour de notre `package.json` pour ajouter nos nouveaux scripts

```json
{
  ...,
  "scripts": {
    "start": "webpack-dev-server --open",
    "build": "webpack"
  },
  "devDependencies": {
    ...
  }
}
```

----

```sh
yarn start
npm start
```

And 🎉

---

#### À savoir

Webpack a pour but de lancer la transpilation des différents fichiers jsx vers du js pur et d'optimiser tous les assets de la page web avant de les mettre à disposition sur un serveur de dev

----

Babel s'occupe de la transpilation à proprement parler

```jsx
const HelloWorld = () => {
  return <div>Hello World!</div>;
}
```

deviendra

----

du code JS pure qui est integrable dans une page web comme étant un script

```js
const HelloWorld = () => {
  return React.createElement('div', 'Hello World!');
}
```

----

Dans l'`index.js`, ce n'est pas à nous d'appeler nos composants, c'est React qui s'en occupe

```js
ReactDOM.render(React.createElement(HelloWorld), document.querySelector('#root'));
```

---

### You turn !

----

À partir de notre hello world, enrichissez le code pour ajouter ces fonctionnalités:
- Changer le "Hello World!" en "Hello \<your_name\>!" (ex: "Hello Nicolas!" en dur pas encore en dynamique 😉)
- Mettre le prénom en gras à l'aide d'une balise `<strong>`
- Ajouter d'autres élements HTML à l'intérieur de la `div` et en dessous de la string "Hello \<your_name\>!"

----

- Insérer du js entre les balises (le js est interprété lorsque le statement est encapsulé dans des accolades ex: `<span>{5 + 10}</span>`)
- Ajouter du style via un fichier css que vous importer dans le HelloWorld.jsx au début `import './HelloWorld.css'`

  Définir une classe dans ce css et l'utiliser via l'attribut `className` de la `div` (ou autres éléments)

---

À ce stade vous pouvez reproduire n'importe quel site statique, bravo ! 👏 👏

---

## Composants dynamiques et réutilisables

----

```js
const saluer = () => "Salut Nicolas"

||

function saluer() {
  return "Salut Nicolas";
}
```

----

Si on veut que le prénom soit dynamique on obtient

```js
const saluer = (name) => {
  return "Salut " + name // `Salut ${name}`
}

console.log(saluer("Nicolas")) // Salut Nicolas
console.log(saluer("Roland")) // Salut Roland
```

----

> Comment peut-on faire pareil avec React ???

----

Réponse: LES PROPS !!!! 🎉

```js
function Hello() {
  return <div>Hello World!</div>;
}
```

```js
function Hello(props) {
  return <div>Hello {props.name}!</div>;
}
```

----

Mais comment cela s'utilise lors de l'appel du composant ?? 🤔

----

Comme un attribut d'une balise HTML

```jsx
<Hello name="Nicolas"/>

<Hello name="Roland"/>
```

----

On peut y passer aussi des objets ou des fonctions pas que des strings

Dans ce cas, on utilise les accolades pour spécifier qu'on quitte la syntaxe HTML-like pour du JS

```jsx
<Hello name={(() => 'Nicolas')()}/>
```

----

⚠️ Le nom que vous donner à l'attribut est aussi le nom de la clé pour recupérer sa valeur dans l'objet `props`

```jsx
<Hello firstName={'Nicolas'} lastName="Remise"/>

function Hello(props) {
  return <div>Hello {props.firstName} {props.lastName}!</div>;
}
```

----

#### Différentes versions similaires du composant

----

Plain JS function sans destructuring

```jsx
function Hello(props) {
  return <div>Hello {props.name}!</div>;
}
```

----

Plain JS function avec destructuring

```jsx
function Hello({ name }) {
  return <div>Hello {name}!</div>;
}
```

----

Fonction anonyme dans une constante

```jsx
const Hello = function ({ name }) {
  return <div>Hello {name}!</div>;
}
```

----

Arrow Function dans une constante

```jsx
const Hello = ({ name }) => {
  return <div>Hello {name}!</div>;
}
```

----

Arrow Function sans return car explicite

```jsx
const Hello = ({ name }) => <div>Hello {name}!</div>;
```

---

You turn !

----

Ecrire un composant `MediaCard` qui accepte 3 `props`, un titre, un body et une url d'image.
Le titre doit être rendu dans un `h3`, le body dans un `p` et l'url dans une balise `img`.

> Peux-t-on les retourner directement tous les 3 ou doit-on wrapper ces 3 balises ?

----

Intégrer ce composant dans le `Render` de l'`index.js`

> Peux-t-on passer du jsx plutôt que du texte uniquement dans les props ? (passer des \<strong\> ou des \<i\>)

---

### Utiliser les hooks d'états => `useState`

----

Transformer un composant sans état en composant avec état 🔥

----

Un petit exemple avant les explications 😉

----

Créer un nouveau projet ou directement télécharger le repo [UseState-Example](https://github.com/millehorde/useState-example-react)

----

```jsx
import React from 'react'

const Room = () => {
  return (
    <div className="room">La chambre est disponible</div>
  );
}

export default Room
```

----

On souhaite transformer ce composant pour que l'on puisse changer l'état de la chambre sur le click d'un bouton.

----

La chambre a donc 2 états possibles: soit elle est occupée, soit elle est disponible.

----

Un boolean donc... 🤔

----

React nous met à disposition un hook appelé `useState`

----

> Qu'est-ce qu'un hook ?

Un Hook est une fonction qui permet de « se brancher » sur des fonctionnalités React.

----

`useState` est un Hook qui permet d’ajouter l’état local React à des fonctions "composant".

Ainsi nos états sont gérés entièrement par React et donc son cloisonnée à chaque instance de composant et persistent durant la vie du composant.

----

`useState` prend en argument la valeur de l'état à l'initialisation

----

`useState` retourne un tableau dont le premier élément est l'état en question et le second la méthode permettant de le modifier de manière sûre et cloisonnée

----

```jsx
const [count, setCount] = React.useState(0)

const [isLit, setIsLit] = React.useState(true)
```

----

Il existe de nombreux hooks différents au sein de React (`useEffect`, `useCallback`, `useContext`, etc...)

Il est même possible de faire ses propres hooks custom permettant de faire du code réutilisable

----

⚠️ 2 règles à respecter avec les hooks

----

⚠️

1. Appelez les Hooks uniquement au niveau racine

N’appelez pas de Hooks à l’intérieur de boucles, de code conditionnel ou de fonctions imbriquées

----

⚠️

2. Appelez les Hooks uniquement depuis des fonctions React

N’appelez pas les Hooks depuis des fonctions JavaScript classiques

----

Revenons à notre exemple

----

Sans état:

```jsx
import React from 'react'

const Room = () => {
  return (
    <div className="room">La chambre est disponible</div>
  );
}

export default Room
```

----

Avec un état:

```jsx
import React from 'react'

const Room = () => {
  const [isLit, setIsLit] = React.useState(true)

  return (
    <div className="room">La chambre est disponible</div>
  );
}

export default Room
```

----

> On a maintenant un état et puis quoi? On fait quoi?

----

On peut faire du "conditional rendering"

```jsx
import React from 'react'

const Room = () => {
  const [isLit, setIsLit] = React.useState(true)

  return (
    <div className="room">La chambre est {isLit ? 'disponible' : 'occupée'}</div>
  );
}

export default Room
```

----

On peut faire changer ce state via un bouton pour que notre conditional rendering précédent soit effectif

----

```jsx
const Room = () => {
  const [isLit, setIsLit] = React.useState(true)

  return (
    <div className="room">
      La chambre est {isLit ? 'disponible' : 'occupée'}
      <button onClick={() => setIsLit(prev => !prev)}>Changer l'état</button>
    </div>
  );
}
```

----

Démo !

----

Ajoutons du css et rendons-le dynamique aussi !

----

```jsx
const Room = () => {
  const [isLit, setIsLit] = useState(true);

  return (
    <div className="room">
      <div id="status" className={isLit ? 'lit' : 'free'}></div>
      <span>La chambre est {isLit ? 'disponible' : 'occupée'}</span>
      <button onClick={() => setIsLit(prev => !prev)}>
        Changer l'état
      </button>
    </div>
  );
};
```

----

```css
.lit {
  background-color: whitesmoke;
}

.room {
  display: inline-block;
}

.room span,
.room div {
  margin-right: 10px;
  display: inline-block;
}

.room button {
  display: block;
}

#status {
  width: 20px;
  height: 20px;
  border: 1px solid black;
}

.free {
  background-color: grey;
}
```

----

Sympa, non?

----

> Mais comment fonctionne le setter fourni lors du `useState`?

----

Dans la fonction onClick, nous basculons l'état isLit vrai/faux en fonction de son réglage actuel.

----

> Pourquoi pas simplement isLit = !isLit ?

----

La fonction setIsLit a deux tâches:
- d'abord, elle change l'État
- puis elle re-render le composant

----

Si vous vous contentez de modifier directement la variable, React n'a aucun moyen de savoir qu'elle a changé, et elle ne le fera pas
de restitution

----

N'oubliez pas qu'isLit est une vieille variable régulière - pas un truc spécial de React

----

Elle sortira à la fin de la fonction et toute modification de celle-ci serait perdue

----

C'est pourquoi il est important d'appeler le setter, afin que React puisse mettre à jour la valeur de l'état de ce crochet dans les coulisses 😉

---

You turn !

----

À partir de la branche `with-state-3` du repo [UseState-example](https://github.com/millehorde/useState-example-react)

----

🔍Configuration `css`

----

- Ajouter une deuxième chambre à la page
- Ajouter sur chaque chambre un bouton permettant de gérer la lumière (allumée/eteinte) de celles-ci

----

- Ajouter une notion de température (initialisé à 22°C) sur les chambres et la visualiser
- Ajouter 2 boutons (+/-) pour pouvoir augmenter/diminuer la température d'une chambre

---

On a vu:
- la création d'un composant
- le passage de props pour rendre customisable un composant
- la gestion d'état pour rendre dynamique un composant
- la gestion du style

----

> Que pouvons-nous encore voir d'utile avant de passer au PWA?

---

### Le fetching des données

----

> Comment obtenir des données à partir d'une API ?

----

React n'a aucun outil privilégié pour faire cela

C'est une lib UI pas un lib de fetching 😉

----

Pour un composant React, la récupération de quelque chose sur un serveur est un effet secondaire.

Pour lui c'est :"Après que j'ai fini, je vais lancer un appel pour obtenir des données"

----

Une fois que ces données reviennent, elles doivent être mises dans l'état, puis vous pouvez les restituer à partir de là

----

On peut compliquer ce processus avec des services et des modèles de données, des `redux-thunk` et des sagas

Mais tout se résume à des éléments qui rendent des props et des states

----

Pour faire tout ça, nous avons donc besoin d'une lib HTTP

Il en existe des tonnes

Nous utiliserons `axios` pour sa facilité

----

Utiliser `axios` nous permettra de voir comment utiliser une lib externe dans nos composants

😉

----

On va utiliser l'api `pokeapi.co` pour afficher une liste de pokémons et lorsqu'on en sélectionnera un on affichera ses détails dans un panel à côté.

----

Cloner le repo [PokeExample-react](https://github.com/millehorde/poke-example-react)

----

Ajouter `axios`

----

Importer `axios` dans le composant `PokemonList`

```js
import axios from 'axios'
```

----

#### useEffect

----

Le Hook d’effet permet l’exécution d’effets de bord dans les fonctions composants

> Tiens mais un appel API c'est un effet de bord, non?

----

> Que fait useEffect ?

On utilise ce Hook pour indiquer à React que notre composant doit exécuter quelque chose après chaque affichage

React enregistre la fonction passée en argument et l’appellera plus tard, après avoir mis à jour le DOM

Elle est exécutée par défaut après le premier affichage et après chaque mise à jour

----

```js
useEffect(() => setIsLit(false))
```

Remettra la chambre comme occupée après chaque mise à jour du composant

----

```js
useEffect(() => setIsLit(false))

useEffect(() => setTemperature(10))
```

On peut utiliser plusieurs `useEffect` dans le meme composant pour splitter le code

----

```js
useEffect(() => setIsLit(false))
```

Ce code est appelé à chaque mise à jour

----

Ce hook prend un second paramètre qui correspond à un tableau d'abonnement aux changements

```js
useEffect(() => isLightOn && setIsLit(false), [isLightOn])
```

Ici, la fonction passée dans le `useEffect` ne sera appelé que lorsque `isLightOn` est modifié

----

```js
useEffect(() => isLightOn && temperature>20 && setIsLit(false), [isLightOn, temperature])
```

On peut passer autant d'abonnements que voulut 😃

----

**Astuces**

```js
useEffect(() => setIsLit(false), [])
```

Mettre un tableau vide permettra que le code ne soit appelé qu'a l'initialisation du composant pas après

----

Si vous désirez que du code soit appelé pour nettoyer le composant lors de sa destruction

Faites retourner une fonction dans le callback de votre `useEffect`

```js
useEffect(() => {return () => console.log('je me detruis')})
```

----

Revenons à nos pokémons

----

```jsx
const PokemonList = () => {
  const [pokemons, setPokemons] = useState([]);
  const [next, setNext] = useState(undefined);
  const [previous, setPrevious] = useState(undefined);
  const [url, setUrl] = useState('https://pokeapi.co/api/v2/pokemon')

  useEffect(()=>{
    axios.get(url).then(({data: {results, next, previous}})=>{
      setPokemons(results)
      setNext(next)
      setPrevious(previous)
    })
  }, [url])

  return (
    <div>
      <ul className="pkmnList">
        {pokemons.map(pkmn => (
          <li key={pkmn.name}>{pkmn.name}</li>
        ))}
      </ul>
      <div style={{display: 'inline'}}>
        <button disabled={!previous} onClick={()=>setUrl(previous)}>⬅️ Previous</button>
        <button disabled={!next} onClick={()=>setUrl(next)}>Next ➡️</button>
      </div>
    </div>

  );
```

---

You turn !

----

Ajouter une `div` en dessous de la liste de pokémon affichant les détails du pokémon sur lequel on clique

Pour l'API, les informations sont sur la doc en ligne 😉([API](https://pokeapi.co))

---

### Deployer sa web-app

----

Il est recommandé pour que sa web-app soit bien construite de passer par l'utilitaire de creation d'app de React

`create-react-app`

----

Pour l'installer:

```bash
yarn global add create-react-app

npm add -g create-react-app
```

----

Pour créer un projet:

```sh
npx create-react-app <my-app>
cd <my-app>
npm start
```

----

Avec cet outil, recréer un projet avec la liste des pokémons

----

Pour l'hébergement, nous allons utiliser `surge.sh`

----

Installer `surge`

```sh
yarn global add surge
npm add -g surge
```

----

Builder notre app via un `yarn build` ou `npm run build`

----

Saisir

```sh
cd build
surge
```

----

Créer ou se connecter en suivant le prompt

Et accéder à son projet en ligne 🎉

----

Il s'agit d'un exemple avec Surge mais il existe beaucoup d'autres plateformes de déploiement

> Google is your friend 😉

---

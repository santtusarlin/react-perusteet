# React-perusteet

### 1. [Yleistä](#yleistä)
### 2. [Kehitysympäristö ja asennus](#kehitysympäristö-ja-asennus)
### 3. [JSX](#jsx)
### 4. [Komponentit](#komponentit)
>#### 4.1 [Functional komponentit](#functional-komponentit-(stateless))
>#### 4.2 [Class komponentit](#class-komponentit-(stateful))
>#### 4.3 [Props](#props)
>#### 4.4 [State](#state)
>#### 4.5 [Routing](#routing)
### 5. [Tutoriaaleja](#tutoriaaleja)
### 6. [Tehtävä](#tehtävä)

## Yleistä
[React](https://reactjs.org/) on komponenttipohjainen JavaScript-kirjasto web-käyttöliittymien tekoon, jonka kehityksen taustalla toimii Facebook. (ei sovelluskehys kuten Angular)

React sovellusten rakenne perustuu kokonaisuuden jakoon useisiin pieniin komponentteihin, jotka ovat toisistaan riippumattomia ja tarvittavat tiedot injektoidaan ylemmän tason komponenteista alenpiin.

MVC-mallin näkökulmasta React sisältää vain Viewin. Isompiin sovelluksiin usein hyödynnetään Flux-arkkitehtuuria tai Redux-kirjastoa datan hallintaan.

React käyttöliittymän nopeus perustuu virtuaaliseen DOM:in. 
Käytännössä siis muutoksen tapahtuessa React luo kopion aidosta DOM:sta, jonka jälkeen se havaitsee muutokset ja päivittää tarvittavat komponentit virtual DOM:ssa. Lopuksi React kopioi vain muuttuneet uudet osat virtuaalisesta DOM:sta aitoon DOM:in, eikä joudu siis kokonaan uudelleenkirjoittamaan sitä.

## Kehitysympäristö ja asennus

Reactia voi periaatteessa kehittää lähes kaikilla yleisimmällä IDE:llä tai koodi-editoreilla, Microsoftin Visual Studio Codea suosittelen itse.

Reactin käyttö vaatii että Node.js on asennettuna.

```
// luo react sovellus
npx create-react-app my-app

cd my-app

//käynnistä sovellus localhost:3000
npm start
```

## JSX

React kehityksessä käytetään ES6/JSX (JavaScript XML) koodia. Yksinkertaisesti selitettynä JSX on muuten syntaksiltaan samanlaista kuin JavaScript, mutta se sallii HTML tagien käytön JavaScriptin sisällä. HTML koodi käännetään Babelin avulla JavaScriptiksi.

Esim. JSX koodista
```
var nav = (
    <ul id="nav">
      <li><a href="#">Home</a></li>
      <li><a href="#">About</a></li>
      <li><a href="#">Clients</a></li>
      <li><a href="#">Contact Us</a></li>
    </ul>
);
```

Babelin käännetyä sen JavaScriptiksi se näyttää tältä:
```
var nav = React.createElement(
   "ul",
   { id: "nav" },
   React.createElement(
      "li",
      null,
      React.createElement(
         "a",
         { href: "#" },
         "Home"
      )
   ),
   React.createElement(
      "li",
      null,
      React.createElement(
         "a",
         { href: "#" },
         "About"
      )
   ),
   React.createElement(
      "li",
      null,
      React.createElement(
         "a",
         { href: "#" },
         "Clients"
      )
   ),
   React.createElement(
      "li",
      null,
      React.createElement(
         "a",
         { href: "#" },
         "Contact Us"
      )
   )
);
```

Esimerkki asettaa ```h1```-tageihin JavaScript funktion palauttaman arvo.
```
function format(object) {
  return object.hello + ' ' + object.world;
}

const object = {
  hello: 'Hello',
  world: 'World'
};

const element = (
  <h1>{format(object)}!</h1>
);
```

## Komponentit

Reactissa käytetään kahdenlaisia komponentteja, tilallisia sekä tilattomia.

### Functional komponentit (Stateless)

Funktionaalinen komponentti on periaatteessa vain puhdas JavaScript funktio, joka ottaa propsit argumenttina ja palauttaa react elementin.

Funktionaalisella komponentilla ei ole statea eikä lifecycleä joten ```hooks```eja ei voida käyttää sen sisällä.

Funktionaalisia komponentteja tulisi käyttää esim. sellaisiin komponentteihin jotka vain renderöivät propseja ja silloin kun komponentilla ei ole mitään tarvetta statelle, lifecycle hookeille tai sisäisille muuttujille.

```
const ChildComponent = (props) => {
    return (
    <div className="ChildBox">
        <h2 className="textWhite">{props.text}</h2>
    </div>
    );
}

export default ChildComponent;
```

### Class komponentit (Stateful)

Class-komponenttia tulisi taas käyttää silloin kun komponentti tarvitsee statea datanhallintaan tai esim. sisältää elementtejä jotka ottavat käyttäjältä inputin vastaan.

Stateful komponentit ovat siis Reaktiivisia, eli ne päivittävät DOM:ia tarvittaessa.

```
import React from 'react';
import ChildComponent from './childComponent';

class ParentComponent extends React.Component {

  render() {
    return(
    <div className="ParentBox">
      <h1>ParentComponent.</h1>
      <ChildComponent text={"I'm the 1st ChildComponent"} />
      <ChildComponent text={"I'm the 2nd ChildComponent"} />
    </div>
    )
  }
}

export default ParentComponent;
```

### Props

React-komponenteille voidaan määrittää ```props```eja, joiden avulla komponenttien välillä saadaan siirrettyä dataa.
Propsit ovat yksisuuntaista, read-only dataa ja niitä voidaan välittää vain Parent-komponentilta Child-komponentille.

Määritellään ChildComponentille attribuutti ```text```.
```
class ParentComponent extends Component {
  render() {
    return(
    <div>
    <h1>I'm the parent component, a STATEFUL class.</h1>
    <ChildComponent text={"I'm the 1st child, text handed as props from the Parent Component"} />
    <ChildComponent text={"I'm the 2nd child"} />
    <ChildComponent text={"I'm the 3rd child"} />
    </div>
    )
  }
}
```

Otetaan ChildComponentissa attribuutti ```text``` vastaan propsina.
```props``` palauttaa olion, joten voimme viitata sen elementteihin piste(.)-notaatiolla.
```
const ChildComponent = (props) => {
    return (
    <h2>{props.text}</h2>
    );
}

```

### State

Reactin komponentit sisältvät ```state``` objektin, johon voi tallentaa komponentille kuuluvaa dataa.
Kun ```state``` muuttuu, komponentti uudelleenrenderöidään.
```State``` voidaan alustaa joko komponentin konstruktorissa tai suoraan luokan sisällä.

Konstruktorissa alustettu ```state``` esimerkki. ```super(props)``` vaaditaan aina.

```
class ParentComponent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }
}
```

Tai vaihtoehtoisesti ilman konstruktoria. Viittaukset ```props```eihin toimii silti.
```
class ParentComponent extends Component {
  state = {
    count: 0
  }
}
```

### Routing

Reactista löytyy valmis komponentti "React Router", jolla on helppo toteuttaa React-sovelluksen navigaation hallinta.
```
npm install react-router-dom
```

React Router on hyvin helppokäyttöinen, alla on toteutettu yksinkertainen SPA-navigaatio.
```
<Router>
  <div className="LinkBar">
    <table>
      <tr>
        <td><Link className="NavLink" to="/home">Home</Link></td>
        <td><Link className="NavLink" to="/about">About</Link></td>
        <td><Link className="NavLink" to="/contact">Contact</Link></td>
      </tr>
    </table>
  </div>
  <Route path="/home" render={() => <Home />} />
  <Route path="/about" render={() => <About />} />
  <Route path="/contact" render={() => <Contact />} />
</Router>
```

## Tutoriaaleja

Tässä vielä pari linkkiä hyviin tutoriaaleihin.

https://fullstackopen.com/about/

https://reactjs.org/tutorial/tutorial.html

## Tehtävä

Kehitä jokin haluamasi pieni React sovellus joka sisältää vähintään seuraavat:
* Funktionaalinen komponentti
* Luokka-komponentti
* Propseja hyödynnetty vähintään kerran
* Statea hyödynnetty vähintään kerran
* Datan syöttö ja sen tulostus tulee löytyä eri komponenteista

Ratkaisu voisi olla esim. yksinkertainen TO-DO/muistiinpano sovellus.

### Lisähaaste
Jos haluat tehtävään hieman haastetta, lisää sovellukseen reititys ja pidä datan syöttö ja tulostus eri komponenteissa sekä reiteissä.
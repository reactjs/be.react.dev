---
title: Апісанне UI
---

<Intro>

React — гэта JavaScript бібліятэка для рэндэрынгу інтэрфейсу карыстальніка (UI). UI пабудаваны з маленькіх адзінак, такіх як кнопкі, тэксты, відарысы. React дазваляе камбінаваць іх у прыдатныя да паўторнага выкарыстання *кампаненты*, якія можна ўкладаць адзін у аднаго. Ад вэб-сайтаў да мабільных праграм — усё на экране можа быць разбітае на кампаненты. У гэтай главе вы даведаецеся як ствараць і змяняць кампаненты React, а таксама як паказваць іх па-рознаму ў залежнасці ад умоў.

</Intro>

<YouWillLearn isChapter={true}>

<<<<<<< HEAD
* [Як напісаць свой першы кампанент React](/learn/your-first-component)
* [Калі і як змяшчаць некалькі кампанентаў у адзін файл](/learn/importing-and-exporting-components)
* [Як дадаць разметку ў JavaScript з дапамогай JSX](/learn/writing-markup-with-jsx)
* [Як выкарыстоўваць магчымасці JavaScript з дапамогай фігурных дужак у разметцы JSX](/learn/javascript-in-jsx-with-curly-braces)
* [Як змяняць канфігурацыю кампанента з дапамогай пропсаў](/learn/passing-props-to-a-component)
* [Як рэндэрыць кампаненты ў залежнасці ад умоў](/learn/conditional-rendering)
* [Як рэндэрыць некалькі кампанентаў за раз](/learn/rendering-lists)
* [Як пазбягаць памылак, захоўваючы кампанент чыстымі](/learn/keeping-components-pure)
=======
* [How to write your first React component](/learn/your-first-component)
* [When and how to create multi-component files](/learn/importing-and-exporting-components)
* [How to add markup to JavaScript with JSX](/learn/writing-markup-with-jsx)
* [How to use curly braces with JSX to access JavaScript functionality from your components](/learn/javascript-in-jsx-with-curly-braces)
* [How to configure components with props](/learn/passing-props-to-a-component)
* [How to conditionally render components](/learn/conditional-rendering)
* [How to render multiple components at a time](/learn/rendering-lists)
* [How to avoid confusing bugs by keeping components pure](/learn/keeping-components-pure)
* [Why understanding your UI as trees is useful](/learn/understanding-your-ui-as-a-tree)
>>>>>>> a0cacd7d3a89375e5689ccfba0461e293bfe9eeb

</YouWillLearn>

## Ваш першы кампанент {/*your-first-component*/}

Праграмы React пабудаваны з ізаляваных кавалкаў UI, якія называюцца *кампанентамі*. Кампанент React — гэта функцыя JavaScript, у якую вы можаце дадаць разметку. Кампанент можа быць маленькім, як кнопка, або вялікім, як цэлая старонка. Вось, напрыклад, кампанент `Gallery`, які рэндэрыць тры кампаненты `Profile`:

<Sandpack>

```js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Выбітныя навукоўцы</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/your-first-component">

Звярніцеся да старонкі «**[Ваш першы кампанент](/learn/your-first-component)**» каб даведацца як аб’яўляць і выкарыстоўваць кампаненты React.

</LearnMore>

## Імпарт і экспарт кампанентаў {/*importing-and-exporting-components*/}

Вы можаце аб’явіць некалькі кампанентаў у адным файле, але ў вялікіх файлах можа быць складана арыентавацца. Каб вырашыць гэтую праблему, вы можаце *экспартаваць* кампанент ва ўласны файл, а потым *імпартаваць* яго з іншага файла:


<Sandpack>

```js App.js hidden
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js Gallery.js active
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Выбітныя навукоўцы</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

```js Profile.js
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```css
img { margin: 0 10px 10px 0; }
```

</Sandpack>

<LearnMore path="/learn/importing-and-exporting-components">

Звярніцеся да старонкі «**[Імпарт і экспарт кампанентаў](/learn/importing-and-exporting-components)**» каб даведацца як падзяляць кампаненты па ўласных файлах.

</LearnMore>

## Напісанне разметкі з дапамогай JSX {/*writing-markup-with-jsx*/}

Кожны кампанент React — функцыя JavaScript, якая можа змяшчаць разметку, якую React будзе рэндэрыць у браўзеры. Кампаненты React выкарыстоўваюць пашырэнне сінтаксісу, якое называецца JSX каб апісваць разметку. Разметка JSX вельмі падобная да HTML, але яна больш строгая і можа паказваць даныя дынамічна.

Не любая HTML разметка будзе працаваць, калі ўставіць яе ў кампанент React:

<Sandpack>

```js
export default function TodoList() {
  return (
    // Гэта не будзе працаваць!
    <h1>Спіс задач Хедзі Ламар</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    >
    <ul>
      <li>Вынайсці новыя святлафоры
      <li>Адрэпетаваць сцэну з фільма
      <li>Палепшыць тэхналогію спектра
    </ul>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

Калі ў вас ёсць існуючая разметка ў HTML, вы можаце скарыстацца [канвертарам](https://transform.tools/html-to-jsx):

<Sandpack>

```js
export default function TodoList() {
  return (
    <>
      <h1>Спіс задач Хедзі Ламар</h1>
      <img
        src="https://i.imgur.com/yXOvdOSs.jpg"
        alt="Hedy Lamarr"
        className="photo"
      />
      <ul>
        <li>Вынайсці новыя святлафоры</li>
        <li>Адрэпетаваць сцэну з фільма</li>
        <li>Палепшыць тэхналогію спектра</li>
      </ul>
    </>
  );
}
```

```css
img { height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/writing-markup-with-jsx">

Звярніцеся да старонкі «**[Напісанне разметкі з дапамогай JSX](/learn/writing-markup-with-jsx)**» каб даведацца як пісаць правільную JSX-разметку.

</LearnMore>

## JavaScript у JSX з дапамогай фігурных дужак {/*javascript-in-jsx-with-curly-braces*/}

JSX дазваляе пісаць разметку, падобную да HTML унутры файлаў JavaScript, тым самым захоўваючы логіку і змесціва побач. Часам можа спатрэбіцца дадаць крыху логікі JavaScript або звярнуцца да дынамічных даных знутры разметкі. У падобнай сітуацыі можна выкарыстоўваць фігурныя дужкі ўнутры JSX каб «адчыніць акенца» ў JavaScript:

<Sandpack>

```js
const person = {
  name: 'Грэгорыа І. Зара',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>Спіс задач {person.name}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Палепшыць відэатэлефон</li>
        <li>Падрыхтаваць лекцыі па аэранаўтыцы</li>
        <li>Папрацаваць над рухавіком на спірце</li>
      </ul>
    </div>
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

<LearnMore path="/learn/javascript-in-jsx-with-curly-braces">

Звярніцеся да старонкі «**[JavaScript у JSX з дапамогай фігурных дужак](/learn/javascript-in-jsx-with-curly-braces)**» каб даведацца як выкарыстоўваць даныя з JavaScript у JSX.

</LearnMore>

## Перадача пропсаў у кампанент {/*passing-props-to-a-component*/}

Для камунікацыі між сабой кампаненты React выкарыстоўваць *пропсы*. Кожны бацькоўскі кампанент можа перадаваць некаторую інфармацыю даччыным, задаючы ім пропсы. Пропсы падобныя на атрыбуты ў HTML, але ў іх вы можаце перадаваць любыя JavaScript значэнні, уключаючы аб’екты, масівы, функцыі і нават JSX!

<Sandpack>

```js
import { getImageUrl } from './utils.js'

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{
          name: 'Кацуко Сарухасі',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

```

```js utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.card {
  width: fit-content;
  margin: 5px;
  padding: 5px;
  font-size: 20px;
  text-align: center;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.avatar {
  margin: 20px;
  border-radius: 50%;
}
```

</Sandpack>

<LearnMore path="/learn/passing-props-to-a-component">

Звярніцеся да старонкі «**[Перадача пропсаў у кампанент](/learn/passing-props-to-a-component)**» каб даведацца як перадаваць і чытаць пропсы.

</LearnMore>

## Умоўны рэндэрынг {/*conditional-rendering*/}

Часта можа ўзнікаць патрэба паказваць у кампанентах розныя рэчы ў залежнасці ад пэўных умоў. У React вы можаце пісаць умовы для рэндэрынгу JSX з дапамогай JavaScript, выкарыстоўваючы ўмоўныя аператары `if`, `&&` або `? :`.

У прыкладзе ніжэй JavaScript аператар `&&` выкарыстоўваецца, каб паказваць птушку ў залежнасці ад умовы:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✔'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд у дарогу</h1>
      <ul>
        <Item
          isPacked={true}
          name="Скафандр"
        />
        <Item
          isPacked={true}
          name="Шалом з залатым лістком"
        />
        <Item
          isPacked={false}
          name="Фота Тэм"
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<LearnMore path="/learn/conditional-rendering">

Звярніцеся да старонкі «**[Умоўны рэндэрынг](/learn/conditional-rendering)**» каб даведацца розніцу паміж рознымі спосабамі ўмоўнага рэндэрынгу.

</LearnMore>

## Рэндэрынг спісаў {/*rendering-lists*/}

Вам можа спатрэбіцца паказваць некалькі аднолькавых кампанентаў, базуючыся на пэўных даных. Вы можаце выкарыстоўваць `filter()` і `map()` з JavaScript у React каб фільтраваць і ператвараць вашыя даныя з масіваў у кампаненты.

Кожны элемент масіву абавязкова мае мець унікальны `key` (ключ). Найчасцей вы можаце выкарыстоўваць ID з базы даных у якасці `key`. Ключы дазваляюць React адсочваць месцазнаходжанне кожнага элемента, нават калі спіс змяняецца.

<Sandpack>

```js App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        вядомы(-ая) {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Навукоўцы</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

```js data.js
export const people = [{
  id: 0,
  name: 'Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'касмічнымі разлікамі',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Маліна',
  profession: 'хімік',
  accomplishment: 'адкрыццём арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыяй электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Пэрсі Джуліан',
  profession: 'хімік',
  accomplishment: 'унёскамі ў распрацоўку картызону, стэроідаў і супрацьзачаткавых лекаў',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлікамі масы белых карлікаў',
  imageId: 'lrWQx8l'
}];
```

```js utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    's.jpg'
  );
}
```

```css
ul { list-style-type: none; padding: 0px 10px; }
li {
  margin-bottom: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
h1 { font-size: 22px; }
h2 { font-size: 20px; }
```

</Sandpack>

<LearnMore path="/learn/rendering-lists">

Звярніцеся да старонкі «**[Рэндэрынг спісаў](/learn/rendering-lists)**» каб даведацца як рэндэрыць спісы кампанентаў і як выбіраць ключ.

</LearnMore>

## Захоўванне кампанентаў чыстымі {/*keeping-components-pure*/}

Некаторыя JavaScript функцыі называюцца *чыстымі*. Чыстыя функцыі:

* **Займаюцца сваёй справай.** Яны не змяняюць аб’екты або пераменныя, якія існавалі да выкліку функцыі.
* **Вяртаюць прадказальны вынік.** Пры аднолькавых уваходных даных чыстая функцыя заўсёды вяртае аднолькавы вынік.

Калі вы будзеце строга адпавядаць дадзеным прынцыпам, вы зможаце пазбегнуць шэрагу незразумелых хіб і непрадказальных паводзін па меры росту вашай кодавай базы. Вось прыклад кампанента, які не з’яўляецца чыстым:

<Sandpack>

```js
let guest = 0;

function Cup() {
  // ДРЭННА: код змяняе пераменную, якая існуе па-за функцыяй!
  guest = guest + 1;
  return <h2>Кубачак чаю для госця №{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

</Sandpack>

Вы можаце зрабіць гэты кампаненты чыстым, перадаўшы пропс замест змянення знешняй пераменнай:

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Кубачак чаю для госця №{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

</Sandpack>

<LearnMore path="/learn/keeping-components-pure">

Азнаёмцеся са старонкай «**[Захоўванне кампанентаў чыстымі](/learn/keeping-components-pure)**» каб даведацца як пісаць кампаненты ў выглядзе чыстых прадказальных функцый.

</LearnMore>

<<<<<<< HEAD
## Наступныя крокі {/*whats-next*/}
=======
## Your UI as a tree {/*your-ui-as-a-tree*/}

React uses trees to model the relationships between components and modules. 

A React render tree is a representation of the parent and child relationship between components. 

<Diagram name="generic_render_tree" height={250} width={500} alt="A tree graph with five nodes, with each node representing a component. The root node is located at the top the tree graph and is labelled 'Root Component'. It has two arrows extending down to two nodes labelled 'Component A' and 'Component C'. Each of the arrows is labelled with 'renders'. 'Component A' has a single 'renders' arrow to a node labelled 'Component B'. 'Component C' has a single 'renders' arrow to a node labelled 'Component D'.">An example React render tree.</Diagram>

Components near the top of the tree, near the root component, are considered top-level components. Components with no child components are leaf components. This categorization of components is useful for understanding data flow and rendering performance.

Modelling the relationship between JavaScript modules is another useful way to understand your app. We refer to it as a module dependency tree. 

<Diagram name="generic_dependency_tree" height={250} width={500} alt="A tree graph with five nodes. Each node represents a JavaScript module. The top-most node is labelled 'RootModule.js'. It has three arrows extending to the nodes: 'ModuleA.js', 'ModuleB.js', and 'ModuleC.js'. Each arrow is labelled as 'imports'. 'ModuleC.js' node has a single 'imports' arrow that points to a node labelled 'ModuleD.js'.">An example module dependency tree.</Diagram>

A dependency tree is often used by build tools to bundle all the relevant JavaScript code for the client to download and render. A large bundle size regresses user experience for React apps. Understanding the module dependency tree is helpful to debug such issues. 

<LearnMore path="/learn/understanding-your-ui-as-a-tree">

Read **[Your UI as a Tree](/learn/understanding-your-ui-as-a-tree)** to learn how to create a render and module dependency trees for a React app and how they're useful mental models for improving user experience and performance.

</LearnMore>


## What's next? {/*whats-next*/}
>>>>>>> a0cacd7d3a89375e5689ccfba0461e293bfe9eeb

Пачніце са старонкі «[Ваш першы кампанент](/learn/your-first-component)» каб пачаць чытанне главы старонка за старонкай!

Калі дадзеная тэма вам ужо знаёмая, прапануем прачытаць пра [Дадаванне інтэрактыўнасці](/learn/adding-interactivity).

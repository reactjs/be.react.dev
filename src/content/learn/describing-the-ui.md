---
title: Апісанне UI
---

<Intro>

React — гэта JavaScript бібліятэка для рэндэрынгу інтэрфейсу карыстальніка (UI). UI пабудаваны з маленькіх адзінак, такіх як кнопкі, тэксты, відарысы. React дазваляе камбінаваць іх у прыдатныя да паўторнага выкарыстання *кампаненты*, якія можна ўкладаць адзін у аднаго. Ад вэб-сайтаў да мабільных праграм — усё на экране можа быць разбітае на кампаненты. У гэтай главе вы даведаецеся як ствараць і змяняць кампаненты React, а таксама як паказваць іх па-рознаму ў залежнасці ад умоў.

</Intro>

<YouWillLearn isChapter={true}>

* [Як напісаць свой першы кампанент React](/learn/your-first-component)
* [Калі і як змяшчаць некалькі кампанентаў у адзін файл](/learn/importing-and-exporting-components)
* [Як дадаць разметку ў JavaScript з дапамогай JSX](/learn/writing-markup-with-jsx)
* [Як выкарыстоўваць магчымасці JavaScript з дапамогай фігурных дужак у разметцы JSX](/learn/javascript-in-jsx-with-curly-braces)
* [Як змяняць канфігурацыю кампанента з дапамогай пропсаў](/learn/passing-props-to-a-component)
* [Як рэндэрыць кампаненты ў залежнасці ад умоў](/learn/conditional-rendering)
* [Як рэндэрыць некалькі кампанентаў за раз](/learn/rendering-lists)
* [Як пазбягаць памылак, захоўваючы кампанент чыстымі](/learn/keeping-components-pure)
* [Чаму карысна ўяўляць ваш карыстальніцкі інтэрфейс у выглядзе дрэў](/learn/understanding-your-ui-as-a-tree)

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

```js src/App.js hidden
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js src/Gallery.js active
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

```js src/Profile.js
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

```js src/utils.js
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
      {name} {isPacked && '✅'}
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

```js src/App.js
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

```js src/data.js
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

```js src/utils.js
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

## Карыстальніцкі інтэрфейс у выглядзе дрэва {/*your-ui-as-a-tree*/}

React выкарыстоўвае дрэвы для мадэліравання адносін паміж кампанентамі і модулямі.

Дрэва рэндэрынгу React — гэта адлюстраванне бацькоўскіх і даччыных адносін паміж кампанентамі.

<Diagram name="generic_render_tree" height={250} width={500} alt="Дрэвападобны графік з пяццю вузламі, кожны з якіх уяўляе кампанент. Каранёвы вузел знаходзіцца ў верхняй частцы дрэвападобнага графіка і пазначаны як «Каранёвы кампанент (Root Component)». Ён мае дзве стрэлкі, якія ідуць уніз да двух вузлоў, пазначаных «Кампанент A (Component A)» і «Кампанент C (Component C)». Кожная са стрэлак пазначана надпісам «рэндэрыць». «Кампанент A» мае адзіную стрэлку «рэндэрыць» да вузла з надпісам «Кампанент B (Component B)». «Кампанент C» мае адну стрэлку «рэндэрыць» да вузла з надпісам «Кампанент D (Component D)».">

Прыклад дрэва рэндэрынгу React.

</Diagram>

Кампаненты, якія знаходзяцца ў верхняй частцы дрэва, каля каранёвага кампанента, лічацца кампанентамі верхняга ўзроўню. Кампаненты без даччыных кампанентаў з'яўляюцца ліставымі кампанентамі. Такая класіфікацыя кампанентаў карысная для разумення патоку даных і прадукцыйнасці рэндэрынгу.

Мадэліраванне адносін паміж модулямі JavaScript — гэта яшчэ адзін карысны спосаб зразумець вашу праграму. Мы называем гэта «дрэвам залежнасці модуляў».

<Diagram name="generic_dependency_tree" height={250} width={500} alt="Дрэвападобны графік з пяццю вузламі. Кожны вузел уяўляе сабой JavaScript модуль. Самы верхні вузел пазначаны як «RootModule.js». Ён мае тры стрэлкі, якія ідуць да вузлоў: «ModuleA.js», «ModuleB.js» і «ModuleC.js». Кожная стрэлка пазначана надпісам «імпартуе». Вузел «ModuleC.js» мае адну стрэлку з надпісам «імпартуе», якая паказвае на вузел з надпісам «ModuleD.js».">

Прыклад дрэва залежнасцяў модуля.

</Diagram>

Дрэва залежнасцяў часта выкарыстоўваецца інструментамі зборкі для аб'яднання ўсяго адпаведнага JavaScript кода для спампоўвання і адлюстравання кліентам. Вялікі памер пакета пагаршае карыстальніцкі досвед узаемадзеяння з праграмамі напісанымі з дапамогай React. Разуменне дрэва залежнасцяў модуля дапамагае вырашаць такія праблемы.

<LearnMore path="/learn/understanding-your-ui-as-a-tree">

Азнаёмцеся са старонкай «**[Карыстальніцкі інтэрфейс у выглядзе дрэва](/learn/understanding-your-ui-as-a-tree)**» каб даведацца як ствараць дрэвы рэндэрынгу і дрэва залежнасцяў модуля для праграмы React і як гэтыя ментальныя мадэлі дапамагаюць палепшыць карыстальніцкі досвед і прадукцыйнасць.

</LearnMore>

## Наступныя крокі {/*whats-next*/}

Пачніце са старонкі «[Ваш першы кампанент](/learn/your-first-component)» каб пачаць чытанне главы старонка за старонкай!

Калі дадзеная тэма вам ужо знаёмая, прапануем прачытаць пра [Дадаванне інтэрактыўнасці](/learn/adding-interactivity).

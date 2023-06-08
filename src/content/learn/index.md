---
title: Хуткі старт
---

<Intro>

Вітаем вас у дакументацыі React! Гэтая старонка пазнаёміць вас з 80% канцэпцый React, якімі вы будзеце карыстацца кожны дзень.

</Intro>

<YouWillLearn>

- Як ствараць і ўкладаць кампаненты
- Як дадаваць разметку і стылі
- Як адлюстроўваць даныя
- Як адлюстроўваць умовы і спісы
- Як рэагаваць на падзеі і абнаўляць экран
- Як абменьвацца данымі паміж кампанентамі

</YouWillLearn>

## Стварэнне і ўкладанне кампанентаў {/*components*/}

Праграмы на React складаюцца з *кампанентаў*. Кампанент — гэта частка UI (карыстальніцкага інтэрфейсу), якая мае ўласную логіку і знешні выгляд. Кампанент можа быць маленькім, як кнопка, або вялікім, як цэлая старонка.

Кампаненты React — гэта функцыі JavaScript, якія вяртаюць разметку:

```js
function MyButton() {
  return (
    <button>Я — кнопка</button>
  );
}
```

Цяпер, калі вы аб'явілі `MyButton`, вы можаце ўкласці яго ў іншы кампанент:

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Вітаю ў маёй праграме</h1>
      <MyButton />
    </div>
  );
}
```

Звярніце ўвагу, што `<MyButton />` пачынаецца з вялікай літары. Такім чынам вы разумееце, што гэта кампанент React. Назвы кампанентаў React заўсёды павінны пачынацца з вялікай літары, а тэгі HTML — з малой.

Паглядзіце на вынік:

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      Я — кнопка
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Вітаю ў маёй праграме</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

Ключавыя словы `export default` вызначаюць асноўны кампанент у файле. Калі вы не знаёмыя з некаторымі часткамі сінтаксісу JavaScript, [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) і [javascript.info](https://javascript.info/import-export) маюць выдатныя матэрыялы на гэтую тэму.

## Напісанне разметкі з дапамогай JSX {/*writing-markup-with-jsx*/}

Сінтаксіс разметкі, які вы бачылі вышэй, называецца *JSX*. Ён неабавязковы, але большасць праектаў React выкарыстоўваюць JSX для зручнасці. Усе [інструменты, якія мы рэкамендуем для лакальнай распрацоўкі](/learn/installation) падтрымліваюць JSX.

JSX больш строгі, чым HTML. Вы павінны закрываць такія тэгі, як `<br />`. Таксама ваш кампанент не можа вяртаць некалькі тэгаў JSX. Іх трэба змясціць у агульны бацькоўскі элемент, напрыклад `<div>...</div>` або пустую абгортку `<>...</>`:

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>Пра мяне</h1>
      <p>Вітаю.<br />Як маецеся?</p>
    </>
  );
}
```

Для таго, каб перанесці вялікую колькасць HTML у JSX, можна выкарыстоўваць [анлайн-канвертар.](https://transform.tools/html-to-jsx)

## Дадаванне стыляў {/*adding-styles*/}

У React клас CSS вызначаецца з дапамогай `className`. Ён працуе гэтак жа, як атрыбут [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) у HTML:

```js
<img className="avatar" />
```

Затым вы прапісваеце правілы CSS для яго ў асобным файле CSS:

```css
/* У вашым CSS */
.avatar {
  border-radius: 50%;
}
```

React не вызначае, як дадаваць файлы CSS. У самым простым выпадку вы дадасце тэг [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) у ваш HTML. Калі вы выкарыстоўваеце інструмент зборкі або фрэймворк, звярніцеся да яго дакументацыі, каб даведацца, як дадаць файл CSS у ваш праект.

## Адлюстраванне даных {/*displaying-data*/}

JSX дазваляе дадаваць разметку ў JavaScript. Фігурныя дужкі дазваляюць вам «вярнуцца» ў JavaScript, каб вы маглі ўставіць некаторую пераменную з вашага кода і паказаць яе карыстальніку. Напрыклад, код ніжэй пакажа `user.name`:

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

Вы таксама можаце выкарыстоўваць JavaScript у атрыбутах JSX. Для гэтага вам трэба паставіць фігурныя дужкі *замест* двукоссяў. Напрыклад, `className="avatar"` перадае радок `"avatar"` як клас CSS, а `src={user.imageUrl}` счытвае значэнне пераменнай JavaScript `user.imageUrl` і перадае гэта значэнне як атрыбут `src`:

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

Вы таксама можаце выкарыстоўваць больш складаныя выразы ў фігурных дужках JSX, напрыклад, [канкатэнацыю радкоў](https://javascript.info/operators#string-concatenation-with-binary):

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Фота ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

У прыведзеным вышэй прыкладзе `style={{}}` — гэта не спецыяльны сінтаксіс, а звычайны аб'ект `{}` у фігурных дужках JSX `style={ }`. Вы можаце выкарыстоўваць атрыбут `style`, калі вашы стылі залежаць ад пераменных JavaScript.

## Умоўны рэндэрынг {/*conditional-rendering*/}

У React не існуе спецыяльнага сінтаксісу для апісання ўмоў, замест гэтага можна выкарыстоўваць звычайны код на JavaScript. Напрыклад, для ўмоўнага рэндэрынгу JSX можна ўжываць аператар [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else):

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

Калі вы аддаяце перавагу больш кампактнаму коду, вы можаце выкарыстоўваць [умоўны аператар `?`.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) У адрозненне ад `if` яго можна выкарыстоўваць у JSX:

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Калі вам не патрэбна галіна `else`, можна выкарыстоўваць карацейшы [лагічны аператар `&&`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation):

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

Усе гэтыя спосабы падыходзяць і для задання ўмоў у атрыбутах. Калі вам не знаёмы некаторыя з гэтых сінтаксічных структур JavaScript, вы можаце, для пачатку, заўсёды выкарыстоўваць `if...else`.

## Рэндэрынг спісаў {/*rendering-lists*/}

Для адлюстравання спісаў кампанентаў вам трэба будзе выкарыстоўваць такія магчымасці JavaScript, як [цыкл `for`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) і [функцыя масіву `map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).


Напрыклад, уявім, што ў вас ёсць масіў прадуктаў:

```js
const products = [
  { title: 'Капуста', id: 1 },
  { title: 'Часнок', id: 2 },
  { title: 'Яблык', id: 3 },
];
```

Пераўтварыце гэты масіў прадуктаў у масіў элементаў `<li>` з дапамогай функцыі `map()` унутры вашага кампанента:

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Звярніце ўвагу, што `<li>` мае атрыбут `key`. Для кожнага элемента спісу вам трэба задаваць ключ у выглядзе радка або ліку, які дазволіць адназначна аддзяліць гэты элемент ад астатніх у спісе. Звычайна гэты ключ бярэцца з вашых даных, напрыклад, гэта можа быць ідэнтыфікатар з базы даных. React выкарыстоўвае гэтыя ключы пры дадаванні, выдаленні або змяненні парадку элементаў.

<Sandpack>

```js
const products = [
  { title: 'Капуста', isFruit: false, id: 1 },
  { title: 'Часнок', isFruit: false, id: 2 },
  { title: 'Яблык', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## Апрацоўка падзей {/*responding-to-events*/}

Вы можаце рэагаваць на падзеі, аб'яўляючы ўнутры вашых кампанентаў функцыі *апрацоўшчыкаў падзей*:

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('Мяне націснулі!');
  }

  return (
    <button onClick={handleClick}>
      Націсніце мяне
    </button>
  );
}
```

Звярніце ўвагу, што `onClick={handleClick}` не мае круглых дужак у канцы! Не _выклікайце_ функцыю апрацоўшчыка падзей: вам трэба толькі *перадаць яе*. React выкліча ваш апрацоўшчык падзей, калі карыстальнік націсне кнопку.

## Абнаўленне экрана {/*updating-the-screen*/}

Часта вам можа спатрэбіцца, каб ваш кампанент «запомніў» некаторую інфармацыю і адлюстраваў яе. Напрыклад, вы хочаце падлічыць, колькі разоў націскалася кнопка. Для гэтага дадайце *стан* да вашага кампанента.

Спачатку імпартуйце [`useState`](/reference/react/useState) з React:

```js
import { useState } from 'react';
```

Цяпер вы можаце аб'явіць *пераменную стану* ўнутры вашага кампанента:

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

`useState` верне вам дзве рэчы: бягучы стан (`count`) і функцыю, якая дазваляе абнаўляць яго (`setCount`). Вы можаце даць ім любыя назвы, але прынята пісаць `[something, setSomething]`.

Пры першым паказе кнопкі `count` будзе `0`, таму што вы перадалі `0` у `useState()`. Для змены стану выклічце `setCount()` і перадайце туды новае значэнне. Націсканне на гэту кнопку павялічыць лічыльнік:

```js {5}
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Націснута разоў: {count}
    </button>
  );
}
```

React зноў выкліча функцыю вашага кампанента. На гэты раз `count` будзе роўны `1`, затым `2`, і гэтак далей.

Калі вы рэндэрыце адзін і той жа кампанент некалькі разоў, то ў кожнага з іх будзе свой стан. Паспрабуйце націснуць кожную кнопку паасобку:

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Лічыльнікі, якія абнаўляюцца асобна</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Націснута разоў: {count}
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

Звярніце ўвагу на тое, як кожная кнопка «запамінае» свой уласны стан `count` і не ўплывае на іншыя кнопкі.

## Выкарыстанне хукаў {/*using-hooks*/}

Функцыі, чые назвы пачынаюцца з `use`, называюцца *хукамі*. `useState` — гэта ўбудаваны хук у React. У [даведцы па API](/reference/react) можна знайсці іншыя ўбудаваныя хукі. Таксама, вы можаце пісаць свае ўласныя хукі, сумяшчаючы іх з ужо існуючымі.

Хукі маюць больш абмежаванняў, чым іншыя функцый. Хукі могуць выклікацца толькі *у пачатку* вашых кампанентаў (або іншых хукаў). Калі вам патрэбны `useState` ва ўмове ці цыкле, стварыце новы кампанент і выкарыстоўвайце яго там.

## Абмен данымі паміж кампанентамі {/*sharing-data-between-components*/}

У папярэднім прыкладзе кожны кампанент `MyButton` меў свой уласны незалежны `count`, і калі націскалася кожная кнопка, мяняўся толькі `count` для націснутай кнопкі:

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Дыяграма, якая паказвае дрэва з трох кампанентаў, адзін бацькоўскі з надпісам MyApp і два даччыныя з надпісам MyButton. Абодва кампаненты MyButton утрымліваюць лік з нулявым значэннем.">

Першапачаткова ў кожнага `MyButton` стан `count` роўны `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="Тая ж дыяграма, што і папярэдняя, з вылучаным лікам першага даччынага кампанента MyButton, што паказвае націсканне і павялічэнне count да адзінкі. Другі кампанент MyButton па-ранейшаму змяшчае нулявое значэнне." >

Першы `MyButton` абнаўляе свой `count` да `1`

</Diagram>

</DiagramGroup>

Аднак, часта ўзнікаюць сітуацыі, калі вам трэба, каб кампаненты «мелі агульныя даныя і заўсёды абнаўляліся разам».

Для таго, каб абодва кампаненты `MyButton` адлюстроўвалі аднолькавы `count` і абнаўляліся разам, вам трэба перамясціць стан ад асобных кнопак «уверх» да бліжэйшага кампанента, які змяшчае гэтыя кампаненты.

У гэтым прыкладзе гэта кампанент `MyApp`:

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Дыяграма, якая паказвае дрэва з трох кампанентаў, адзін бацькоўскі з надпісам MyApp і два даччыныя з надпісам MyButton. MyApp змяшчае нулявое значэнне лічыльніка, якое перадаецца абодвум кампанентам MyButton, якія таксама паказваюць нулявое значэнне." >

Першапачаткова стан `count` кампанента `MyApp` роўны `0` і перадаецца абодвум даччыным кампанентам

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="Тая ж дыяграма, што і папярэдняя, з вылучаным лікам бацькоўскага кампанента MyApp, што паказвае націсканне і павялічэнне count да адзінкі. Кірунак да абодвух даччыных кампанентаў MyButton таксама вылучаны, і значэнне лічыльнікаў ў кожным даччыным элементе раўно аднаму, што паказвае, што значэнне было перададзена." >

Пасля націскання `MyApp` абнаўляе свой стан `count` на `1` і перадае яго абодвум даччыным кампанентам

</Diagram>

</DiagramGroup>

Цяпер, калі вы націснеце адну з кнопак, `count` у `MyApp` зменіцца, што зменіць значэнні лічыльнікаў у абодвух кампанентах `MyButton`. Вось як гэта можна запісаць у кодзе.

Спачатку *перамясціце ўверх стан* з `MyButton` у `MyApp`:

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Лічыльнікі, якія абнаўляюцца асобна</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... мы перамяшчаем код адсюль ...
}

```

Затым *перадайце стан на ўзровень ніжэй* з `MyApp` у кожны `MyButton` разам з агульным апрацоўшчыкам націсканняў. Вы можаце перадаць інфармацыю ў `MyButton` з дапамогай фігурных дужак JSX такім жа чынам, як вы гэта рабілі з убудаванымі тэгамі накшталт `<img>`:

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Лічыльнікі, якія абнаўляюцца разам</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

Інфармацыя, якую вы перадаяце такім чынам, называецца _пропсамі_ (props). Цяпер кампанент `MyApp` мае стан `count` і апрацоўшчык падзеі `handleClick` і *перадае абодва ў якасці пропсаў* кожнай кнопцы.

Нарэшце, змяніце кампанент `MyButton` так, каб ён *счытваў* пропсы, якія яму перадалі з яго бацькоўскага кампанента:

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Націснута разоў: {count}
    </button>
  );
}
```

Калі вы націскаеце кнопку, спрацоўвае апрацоўшчык `onClick`. У кожнай кнопцы ў якасці значэння пропсаў `onClick` зададзена функцыя `handleClick` з кампанента `MyApp`, там выконваецца код гэтай функцыі. Гэты код выклікае `setCount(count + 1)`, што павялічвае пераменную стану `count`. Новае значэнне `count` перадаецца ў якасці пропсаў кожнай кнопцы, такім чынам усе кнопкі будуць паказваць гэтае новае значэнне. Гэта называецца «падніманнем стану ўверх». Паднімаючы стан уверх, вы робіце яго агульным для ўсіх кампанентаў.

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Лічыльнікі, якія абнаўляюцца разам</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Націснута разоў: {count}
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

## Наступныя крокі {/*next-steps*/}

Цяпер вы ведаеце асновы таго, як пісаць код на React!

Азнаёмцеся з [уводзінамі](/learn/tutorial-tic-tac-toe), каб прымяніць на практыцы атрыманыя веды і стварыць сваю першую міні-праграму на React.

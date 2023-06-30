---
title: Філасофія React
---

<Intro>

React можа змяніць вашы падыходы да дызайну макетаў і распрацоўкі праграм. Калі вы ствараеце карыстальніцкі інтэрфейс (UI) з React, спачатку вы разбіваеце яго на часткі — *кампаненты*. Затым для кожнага з іх вы апісваеце разнастайныя візуальныя станы. Нарэшце, вы злучаеце вашыя кампаненты разам так, каб даныя перамяшчаліся па іх. У гэтым дапаможніку мы пакажам вам мысліцельны працэс падчас стварэння табліцы прадуктаў з пошукам на React.

</Intro>

## Пачнём з макета {/*start-with-the-mockup*/}

Уявіце, што ў вас ужо ёсць JSON API і макет ад дызайнера.

JSON API вяртае даныя, якія выглядаюць наступным чынам:

```json
[
  {category: "Fruits", price: "$1", stocked: true, name: "Яблык"},
  {category: "Fruits", price: "$1", stocked: true, name: "Пітайя"},
  {category: "Fruits", price: "$2", stocked: false, name: "Маракуйя"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Шпінат"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Гарбуз"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Гарох"}
]
```

Макет выглядае так:

<img src="/images/docs/s_thinking-in-react_ui.png" width="300" style={{margin: '0 auto'}} />

Каб стварыць карыстальніцкі інтэрфейс на React, звычайна трэба зрабіць адны і тыя ж пяць крокаў.

## Крок 1: Разбіце інтэрфейс на кампаненты {/*step-1-break-the-ui-into-a-component-hierarchy*/}

Для пачатку вылучыце ўсе кампаненты і падкампаненты на макеце і дайце ім імёны. Калі вы працуеце з дызайнерам, то магчыма, што ён ужо неяк назваў гэтыя кампаненты. Спытайцеся ў яго!

Вы можаце падыходзіць да разбівання дызайну на кампаненты па-рознаму, асноўваючыся на вашым досведзе:


* **Праграмаванне**--выкарыстоўвайце той жа падыход, што і пры стварэнні простай функцыі або аб'екта. Можна прымяніць [прынцып адзінай адказнасці:](https://be.wikipedia.org/wiki/%D0%9F%D1%80%D1%8B%D0%BD%D1%86%D1%8B%D0%BF_%D0%B0%D0%B4%D0%B7%D1%96%D0%BD%D0%B0%D0%B9_%D0%B0%D0%B4%D0%BA%D0%B0%D0%B7%D0%BD%D0%B0%D1%81%D1%86%D1%96) гэта значыць, што ў ідэале кампанент павінен займацца нейкай адной задачай. Калі функцыянальнасць кампанента павялічваецца з цягам часу, яго варта разбіць на драбнейшыя падкампаненты.
* **CSS**--падумайце, для чаго б вы зрабілі селектары класаў (аднак памятайце, што кампаненты крыху менш дэтальныя).
* **Дызайн**--падумайце, як бы вы арганізавалі слаі дызайну.

Добрая структура ў JSON часта ўжо адлюстроўвае структуру кампанентаў у вашым UI. Гэта адбываецца праз тое, што ў UI і мадэлі даных часта падобная інфармацыйная архітэктура. Разбіце UI на кампаненты, кожны з якіх адлюстроўвае частку мадэлі даных.

Тут можна вылучыць пяць кампанентаў:

<FullWidth>

<CodeDiagram flip>

<img src="/images/docs/s_thinking-in-react_ui_outline.png" width="500" style={{margin: '0 auto'}} />

1. `FilterableProductTable` (шэры) змяшчае ўсю праграму.
2. `SearchBar` (блакітны) атрымлівае ўвод карыстальніка.
3. `ProductTable` (фіялетавы) адлюстроўвае і фільтруе спіс у адпаведнасці з уводам карыстальніка.
4. `ProductCategoryRow` (зялёны) адлюстроўвае загалоўкі катэгорый.
5. `ProductRow`	(жоўты) адлюстроўвае радок для кожнага прадукту.

</CodeDiagram>

</FullWidth>

Звярніце ўвагу, што ўнутры `ProductTable` (фіялетавы) загаловак табліцы («Name» і «Price») сам па сабе не з'яўляецца асобным кампанентам. Аддзяляць яго ці не -- гэта ў асноўным пытанне пераваг. У гэтым прыкладзе ён з'яўляецца часткай `ProductTable`, таму што знаходзіцца ўнутры спіса `ProductTable`. Тым не менш, калі ў будучыні загаловак папоўніцца новымі функцыямі (напрыклад магчымасцю сартаваць тавары) вы можаце перанесці яго ў самастойны кампанент `ProductTableHeader`.

Цяпер, калі вы вызначылі кампаненты ў макеце, упарадкуйце іх у іерархію. Кампаненты, якія з'яўляюцца часткай іншых кампанентаў, у іерархіі будуць даччынымі:

* `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
        * `ProductCategoryRow`
        * `ProductRow`

## Крок 2: Стварыце статычную версію на React {/*step-2-build-a-static-version-in-react*/}

Цяпер, калі ў вас ёсць іерархія кампанентаў, прыйшоў час стварыць вашу праграму. Самы просты падыход -- стварыць версію, якая рэндэрыць UI, засноўваючыся на вашай мадэлі даных, але без дадавання якой-небудзь інтэрактыўнасці... пакуль! Звычайна прасцей спачатку стварыць статычную версію, а потым дадаць інтэрактыўнасць. Стварэнне статычнай версіі патрабуе шмат набору тэксту і амаль не патрабуе глыбокіх разважанняў. З іншага боку, дадаванне інтэрактыўнасці патрабуе шмат разважанняў і няшмат набору тэксту.

Каб стварыць статычную версію, якая адлюстроўвае мадэль даных, нам трэба стварыць [кампаненты,](/learn/your-first-component) якія выкарыстоўваюць іншыя кампаненты і перадаюць даныя з дапамогай [пропсаў.](/learn/passing-props-to-a-component) З дапамогай пропсаў даныя перадаюцца ад бацькоўскага кампанента да кампанентаў нашчадкаў. (Калі вы знаёмыя з паняццем [стан](/learn/state-a-components-memory), то для статычнай праграмы гэта як раз тое, што вам выкарыстоўваць не трэба. Стан выкарыстоўваецца тады, калі ў нас ёсць даныя, якія мяняюцца з часам, інтэрактыўнасць. Паколькі мы ствараем статычную версію праграмы, нам гэта не патрэбна.)

Напісанне кода можна пачаць як «зверху ўніз», пачынаючы са стварэння кампанентаў, якія знаходзяцца вышэй у іерархіі (такіх як `FilterableProductTable`), так і «знізу ўверх», пачаўшы з нізкаўзроўневых кампанентаў (такіх як `ProductRow`). У больш простых праектах звычайна лягчэй ісці зверху ўніз, а ў больш складаных — знізу ўверх.

<Sandpack>

```jsx App.js
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Назва</th>
          <th>Цана</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Шукаць..." />
      <label>
        <input type="checkbox" />
        {' '}
        Паказваць толькі тавары ў наяўнасці
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Яблык"},
  {category: "Fruits", price: "$1", stocked: true, name: "Пітайя"},
  {category: "Fruits", price: "$2", stocked: false, name: "Маракуйя"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Шпінат"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Гарбуз"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Гарох"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 10px;
}
td {
  padding: 2px;
  padding-right: 40px;
}
```

</Sandpack>

(Калі гэты код здаўся вам складаным, то прачытайце раздзел [Хуткі старт](/learn/)!)

Калі скончыце ствараць кампаненты, у вас атрымаецца бібліятэка прыдатных да паўторнага выкарыстання кампанентаў, якія візуалізуюць вашу мадэль даных. Паколькі гэта статычная праграма, кампаненты будуць вяртаць толькі JSX. Кампанент на вяршыні іерархіі (`FilterableProductTable`) будзе прымаць мадэль даных праз пропсы. Гэта называецца _аднабаковы паток даных,_ таму што даныя перадаюцца ад кампанентаў з верхняга ўзроўню іерархіі да кампанентаў у ніжняй частцы іерархіі.

<Pitfall>

Зараз вы не павінны выкарыстоўваць ніякіх значэнняў стану. Мы дададзім іх на наступным кроку!

</Pitfall>

## Крок 3: Знайдзіце мінімальнае, але поўнае прадстаўленне стану карыстальніцкага інтэрфейсу {/*step-3-find-the-minimal-but-complete-representation-of-ui-state*/}

Каб зрабіць UI інтэрактыўным, трэба дазволіць карыстальнікам змяняць вашу асноўную мадэль даных. Для гэтага вы будзеце выкарыстоўваць *стан*.

Стан можна ўявіць як мінімальны набор даных, якія змяняюцца і якія вашай праграме неабходна запомніць. Галоўнае пры рабоце са станам — гэта прытрымлівацца прынцыпу [DRY: Don't Repeat Yourself](https://be.wikipedia.org/wiki/DRY_(%D0%BF%D1%80%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D0%B2%D0%B0%D0%BD%D0%BD%D0%B5)) (бел.: не паўтарайся). Вызначыце мінімальны неабходны стан, які патрэбны вашай праграме, а ўсё астатняе вылічайце па неабходнасці. Напрыклад, калі вы ствараеце спіс пакупак, можаце стварыць стан і змясціць у яго масіў прадметаў са спісу. Калі трэба адлюстраваць колькасць прадметаў, не варта ствараць яшчэ адзін стан для іх ліку. Замест гэтага выкарыстоўвайце даўжыню існуючага масіву.

А цяпер падумайце пра ўсе фрагменты даных у гэтай дэма-праграме:

1. Першапачатковы спіс прадуктаў
2. Пошукавы запыт, які ўвёў карыстальнік
3. Значэнне чэкбокса
4. Адфільтраваны спіс прадуктаў

Якія з гэтых даных павінны захоўвацца ў стане? Вызначыце тыя, якія даюць адмоўны адказ на наступныя пытанні:

* Ці **застаюцца яны нязменнымі** з цягам часу? Калі так, то гэтыя даныя не павінны захоўвацца ў стане.
* **Ці перадаюцца яны ад бацькоўскага кампаненту** праз пропсы? Калі так, то гэтыя даныя не павінны захоўвацца ў стане.
* **Ці можаце вы вылічыць іх** на аснове існуючага стану або пропсаў у вашым кампаненце? Калі так, то гэтыя даныя *дакладна* не павінны захоўвацца ў стане.

Астатнія даныя, хутчэй за ўсё, павінны захоўвацца ў стане.

Давайце пройдземся па іх адзін за адным яшчэ раз:

1. Першапачатковы спіс прадуктаў **перадаецца праз пропсы, таму яго не трэба захоўваць у стане.**
2. Пошукавы запыт, здаецца, павінен захоўвацца ў стане, бо ён змяняецца з цягам часу і не можа быць вылічаны з іншых даных.
3. Значэнне чэкбокса, здаецца, павінна захоўвацца ў стане, бо яно змяняецца з цягам часу і не можа быць вылічаны з іншых даных.
4. Адфільтраваны спіс тавараў **не трэба захоўваць у стане, бо яго можна вылічыць,** адфільтраваўшы арыгінальны спіс з дапамогай пошукавага запыту і значэння чэкбокса.

Атрымліваецца, што толькі тэкст пошуку і значэнне чэкбокса будуць захоўвацца ў стане! Выдатная праца!

<DeepDive>

#### Розніца паміж пропсамі і станам {/*props-vs-state*/}

У React ёсць два тыпы даных «мадэлі»: пропсы і стан. Яны вельмі адрозніваюцца:

* [**Пропсы** падобныя на аргументы, якія вы перадаяце](/learn/passing-props-to-a-component) функцыі. Яны дазваляюць бацькоўскаму кампаненту перадаваць даныя даччынаму кампаненту і змяняць яго знешні выгляд. Напрыклад, кампанент `Form` можа перадаваць атрыбут `color` кампаненту `Button`.
* [**Стан** можна назваць памяццю кампанента.](/learn/state-a-components-memory) Ён дазваляе кампаненту захоўваць інфармацыю і змяняць яе ў адказ на ўзаемадзеянне. Напрыклад, кнопка «Button» можа мець стан «isHovered».

Пропсы і стан адрозніваюцца, але працуюць разам. Бацькоўскі кампанент будзе часта захоўваць інфармацыю ў стане (каб ён мог яе змяняць) і *перадаваць* яе даччыным кампанентам як пропсы. Нічога страшнага, калі розніца вам усё яшчэ здаецца невыразнай. Пасля невялікай практыкі розніца стане больш відавочнай!

</DeepDive>

## Крок 4: Вызначыце, дзе павінен знаходзіцца ваш стан {/*step-4-identify-where-your-state-should-live*/}

Пасля вызначэння мінімальнага набору станаў праграмы вам трэба высветліць, які з кампанентаў адказвае за змену стану або «валодае» ім. Памятайце: React выкарыстоўвае аднабаковы паток даных, перадаючы даныя ўніз па іерархіі кампанентаў ад бацькоўскага да даччынага кампанента. Спачатку можа быць не зусім зразумела, які з кампанентаў які стан павінен захоўваць. Калі для вас гэта новая канцэпцыя, то, магчыма, гэта будзе складана для вас. Аднак вы можаце разабрацца, прытрымліваючыся гэтых інструкцый!

Для кожнага стану ў вашай праграме:

1. Вызначыце *ўсе* кампаненты, якія рэндэраць нешта на аснове гэтага стану.
2. Знайдзіце іх бліжэйшы агульны бацькоўскі кампанент — гэта кампанент, які знаходзіцца вышэй за іх усіх у іерархіі.
3. Вызначыце, дзе павінен знаходзіцца стан:
    1. Часта вы можаце змясціць стан непасрэдна ў іх агульнага продка.
    2. Таксама вы можаце змясціць стан у нейкі кампанент над іх агульным бацькам.
    3. Калі вы не можаце знайсці прыдатны кампанент, то стварыце новы кампанент выключна для ўтрымання стану і размясціце яго вышэй у іерархіі над агульным бацькоўскім кампанентам.

На папярэднім кроку вы знайшлі два станы ў гэтай дэма-праграме: пошукавы запыт і значэнне чэкбокса. У гэтым прыкладзе яны заўсёды з'яўляюцца разам, таму мае сэнс размясціць іх у адным месцы.

Давайце разбяром іх з дапамогай нашай стратэгіі:

1. **Вызначыце кампаненты, якія выкарыстоўваюць стан:**
    * `ProductTable` фільтруе спіс прадуктаў на аснове стану (пошукавы запыт і значэнне чэкбокса).
    * `SearchBar` адлюстроўвае стан (пошукавы запыт і значэнне чэкбокса).
1. **Знайдзіце іх агульнага продка:** Першы агульны бацькоўскі кампанент для іх — `FilterableProductTable`.
2. **Вызначыце, дзе будзе знаходзіцца стан**: Мы будзем захоўваць тэкст фільтра і значэнне чэкбокса ў `FilterableProductTable`.

Такім чынам, значэнні стану будуць знаходзіцца ў `FilterableProductTable`.

Дадайце стан да кампанента з дапамогай [хуку `useState()`.](/reference/react/useState) Хукі — гэта спецыяльныя функцыі, якія дазваляюць «падчапіцца» да React. Дадайце дзве пераменныя стану ў пачатак `FilterableProductTable` і ўкажыце іх пачатковыя значэнні:

```js
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);  
```

Затым перадайце `filterText` і `inStockOnly` у кампаненты `ProductTable` і `SearchBar` у якасці пропсаў:

```js
<div>
  <SearchBar 
    filterText={filterText} 
    inStockOnly={inStockOnly} />
  <ProductTable 
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

Цяпер вы пачынаеце бачыць, як будзе працаваць ваша праграма. Змяніце пачатковае значэнне `filterText` з `useState('')` на `useState('fruit')` у рэдактары кода ніжэй. Вы ўбачыце, што змяніліся і пошукавы запыт, і табліца:

<Sandpack>

```jsx App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} />
      <ProductTable 
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Назва</th>
          <th>Цана</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Шукаць..."/>
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} />
        {' '}
        Паказваць толькі тавары ў наяўнасці
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Яблык"},
  {category: "Fruits", price: "$1", stocked: true, name: "Пітайя"},
  {category: "Fruits", price: "$2", stocked: false, name: "Маракуйя"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Шпінат"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Гарбуз"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Гарох"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 5px;
}
td {
  padding: 2px;
}
```

</Sandpack>

Звярніце ўвагу, што змяненне пошукавага запыту пакуль нічога не робіць. Памылка ў кансолі рэдактара вышэй тлумачыць, чаму:

<ConsoleBlock level="error">

You provided a \`value\` prop to a form field without an \`onChange\` handler. This will render a read-only field.

</ConsoleBlock>

У рэдактары кода зверху `ProductTable` і `SearchBar` счытваюць пропсы `filterText` і `inStockOnly`, каб адрэндэрыць табліцу, поле ўводу і чэкбокс. Напрыклад, вось так `SearchBar` запаўняе значэнне поля ўводу:

```js {1,6}
function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Шукаць..."/>
```

Аднак вы яшчэ не дадалі ніякага кода для рэагавання на дзеянні карыстальніка. Гэта вы зробіце ў апошнім кроку.


## Крок 5: Дадайце зваротны паток даных {/*step-5-add-inverse-data-flow*/}

Зараз ваша праграма рэндэрыцца, грунтуючыся на пропсах і стане, якія перадаюцца ўніз па іерархіі. Але для таго, каб змяніць стан у адпаведнасці з уводам карыстальніка, вам трэба будзе забяспечыць паток даных у іншы бок: кампаненты формы ў нізе іерархіі павінны абнаўляць стан у `FilterableProductTable`.

React патрабуе каб гэты паток даных быў відавочна прапісаны ўручную, што патрабуе крыху больш кода, чым двухбаковае прывязванне даных. Калі вы паспрабуеце ўвесці тэкст у поле пошуку або паставіць «птушку» ў чэкбоксе ў прыкладзе вышэй, то ўбачыце, што React ігнаруе ваш увод. Ён гэта робіць наўмысна. Калі вы напісалі `<input value={filterText} />`, вы ўказалі, што пропс `value` у `input` будзе роўным значэнню стану `filterText`, якое перадаецца з кампанента `FilterableProductTable`. Паколькі стан `filterText` не зададзены, поле ўводу ніколі не зменіцца.

Вы трэба зрабіць так, каб пры змяненні пошукавай формы, значэнне стану таксама абнаўлялася. Стан знаходзіцца ў `FilterableProductTable`, таму толькі ён можа выклікаць `setFilterText` і `setInStockOnly`. Каб дазволіць `SearchBar` абнаўляць стан `FilterableProductTable`, вам трэба перадаць гэтыя функцыі ў кампанент `SearchBar`:

```js {2,3,10,11}
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
```

Унутры `SearchBar` дадайце апрацоўшчыкі падзей `onChange` і з іх дапамогай задайце значэнні станаў у бацькавым кампаненце:

```js {5}
<input 
  type="text" 
  value={filterText} 
  placeholder="Шукаць..." 
  onChange={(e) => onFilterTextChange(e.target.value)} />
```

Цяпер праграма цалкам працуе!

<Sandpack>

```jsx App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Назва</th>
          <th>Цана</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Шукаць..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Паказваць толькі тавары ў наяўнасці
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Яблык"},
  {category: "Fruits", price: "$1", stocked: true, name: "Пітайя"},
  {category: "Fruits", price: "$2", stocked: false, name: "Маракуйя"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Шпінат"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Гарбуз"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Гарох"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding: 4px;
}
td {
  padding: 2px;
}
```

</Sandpack>

Аб апрацоўцы падзей і абнаўленні стану вы можаце прачытаць у раздзеле «[Дадаванне інтэрактыўнасці](/learn/adding-interactivity)».

## Што далей {/*where-to-go-from-here*/}

Гэта было вельмі кароткае ўвядзенне ў падыход да напісання кампанентаў і праграм на React. Цяпер вы можаце [пачаць праект на React](/learn/installation) прама зараз ці [паглыбіцца ў сінтаксіс,](/learn/describing-the-ui) што выкарыстоўваўся ў гэтым дапаможніку.

---
title: Рэндэрынг спісаў
---

<Intro>

Часта ўзнікае неабходнасць адлюстравання шэрагу падобных кампанентаў на аснове набору даных. Каб маніпуліраваць масівам даных можна выкарыстоўваць [метады масіваў JavaScript.](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array#) На гэтай старонцы вы будзеце выкарыстоўваць метады [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) і [`map()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/map) разам з React для фільтрацыі і пераўтварэння масіву даных у масіў кампанентаў.

</Intro>

<YouWillLearn>

* Як рэндэрыць кампаненты з масіву з дапамогай метаду `map()`
* Як рэндэрыць толькі пэўныя кампаненты з дапамогай метаду `filter()`
* Калі і навошта выкарыстоўваць ключы ў React

</YouWillLearn>

## Рэндэрынг даных з масіваў {/*rendering-data-from-arrays*/}

Дапусцім, у вас ёсць спіс кантэнту.

```js
<ul>
  <li>Крэола Кэтрын Джонсан: матэматык</li>
  <li>Марыа Хасэ Маліна-Паскель Энрыкес: хімік</li>
  <li>Махамад Абдус Салам: фізік</li>
  <li>Персі Лавон Джуліан: хімік</li>
  <li>Субрахманьян Чандрасекар: астрафізік</li>
</ul>
```

Адзіная розніца паміж гэтымі пунктамі спісу — іх змест, іх даныя. Пры стварэнні інтэрфейсаў часта трэба паказваць некалькі асобнікаў аднаго і таго ж кампанента, выкарыстоўваючы розныя даныя: ад спісаў каментарыяў да галерэй відарысаў профілю. У такіх сітуацыях вы можаце захоўваць гэтыя даныя ў аб'ектах і масівах JavaScript і выкарыстоўваць такія метады, як [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) і [`filter()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), каб рэндэрыць з іх спісы кампанентаў.

Вось кароткі прыклад таго, як стварыць спіс элементаў з масіву:

1. **Перамясціце** даныя ў масіў:

```js
const people = [
  'Крэола Кэтрын Джонсан: матэматык',
  'Марыа Хасэ Маліна-Паскель Энрыкес: хімік',
  'Махамад Абдус Салам: фізік',
  'Персі Лавон Джуліан: хімік',
  'Субрахманьян Чандрасекар: астрафізік'
];
```

2. **Пераўтварыце** элементы масіву `people` у новы масіў JSX вузлоў — `listItems`:

```js
const listItems = people.map(person => <li>{person}</li>);
```

3. **Вярніце** `listItems` з вашага кампанента, абгарнуўшы іх у `<ul>`:

```js
return <ul>{listItems}</ul>;
```

Вось што мусіць атрымацца ў выніку:

<Sandpack>

```js
const people = [
  'Крэола Кэтрын Джонсан: матэматык',
  'Марыа Хасэ Маліна-Паскель Энрыкес: хімік',
  'Махамад Абдус Салам: фізік',
  'Персі Лавон Джуліан: хімік',
  'Субрахманьян Чандрасекар: астрафізік'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

```css
li { margin-bottom: 10px; }
```

</Sandpack>
 
Звярніце ўвагу, што ў кансолі пясочніцы адлюстроўваецца памылка:

<ConsoleBlock level="error">

Warning: Each child in a list should have a unique "key" prop.

</ConsoleBlock>

Вы даведаецеся, як выправіць гэтую памылку пазней на гэтай старонцы. Перш чым перайсці да гэтага, давайце дададзім некаторую структуру да даных.

## Фільтрацыя масіваў элементаў {/*filtering-arrays-of-items*/}

Структуру гэтых даных можна палепшыць.

```js
const people = [{
  id: 0,
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
}, {
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',  
}, {
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
}];
```

Дапусцім, вы хочаце паказаць толькі людзей, чыя прафесія `'хімік'`. Каб вярнуць толькі гэтых людзей, вы можаце выкарыстоўваць JavaScript метад `filter()`. Гэты метад бярэ масіў элементаў, прапускае іх праз «тэст» (функцыю, якая вяртае `true` або `false`) і вяртае новы масіў толькі з тых элементаў, якія прайшлі праверку (вярнулі `true`).

Нам патрэбны толькі элементы, дзе `profession` з'яўляецца `'хімік'`. Функцыя "тэст" для гэтага выглядае так: `(person) => person.profession === 'хімік'`. Вось як сабраць гэта ўсё разам:

1. **Стварыце** новы масіў толькі з «хімікаў» (пераменная `chemists`) выклікаўшы метад `filter()` на `people` для фільтрацыі па `person.profession === 'хімік'`:

```js
const chemists = people.filter(person =>
  person.profession === 'хімік'
);
```

2. Цяпер **пераўтварыце** элементы масіву `chemists`:

```js {1,13}
const listItems = chemists.map(person =>
  <li>
     <img
       src={getImageUrl(person)}
       alt={person.name}
     />
     <p>
       <b>{person.name}:</b>
       {' ' + person.profession}.
       Галоўнае дасягненне: {person.accomplishment}
     </p>
  </li>
);
```

3. Нарэшце, **вярніце** `listItems` з вашага кампанента:

```js
return <ul>{listItems}</ul>;
```

<Sandpack>

```js App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'хімік'
  );
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession}.
        Галоўнае дасягненне: {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

```js data.js
export const people = [{
  id: 0,
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<Pitfall>

Стрэлачныя функцыі няяўна вяртаюць вынік выразу адразу пасля `=>`, таму не трэба выкарыстоўваць аператар `return`:

```js
const listItems = chemists.map(person =>
  <li>...</li> // Няяўнае вяртанне!
);
```

Аднак **вы павінны яўна напісаць `return`, калі пасля `=>` ідзе фігурная дужка `{`!**

```js
const listItems = chemists.map(person => { // Фігурная дужка
  return <li>...</li>;
});
```

Стрэлачныя функцыі, якія змяшчаюць `=> {` лічацца функцыямі з [«блокавай формай».](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#function_body) Яны дазваляюць вам напісаць больш, чым адзін радок кода, але вы *павінны* напісаць аператар `return` самастойна. Калі вы забудзецеся гэта зрабіць, функцыя нічога не верне!

</Pitfall>

## Захаванне парадку элементаў спісу з дапамогай `key` {/*keeping-list-items-in-order-with-key*/}

Звярніце ўвагу, што ўсе пясочніцы вышэй паказваюць памылку ў кансолі:

<ConsoleBlock level="error">

Warning: Each child in a list should have a unique "key" prop.

</ConsoleBlock>

Каб выправіць гэту памылку трэба прысвоіць кожнаму элементу масіву ключ (`key`) -- радок або лік, які адназначна ідэнтыфікуе яго сярод іншых элементаў у гэтым масіве:

```js
<li key={person.id}>...</li>
```

<Note>

JSX элементы, што ствараюцца непасрэдна ўнутры выкліку метаду `map()` заўсёды мусяць мець ключы!

</Note>

Ключы паведамляюць React, якому элементу масіву адпавядае кожны кампанент, каб потым ён мог іх супаставіць. Гэта важна, калі элементы масіву могуць перамяшчацца (напрыклад, з-за сартавання), устаўляцца або выдаляцца. Добра падабраны `key` дапамагае React зразумець, што менавіта адбылося, і зрабіць правільныя абнаўленні ў дрэве DOM.

Замест таго, каб генераваць ключы на хаду, вы павінны ўключыць іх у свае даныя:

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
        {' ' + person.profession}.
        Галоўнае дасягненне: {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

```js data.js active
export const people = [{
  id: 0, // Выкарыстоўваецца як ключ у JSX
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1, // Выкарыстоўваецца як ключ у JSX
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2, // Выкарыстоўваецца як ключ у JSX
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3, // Выкарыстоўваецца як ключ у JSX
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4, // Выкарыстоўваецца як ключ у JSX
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<DeepDive>

#### Адлюстраванне некалькіх вузлоў DOM для кожнага элемента спіса {/*displaying-several-dom-nodes-for-each-list-item*/}

Што рабіць, калі кожны элемент павінен адлюстроўваць не адзін, а некалькі вузлоў DOM?

Скарочаны сінтаксіс [фрагмента `<>...</>`](/reference/react/Fragment) не дазволіць вам перадаць ключ, таму вам трэба альбо згрупаваць іх у адзін `<div>`, альбо выкарыстоўваць крыху даўжэйшы і [больш выразны сінтаксіс `<Fragment>`:](/reference/react/Fragment#rendering-a-list-of-fragments)

```js
import { Fragment } from 'react';

// ...

const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

Фрагменты знікаюць з DOM, так што гэта створыць плоскі спіс `<h1>`, `<p>`, `<h1>`, `<p>` і г.д.

</DeepDive>

### Дзе ўзяць `key` {/*where-to-get-your-key*/}

Розныя крыніцы даных даюць розныя крыніцы ключоў:

* **Даныя з базы даных:** Калі вашы даныя прыходзяць з базы даных, вы можаце выкарыстоўваць ключы/ID з базы даных, якія па сваёй прыродзе з'яўляюцца ўнікальнымі.
* **Лакальна згенераваныя даныя:** Калі вашы даныя згенерыраваны і захоўваюцца лакальна (напрыклад, нататкі ў праграме для вядзення нататак), выкарыстоўвайце інкрэментны лічыльнік [`crypto.randomUUID()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/randomUUID) або пакет [`uuid`](https://www.npmjs.com/package/uuid) пры стварэнні элементаў.

### Правілы ключоў {/*rules-of-keys*/}

* **Ключы павінны быць унікальнымі сярод сваіх суседніх элементаў.** Аднак, можна выкарыстоўваць аднолькавыя ключы для JSX вузлоў у _розных_ масівах.
* **Ключы не павінны мяняцца** паколькі гэта пазбаўляе іх сэнсу! Не генеруйце іх падчас рэндэрынгу.

### Навошта React патрэбны ключы? {/*why-does-react-need-keys*/}

Уявіце, што файлы на вашым працоўным стале не маюць імёнаў. Замест гэтага вы б спасылаліся на іх па парадку - першы файл, другі файл і г.д. Магчыма да гэтага і можна прызвычаіцца, але калі вы выдаліце які-небудзь файл, парадак зменіцца і ўсё стане заблытаным. Другі файл стане першым файлам, трэці файл будзе другім файлам і г.д.

Імёны файлаў у папцы і JSX ключы ў масіве маюць падобную мэту. Яны дазваляюць нам адрозніваць элементы ад іншых элементаў у масіве. Добра падабраны ключ дае больш інфармацыі, чым пазіцыя ў масіве. Нават калі _пазіцыя_ мяняецца з-за змены парадку, `key` дазваляе React ідэнтыфікаваць элемент на працягу ўсяго яго існавання.

<Pitfall>

У вас можа ўзнікнуць спакуса выкарыстоўваць індэкс элемента ў масіве ў якасці ключа. Дарэчы, гэта тое, што React будзе выкарыстоўваць, калі вы не ўкажаце `key`. Але парадак, у якім вы рэндэрыце элементы, можа памяняцца з часам, калі які-небудзь элемент будзе ўстаўлены, выдалены або калі масіў будзе пераўпарадкаваны. Індэкс у якасці ключа часта прыводзіць да каварных памылак і памылак, што могуць збіваць з толку.

Аналагічна, не генеруйце ключы на ляту, напрыклад, з дапамогай `key={Math.random()}`. Гэта прывядзе да таго, што ключы ніколі не будуць супадаць паміж рэндэрамі, што прывядзе да перастварэння ўсіх вашых кампанентаў і DOM пры кожным рэндэры. Гэта не толькі павольна, але і прывядзе да страты любых даных уведзеных карыстальнікам унутры элементаў спіса. Замест гэтага выкарыстоўвайце стабільны ідэнтыфікатар, заснаваны на даных.

Звярніце ўвагу, што вашыя кампаненты не атрымаюць `key` у якасці пропса. Ён выкарыстоўваецца толькі як падказка для React. Калі вашаму кампаненту патрэбны ідэнтыфікатар, вы мусіце перадаць яго як асобны пропс: `<Profile key={id} userId={id} />`.

</Pitfall>

<Recap>

На гэтай старонцы вы даведаліся:

* Як перамясціць даныя з кампанентаў у такія структуры даных, як масівы і аб'екты.
* Як стварыць наборы падобных кампанентаў з дапамогай JavaScript метаду `map()` .
* Як ствараць масівы адфільтраваных элементаў з дапамогай JavaScript метаду `filter()`.
* Навошта і як задаваць `key` для кожнага кампанента ў калекцыі, каб React мог адсочваць кожны з іх, нават калі іх пазіцыя або даныя змяняюцца.

</Recap>



<Challenges>

#### Падзел спісу на два {/*splitting-a-list-in-two*/}

У гэтым прыкладзе паказаны спіс усіх людзей.

Змяніце яго так, каб паказаць два асобныя спісы адзін за адным: **Хімікі** і **Усе астатнія.** Як і раней, вы можаце вызначыць, ці з'яўляецца чалавек хімікам, праверыўшы `person.profession === 'хімік'`.

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
        {' ' + person.profession}.
        Галоўнае дасягненне: {person.accomplishment}
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
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

<Solution>

Вы можаце выкарыстоўваць `filter()` двойчы, стварыўшы два асобныя масівы, а затым выкарыстаць `map()` на абодвух:

<Sandpack>

```js App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'хімік'
  );
  const everyoneElse = people.filter(person =>
    person.profession !== 'хімік'
  );
  return (
    <article>
      <h1>Навукоўцы</h1>
      <h2>Хімікі</h2>
      <ul>
        {chemists.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession}.
              Галоўнае дасягненне: {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
      <h2>Усе астатнія</h2>
      <ul>
        {everyoneElse.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession}.
              Галоўнае дасягненне: {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </article>
  );
}
```

```js data.js
export const people = [{
  id: 0,
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

У гэтым рашэнні выклікі `map` размяшчаюцца непасрэдна ў бацькоўскіх элементах `<ul>`, але вы можаце вынесці іх у асобныя пераменныя, калі лічыце гэта больш зручным для чытання.

Паміж спісамі ўсё яшчэ існуе невялікае дубліраванне. Вы можаце пайсці далей і вынесці часткі, якія паўтараюцца, у кампанент `<ListSection>`:

<Sandpack>

```js App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

function ListSection({ title, people }) {
  return (
    <>
      <h2>{title}</h2>
      <ul>
        {people.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession}.
              Галоўнае дасягненне: {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </>
  );
}

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'хімік'
  );
  const everyoneElse = people.filter(person =>
    person.profession !== 'хімік'
  );
  return (
    <article>
      <h1>Навукоўцы</h1>
      <ListSection
        title="Хімікі"
        people={chemists}
      />
      <ListSection
        title="Усе астатнія"
        people={everyoneElse}
      />
    </article>
  );
}
```

```js data.js
export const people = [{
  id: 0,
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

Вельмі ўважлівы чытач можа заўважыць, што пры двух выкліках метаду `filter` мы двойчы правяраем прафесію кожнага чалавека. Праверка ўласцівасці працуе вельмі хутка, таму ў гэтым прыкладзе гэта нармальна. Калі б ваша логіка была больш затратная, вы маглі б замяніць выклікі метаду `filter` цыклам, які ўручную стварае масівы і правярае кожнага чалавека адзін раз.

На самай справе, калі `people` ніколі не мяняецца, вы можаце вынесці гэты код са свайго кампанента. З пункту гледжання React, адзінае, што мае значэнне, гэта тое, што ў канцы вы даяце яму масіў JSX вузлоў. React не важна, як вы ствараеце гэты масіў:

<Sandpack>

```js App.js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

let chemists = [];
let everyoneElse = [];
people.forEach(person => {
  if (person.profession === 'хімік') {
    chemists.push(person);
  } else {
    everyoneElse.push(person);
  }
});

function ListSection({ title, people }) {
  return (
    <>
      <h2>{title}</h2>
      <ul>
        {people.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession}.
              Галоўнае дасягненне: {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </>
  );
}

export default function List() {
  return (
    <article>
      <h1>Scientists</h1>
      <ListSection
        title="Хімікі"
        people={chemists}
      />
      <ListSection
        title="Усе астатнія"
        people={everyoneElse}
      />
    </article>
  );
}
```

```js data.js
export const people = [{
  id: 0,
  name: 'Крэола Кэтрын Джонсан',
  profession: 'матэматык',
  accomplishment: 'разлікі для касмічных палётаў',
  imageId: 'MK3eW3A'
}, {
  id: 1,
  name: 'Марыа Хасэ Маліна-Паскель Энрыкес',
  profession: 'хімік',
  accomplishment: 'адкрыццё арктычнай азонавай дзіркі',
  imageId: 'mynHUSa'
}, {
  id: 2,
  name: 'Махамад Абдус Салам',
  profession: 'фізік',
  accomplishment: 'тэорыя электрамагнетызму',
  imageId: 'bE7W1ji'
}, {
  id: 3,
  name: 'Персі Лавон Джуліан',
  profession: 'хімік',
  accomplishment: 'піянер у вытворчасці картызону, стэроідаў і супрацьзачаткавых таблетак',
  imageId: 'IOjWm71'
}, {
  id: 4,
  name: 'Субрахманьян Чандрасекар',
  profession: 'астрафізік',
  accomplishment: 'разлік масы белага карліка',
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
  grid-template-columns: auto 1fr;
  gap: 20px;
  align-items: center;
}
img { width: 100px; height: 100px; border-radius: 50%; }
```

</Sandpack>

</Solution>

#### Укладзеныя спісы ў адным кампаненце {/*nested-lists-in-one-component*/}

Стварыце спіс рэцэптаў з гэтага масіву! Для кожнага рэцэпту ў масіве пакажыце яго назву ўнутры `<h2>` і пералічыце інгрэдыенты ў `<ul>`.

<Hint>

Для гэтага спатрэбіцца ўкладанне двух розных выклікаў метаду `map`.

</Hint>

<Sandpack>

```js App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Рэцэпты</h1>
    </div>
  );
}
```

```js data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Грэчаская салата',
  ingredients: ['памідоры', 'агурок', 'цыбуля', 'аліўкі', 'фета']
}, {
  id: 'hawaiian-pizza',
  name: 'Гавайская піца',
  ingredients: ['цеста для піцы', 'соус для піцы', 'мацарэла', 'вяндліна', 'ананас']
}, {
  id: 'hummus',
  name: 'Хумус',
  ingredients: ['нут', 'аліўкавы алей', 'зубкі часнаку', 'лімон', 'тахіні']
}];
```

</Sandpack>

<Solution>

Вось адзін са спосабаў, як вы можаце гэта зрабіць:

<Sandpack>

```js App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Рэцэпты</h1>
      {recipes.map(recipe =>
        <div key={recipe.id}>
          <h2>{recipe.name}</h2>
          <ul>
            {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
        </div>
      )}
    </div>
  );
}
```

```js data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Грэчаская салата',
  ingredients: ['памідоры', 'агурок', 'цыбуля', 'аліўкі', 'фета']
}, {
  id: 'hawaiian-pizza',
  name: 'Гавайская піца',
  ingredients: ['цеста для піцы', 'соус для піцы', 'мацарэла', 'вяндліна', 'ананас']
}, {
  id: 'hummus',
  name: 'Хумус',
  ingredients: ['нут', 'аліўкавы алей', 'зубкі часнаку', 'лімон', 'тахіні']
}];
```

</Sandpack>

Кожны з рэцэптаў у `recipes` ужо мае поле `id`, таму знешні цыкл і выкарыстоўвае яго ў якасці свайго `key`. У нашай мадэлі няма ідэнтыфікатара, які можна выкарыстоўваць для перабору інгрэдыентаў. Тым не менш, разумна выказаць здагадку, што адзін і той жа інгрэдыент не будзе пералічаны двойчы ў адным рэцэпце, таму яго назва можа выкарыстоўвацца ў якасці `key`. У якасці альтэрнатывы вы можаце змяніць структуру даных і дадаць ідэнтыфікатары, або выкарыстоўваць індэкс у якасці `key` (з агаворкай, што вы не зможаце бяспечна змяніць парадак інгрэдыентаў).

</Solution>

#### Вынясенне кампанента элемента спісу {/*extracting-a-list-item-component*/}

Кампанент `RecipeList` змяшчае два ўкладзеныя выклікі метаду `map`. Каб спрасціць яго, вылучыце з яго кампанент `Recipe`, які будзе прымаць пропсы `id`, `name` і `ingredients`. Дзе вы размясціце знешні `key` і чаму?

<Sandpack>

```js App.js
import { recipes } from './data.js';

export default function RecipeList() {
  return (
    <div>
      <h1>Рэцэпты</h1>
      {recipes.map(recipe =>
        <div key={recipe.id}>
          <h2>{recipe.name}</h2>
          <ul>
            {recipe.ingredients.map(ingredient =>
              <li key={ingredient}>
                {ingredient}
              </li>
            )}
          </ul>
        </div>
      )}
    </div>
  );
}
```

```js data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Грэчаская салата',
  ingredients: ['памідоры', 'агурок', 'цыбуля', 'аліўкі', 'фета']
}, {
  id: 'hawaiian-pizza',
  name: 'Гавайская піца',
  ingredients: ['цеста для піцы', 'соус для піцы', 'мацарэла', 'вяндліна', 'ананас']
}, {
  id: 'hummus',
  name: 'Хумус',
  ingredients: ['нут', 'аліўкавы алей', 'зубкі часнаку', 'лімон', 'тахіні']
}];
```

</Sandpack>

<Solution>

Вы можаце скапіяваць і ўставіць JSX са знешняга выкліку метаду `map` у новы кампанент `Recipe` і вярнуць гэты JSX. Затым вы можаце змяніць `recipe.name` на `name`, `recipe.id` на `id`, і гэтак далей, і перадаць іх у якасці пропсаў у `Recipe`:

<Sandpack>

```js
import { recipes } from './data.js';

function Recipe({ id, name, ingredients }) {
  return (
    <div>
      <h2>{name}</h2>
      <ul>
        {ingredients.map(ingredient =>
          <li key={ingredient}>
            {ingredient}
          </li>
        )}
      </ul>
    </div>
  );
}

export default function RecipeList() {
  return (
    <div>
      <h1>Recipes</h1>
      {recipes.map(recipe =>
        <Recipe {...recipe} key={recipe.id} />
      )}
    </div>
  );
}
```

```js data.js
export const recipes = [{
  id: 'greek-salad',
  name: 'Грэчаская салата',
  ingredients: ['памідоры', 'агурок', 'цыбуля', 'аліўкі', 'фета']
}, {
  id: 'hawaiian-pizza',
  name: 'Гавайская піца',
  ingredients: ['цеста для піцы', 'соус для піцы', 'мацарэла', 'вяндліна', 'ананас']
}, {
  id: 'hummus',
  name: 'Хумус',
  ingredients: ['нут', 'аліўкавы алей', 'зубкі часнаку', 'лімон', 'тахіні']
}];
```

</Sandpack>

Тут `<Recipe {...recipe} key={recipe.id} />` — гэта скарочаны сінтаксіс, які кажа «перадаць усе ўласцівасці аб'екта `recipe` як пропсы ў кампанент `Recipe`». Вы таксама можаце напісаць кожны пропс яўна: `<Recipe id={recipe.id} name={recipe.name} ingredients={recipe.ingredients} key={recipe.id} />`.

**Звярніце ўвагу, што `key` пазначаны ў самім кампаненце `<Recipe>`, а не ў каранёвым `<div>`, які вяртаецца з `Recipe`.** Усё таму, што гэты `key` неабходны непасрэдна ў кантэксце навакольнага масіву. Раней у вас быў масіў `<div>`, таму кожнаму з іх быў патрэбны ключ, але цяпер у вас ёсць масіў `<Recipe>`. Іншымі словамі, калі вы выносіце кампанент, не забывайце пакідаць `key` па-за JSX, які вы капіюеце і ўстаўляеце.

</Solution>

#### Спіс з раздзяляльнікам {/*list-with-a-separator*/}

У гэтым прыкладзе рэндэрыцца знакамітае хайку Тацібана Хокусі, кожны радок якога абгорнуты тэгам `<p>`. Ваша задача — уставіць раздзяляльнік `<hr />` паміж кожным абзацам. Ваша выніковая структура павінна выглядаць так:

```js
<article>
  <p>Я пішу, сціраю, перапісваю</p>
  <hr />
  <p>Сціраю зноў, а потым</p>
  <hr />
  <p>Зацвіў мак.</p>
</article>
```

Хайку змяшчае толькі тры радкі, але ваша рашэнне павінна працаваць з любой колькасцю радкоў. Звярніце ўвагу, што элементы `<hr />` павінны быць толькі *паміж* элементамі `<p>`, а не ў пачатку або ў канцы!

<Sandpack>

```js
const poem = {
  lines: [
    'Я пішу, сціраю, перапісваю',
    'Сціраю зноў, а потым',
    'Зацвіў мак.'
  ]
};

export default function Poem() {
  return (
    <article>
      {poem.lines.map((line, index) =>
        <p key={index}>
          {line}
        </p>
      )}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

(Гэта рэдкі выпадак, калі выкарыстоўванне індэкса ў якасці ключа — прымальны варыянт, таму што парадак радкоў верша ніколі не будзе зменены.)

<Hint>

Вам трэба альбо пераўтварыць `map` у звычайны цыкл, альбо выкарыстоўваць фрагмент.

</Hint>

<Solution>

Вы можаце выкарыстоўваць звычайны цыкл, устаўляючы `<hr />` і `<p>...</p>` у выхадны масіў па ходзе выканання:

<Sandpack>

```js
const poem = {
  lines: [
    'Я пішу, сціраю, перапісваю',
    'Сціраю зноў, а потым',
    'Зацвіў мак.'
  ]
};

export default function Poem() {
  let output = [];

  // Запоўненне выхаднога масіву
  poem.lines.forEach((line, i) => {
    output.push(
      <hr key={i + '-separator'} />
    );
    output.push(
      <p key={i + '-text'}>
        {line}
      </p>
    );
  });
  // Выдаляем першы <hr />
  output.shift();

  return (
    <article>
      {output}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

Выкарыстанне першапачатковага радковага індэкса ў якасці `key` больш не працуе, таму што цяпер кожны раздзяляльнік і абзац знаходзяцца ў адным і тым жа масіве. Аднак вы можаце даць кожнаму з іх асобны ключ з дапамогай суфікса, напрыклад. `key={i + '-text'}`.

У якасці альтэрнатывы вы можаце стварыць калекцыю фрагментаў, якія змяшчаюць `<hr />` і `<p>...</p>`. Аднак скарочаны сінтаксіс `<>...</>` не падтрымлівае перадачу ключоў, таму вам трэба напісаць `<Fragment>` відавочна:

<Sandpack>

```js
import { Fragment } from 'react';

const poem = {
  lines: [
    'Я пішу, сціраю, перапісваю',
    'Сціраю зноў, а потым',
    'Зацвіў мак.'
  ]
};

export default function Poem() {
  return (
    <article>
      {poem.lines.map((line, i) =>
        <Fragment key={i}>
          {i > 0 && <hr />}
          <p>{line}</p>
        </Fragment>
      )}
    </article>
  );
}
```

```css
body {
  text-align: center;
}
p {
  font-family: Georgia, serif;
  font-size: 20px;
  font-style: italic;
}
hr {
  margin: 0 120px 0 120px;
  border: 1px dashed #45c3d8;
}
```

</Sandpack>

Памятайце, што фрагменты (часта пішуцца як `<> </>`) дазваляюць групаваць JSX вузлы без дадавання дадатковых `<div>`!

</Solution>

</Challenges>

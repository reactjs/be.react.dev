---
title: JavaScript у JSX з дапамогай фігурных дужак
---

<Intro>

JSX дазваляе пісаць HTML-падобную разметку ўнутры JavaScript файла, каб трымаць логіку рэндэрынгу і змесціва ў адным месцы. Іншы раз вы захочаце дадаць трохі логікі JavaScript або звярнуцца да дынамічнай ўласцівасці у гэтай разметцы. У гэтай выпадку вы можаце выкарыстоўваць фігурныя дужкі ў вашым JSX, каб адкрыць доступ да JavaScript.

</Intro>

<YouWillLearn>

* Як перадаваць радкі з дапамогай двукоссяў
* Як спасылацца на JavaScript пераменную ўнутры JSX з дапамогай фігурных дужак
* Як выклікаць JavaScript функцыю ў JSX з дапамогай фігурных дужак
* Як выкарыстоўваць JavaScript аб'ект у JSX з дапамогай фігурных дужак

</YouWillLearn>

## Перадача радкоў з дапамогай двукоссяў {/*passing-strings-with-quotes*/}

Калі вы хочаце перадаць радковы атрыбут у JSX, вам трэба змясціць яго ў адзінарныя або двайныя двукоссі:

<Sandpack>

```js
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/7vQD0fPs.jpg"
      alt="Грэгорыа І. Зара"
    />
  );
}
```

```css
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Тут, `"https://i.imgur.com/7vQD0fPs.jpg"` і `"Грэгорыа І. Зара"` перадаюцца як радкі.

Але што, калі вы хочаце дынамічна задаць значэнне `src` або `alt`? Вы можаце **выкарыстоўваць значэнне з JavaScript, замяніўшы `"` і `"` на `{` і `}`**:

<Sandpack>

```js
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Грэгорыа І. Зара';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

```css
.avatar { border-radius: 50%; height: 90px; }
```

</Sandpack>

Звярніце ўвагу на розніцу паміж запісам `className="avatar"`, які вызначае назву CSS класа `"avatar"`, што робіць відарыс круглым, і запісам `src={avatar}`, які счытвае значэнне JavaScript пераменнай пад назвай `avatar`. Гэта адбываецца таму, што фігурныя дужкі дазваляюць працаваць з JavaScript прама ў вашай разметцы!

## Выкарыстанне фігурных дужак: акно ў свет JavaScript {/*using-curly-braces-a-window-into-the-javascript-world*/}

JSX — гэта адмысловы спосаб напісання JavaScript. Гэта азначае, што ўнутры JSX можна выкарыстоўваць JavaScript — з дапамогай фігурных дужак `{ }`. У прыведзеным ніжэй прыкладзе спачатку аб'яўляецца пераменная `name`, у якой захоўваецца імя вучонага, а потым гэтая пераменная убудоўваецца ў `<h1>` з дапамогай фігурных дужак:

<Sandpack>

```js
export default function TodoList() {
  const name = 'Грэгорыа І. Зара';
  return (
    <h1>Спіс спраў {name}</h1>
  );
}
```

</Sandpack>

Паспрабуйце змяніць значэнне пераменнай `name` з `'Грэгорыя І. Зара'` на `'Хедзі Ламар'`. Бачыце, як змяняецца назва спісу?

Любы JavaScript выраз будзе працаваць паміж фігурнымі дужкамі, у тым ліку выклікі функцый накшталт `formatDate()`:

<Sandpack>

```js
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>Спіс спраў на {formatDate(today)}</h1>
  );
}
```

</Sandpack>

### Дзе выкарыстоўваць фігурныя дужкі {/*where-to-use-curly-braces*/}

Унутры JSX вы можаце выкарыстоўваць фігурныя дужкі толькі двума спосабамі:

1. **Як тэкст** непасрэдна ўнутры JSX тэга: `<h1>Спіс спраў {name}</h1>` будзе працаваць, але `<{tag}>Спіс спраў Грэгорыа І. Зара</{tag}>` працаваць не будзе.
2. **У якасці атрыбутаў** адразу пасля знака `=`: `src={avatar}` прачытае пераменную `avatar`, але `src="{avatar}"` перадасць радок `"{avatar}"`.

## Выкарыстанне падвойных фігурных дужак: CSS і іншыя аб'екты ў JSX {/*using-double-curlies-css-and-other-objects-in-jsx*/}

У дадатак да радкоў, лічбаў і іншых JavaScript выразаў, у JSX вы можаце перадаваць нават аб'екты. Аб'екты таксама пазначаюцца фігурнымі дужкамі, напрыклад `{ name: "Хедзі Ламар", inventions: 5 }`. Такім чынам, каб перадаць JS аб'ект у JSX, вы павінны абгарнуць аб'ект у яшчэ адну пару фігурных дужак: `person={{ name: "Хедзі Ламар", inventions: 5 }}`.

Гэта бачна на прыкладзе убудаваных CSS стыляў у JSX. React не патрабуе ад вас выкарыстання ўбудаваных стыляў (CSS класы выдатна працуюць у большасці выпадкаў). Але калі вам патрэбен убудаваны стыль, вы перадаяце аб'ект у атрыбут `style`:

<Sandpack>

```js
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Палепшыць відэатэлефон</li>
      <li>Падрыхтаваць лекцыі па аэранаўтыцы</li>
      <li>Папрацаваць над рухавіком на спірце</li>
    </ul>
  );
}
```

```css
body { padding: 0; margin: 0 }
ul { padding: 20px 20px 20px 40px; margin: 0; }
```

</Sandpack>

Паспрабуйце змяніць значэнні `backgroundColor` і `color`.

Вы можаце ўбачыць JavaScript аб'ект у фігурных дужках, калі напішаце яго так:

```js {2-5}
<ul style={
  {
    backgroundColor: 'black',
    color: 'pink'
  }
}>
```

У наступны раз, калі вы ўбачыце ў JSX `{{` і `}}`, ведайце, што гэта не што іншае, як аб'ект унутры фігурных дужак JSX!

<Pitfall>

Убудаваныя ўласцівасці атрыбута `style` запісваюцца ў camelCase. Напрыклад, HTML код `<ul style="background-color: black">` будзе запісаны ў вашым кампаненце як `<ul style={{ backgroundColor: 'black' }}>`.

</Pitfall>

## Яшчэ аб JavaScript аб'ектах і фігурных дужках {/*more-fun-with-javascript-objects-and-curly-braces*/}

Вы можаце аб'яднаць некалькі выразаў у адзін аб'ект і спасылацца на іх у JSX з дапамогай фігурных дужак:

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
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Грэгорыа І. Зара"
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

У гэтым прыкладзе JavaScript аб'ект `person` змяшчае радок `name` і аб'ект `theme`:

```js
const person = {
  name: 'Грэгорыа І. Зара',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};
```

Кампанент можа выкарыстоўваць гэтыя значэнні з аб'екта `person` наступным чынам:

```js
<div style={person.theme}>
  <h1>Спіс спраў {person.name}</h1>
```

JSX вельмі мінімалістычная мова шаблонаў, таму што яна дазваляе арганізоўваць даныя і логіку з дапамогай JavaScript.

<Recap>

Цяпер вы ведаеце амаль усё пра JSX:

* JSX атрыбуты у двукоссях перадаюцца як радкі.
* Фігурныя дужкі дазваляюць выкарыстоўваць JavaScript логіку і пераменныя ў вашай разметцы.
* Яны працуюць унутры змесціва JSX тэга або непасрэдна пасля `=` у атрыбутах.
* `{{` і `}}` не з'яўляюцца спецыяльным сінтаксісам: гэта JavaScript аб'ект, схаваны ўнутры фігурных дужак JSX.

</Recap>

<Challenges>

#### Выпраўце памылку {/*fix-the-mistake*/}

Гэты код выдае памылку `Objects are not valid as a React child`:

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
      <h1>Спіс спраў {person}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Грэгорыа І. Зара"
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

Вы можаце знайсці праблему?

<Hint>Шукайце тое, што знаходзіцца ўнутры фігурных дужак. Ці правільна мы туды ўсё змяшчаем?</Hint>

<Solution>

Так адбываецца таму, што гэты прыклад рэндэрыць *сам аб'ект* у разметку, а не радок: `<h1>Спіс спраў {person}</h1>` спрабуе адрэндэрыць увесь аб'ект `person`! Рэндэр аб'ектаў як тэкставае змесціва выклікае памылку, таму што React не ведае, як іх адлюстроўваць.

Каб выправіць гэта, заменіце `<h1>Спіс спраў {person}</h1>` на `<h1>Спіс спраў {person.name}</h1>`:

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
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Грэгорыа І. Зара"
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

</Solution>

#### Выміце інфармацыю ў аб'ект {/*extract-information-into-an-object*/}

Выміце URL-адрас відарыса ў аб'ект `person`.

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
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Грэгорыа І. Зара"
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

<Solution>

Перамясціце URL-адрас відарыса ва ўласцівасць `person.imageUrl` і прачытайце яго з тэга `<img>` з дапамогай фігурных дужак:

<Sandpack>

```js
const person = {
  name: 'Грэгорыа І. Зара',
  imageUrl: "https://i.imgur.com/7vQD0fPs.jpg",
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src={person.imageUrl}
        alt="Грэгорыа І. Зара"
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

</Solution>

#### Напішыце выраз унутры фігурных дужак JSX {/*write-an-expression-inside-jsx-curly-braces*/}

У прыведзеным ніжэй аб'екце поўны URL відарыса падзелены на чатыры часткі: базавы URL, `imageId`, `imageSize` і пашырэнне файла.

Мы хочам аб'яднаць гэтыя атрыбуты разам у URL-адрас відарыса: базавы URL (заўсёды `'https://i.imgur.com/'`), `imageId` (`'7vQD0fP'`), `imageSize` (`'s'`) і пашырэнне файла (заўсёды `'.jpg'`). Аднак нешта не так з тым, як тэг `<img>` вызначае свой `src`.

Вы можаце гэта выправіць?

<Sandpack>

```js

const baseUrl = 'https://i.imgur.com/';
const person = {
  name: 'Грэгорыа І. Зара',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src="{baseUrl}{person.imageId}{person.imageSize}.jpg"
        alt={person.name}
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
.avatar { border-radius: 50%; }
```

</Sandpack>

Каб пераканацца, што ваша выпраўленне спрацавала, паспрабуйце змяніць значэнне `imageSize` на `'b'`. Пасля вашых змяненняў памер відарыса павінен змяніцца.

<Solution>

Вы можаце напісаць так: `src={baseUrl + person.imageId + person.imageSize + '.jpg'}`.

1. `{` адкрывае JavaScript выраз
2. `baseUrl + person.imageId + person.imageSize + '.jpg'` стварае правільны радок URL
3. `}` закрывае JavaScript выраз

<Sandpack>

```js
const baseUrl = 'https://i.imgur.com/';
const person = {
  name: 'Грэгорыа І. Зара',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src={baseUrl + person.imageId + person.imageSize + '.jpg'}
        alt={person.name}
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
.avatar { border-radius: 50%; }
```

</Sandpack>

Вы таксама можаце перанесці гэты выраз у асобную функцыю, напрыклад `getImageUrl` ніжэй:

<Sandpack>

```js App.js
import { getImageUrl } from './utils.js'

const person = {
  name: 'Грэгорыа І. Зара',
  imageId: '7vQD0fP',
  imageSize: 's',
  theme: {
    backgroundColor: 'black',
    color: 'pink'
  }
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>Спіс спраў {person.name}</h1>
      <img
        className="avatar"
        src={getImageUrl(person)}
        alt={person.name}
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

```js utils.js
export function getImageUrl(person) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    person.imageSize +
    '.jpg'
  );
}
```

```css
body { padding: 0; margin: 0 }
body > div > div { padding: 20px; }
.avatar { border-radius: 50%; }
```

</Sandpack>

Пераменныя і функцыі могуць дапамагчы вам спрасціць разметку!

</Solution>

</Challenges>

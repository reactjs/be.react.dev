---
title: Імпартаванне і экспартаванне кампанентаў
---

<Intro>

Магіяй кампанентаў з’яўляецца іх гатоўнасць да паўторнага выкарыстання: вы можаце стварыць кампанент, які будзе змяшчаць у сабе іншыя кампаненты. Але з павелічэннем колькасці ўкладзеных кампанентаў, часта з’яўляецца і патрэба ў разбванні кампанентаў па ўласных файлах. Гэта дазваляе захоўваць вашымі файламі прасцейшымі для разумення, а кампаненты прасцейшымі для паўторнага выкарыстання.

</Intro>

<YouWillLearn>

* Што такое файл каранёвага кампанента
* Як імпартаваць і экспартаваць кампаненты
* Калі выкарыстоўваць прадвызначанае імпартаванне і экспартаванне, а калі найменнае.
* Як імпартаваць і экспартаваць некалькі кампанентаў з аднаго файла
* Як падзяліць кампанент на некалькі файлаў

</YouWillLearn>

## Файл каранёвага кампанента {/*the-root-component-file*/}

На старонцы «[Ваш першы кампанент](/learn/your-first-component)», вы стварылі кампаненты `Profile` і `Gallery`, яны выглядаюць наступным чынам:

<Sandpack>

```js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Кэтрын Джонсан"
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

У дадзеным прыкладзе ўсе кампаненты існуюць у **файле каранёвага кампанента**, які называецца `App.js`. У [Create React App](https://create-react-app.dev/), вашая праграма змяшчаецца ў `src/App.js`. У залежнасці ад вашага асяроддзя, каранёвы кампанент можа змяшчацца і ў іншым файле. Калі вы выкарыстоўваеце фрэймворк з маршрутызацыяй, што базуецца на файлавай сістэме, (напрыклад, Next.js), ваш каранёвы кампанент будзе розным для кожнай старонкі.

## Экспартаванне і імпартаванне кампанентаў {/*exporting-and-importing-a-component*/}

Што калі вы захочаце змяніць старонку ў будучыні, дадаўшы сюды спіс навуковых кніг? Ці перамясціўшы профілі куды-небудзь яшчэ? Тады з’яўляецца сэнс вынесці `Gallery` і `Profile` з файла каранёвага кампанента. Гэта зробіць іх модульнымі і прыдатнымі для паўторнага выкарыстання ў іншых файлах. Каб вынесці кампанент, трэба выканаць наступныя крокі:

1. **Стварыце** новы JS файл, куды вы складзяце кампанент.
2. **Экспартуйце** вашую функцыю з гэтага файла (Выкарыстоўваючы [прадвызначанае](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/export#using_the_default_export) ці [найменнае](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/export#using_named_exports) імпартаванне).
3. **Імпартуйце** кампанент у файл, дзе вы хочаце яго выкарыстоўваць (адпаведна выкарыстоўваючы [прадвызначанае](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/import#importing_defaults) ці [найменнае](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/import#import_a_single_export_from_a_module) імпартаванне, у залежнасці ад таго, як вы экспартуеце свой кампанент).

У дадзеным прыкладзе `Profile` і `Gallery` былі вынесеныя з `App.js` у асобны файл `Gallery.js`. Цяпер вы можаце змяніць `App.js`, каб імпартаваць `Gallery` з `Gallery.js`:

<Sandpack>

```js App.js
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```js Gallery.js
function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
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

Заўважце якім чынам цяпер код у дадзеным прыкладзе разбіты на два файлы з кампанентамі:

1. `Gallery.js`:
     - Вызначае кампанент `Profile`, які  выкарыстоўваецца толькі ў межах файла, таму не экспартуецца.
     - Экспартуе кампанент `Gallery`, выкарыстоўваючы **прадвызначанае экспартаванне**.
2. `App.js`:
     - Імпартуе `Gallery` выкарыстоўваючы **прадвызначанае імпартаванне** з `Gallery.js`.
     - Экспартуе каранёвы кампанент `App`, выкарыстоўваючы **прадвызначанае экспартаванне**.


<Note>

Вы таксама можаце сутыкнуцца з выпадкамі, калі ў назве файла прапушчаная частка `.js`:

```js 
import Gallery from './Gallery';
```

Абодва варыянты: `'./Gallery.js'` і `'./Gallery'` будуць працаваць з React, хай і першы варыянт бліжэйшы да таго, як працуюць [натыўныя ES модулі](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules).

</Note>

<DeepDive>

#### Розніца між прадвызначаным і найменным экспартаваннем {/*default-vs-named-exports*/}

У JavsScript ёсць два асноўных спосабы экспартавання: прадвызначанае экспартаванне і найменнае экспартаванне. У нашых прыкладах дагэтуль выкарыстоўвалася толькі прадвызначанае экспартаванне. Але вы можаце выкарыстоўваць любы з іх ці адразу некалькі ў межах аднаго файла. **Адзін файл можа выкарыстоўваць _прадвызначанае_ экспартаванне не болей за адзін раз, але _найменныя_ экспартаванні можна выкарыстоўваць колькі заўгодна**.

![Прадвызначанае і найменныя спосабы экспартавання](/images/docs/illustrations/i_import-export.svg)

Ад таго, як вы экспартуеце кампанент, залежыць тое, як вам давядзецца яго імпартаваць. Вы атрымаеце памылку, калі паспрабуеце імпартаваць прадвызначана экспартаваны кампанент такім жа чынам, якім трэба імпартаваць найменна экспартаваныя кампаненты! Гэтая табліца дапаможа вам не блытацца:

| Сінтаксіс           | Аператар экспартавання                           | Аператар імпартавання                          |
| -----------      | -----------                                | -----------                               |
| Прадвызначанае  | `export default function Button() {}` | `import Button from './Button.js';`     |
| Найменнае    | `export function Button() {}`         | `import { Button } from './Button.js';` |

Калі вы выкарыстоўваеце _прадвызначанае_ імпартаванне, вы можаце выкарыстоўваць любое імя ў `import`. Напрыклад, вы можаце напісаць `import Banana from './Button.js'` і атрымаць усё той жа прадвызначана экспартаваны кампанент. Гэтаму супрацьпастаўляюцца найменныя імпартаванні: пры іх назва павінна супадаць у абодвух файлах. Таму яны і называюцца _найменнымі_!

**Звычайна людзі выкарыстоўваць прадвызначанае экспартаванне, калі файл экспартуе толькі адзін кампанент, і найменныя экспартаванні, калі файл экспартуе некалькі кампанентаў**. Незалежна ад таго, якому стылю напісання коду вы аддаяце перавагу, заўсёды называйце кампаненты і іх файлы асэнсавана. Кампаненты без назваў, такія як `export default () => {}`, выкарыстоўваць не варта, бо яны ўскладняюць працэс адладжвання.

</DeepDive>

## Экспартаванне і імпартаванне некалькіх кампанентаў з аднаго файла {/*exporting-and-importing-multiple-components-from-the-same-file*/}

Што калі вы хочаце паказваць адзін `Profile` замест галерэі? Вы можаце экспартаваць кампанент `Profile` таксама. Але `Gallery.js` ужо мае *прадвызначанае* экспартаванне, і вы не можаце скарыстацца прадвызначаным экспартаваннем _двойчы_. Вы можаце стварыць новы файл з прадвызначаным экспартаваннем, але таксама вы можаце дадаць найменнае экспартаванне для `Profile`. **Адзін файл можа мець толькі адное прадвызначанае экспартаванне, але любую колькасць найменных экспартаванняў!**

<Note>

Каб пазбавіцца патэнцыйнай блытаніны праз розныя варыянты экспартавання, некаторыя людзі, асабліва хто працуюць у камандзе, карыстаюцца толькі адным стылем экспартавання (прадвызначаным ці найменным) ці прынамсі пазбягаюць выкарыстання іх разам у межах аднаго файла. Рабіце тое, што вам падыдзе найлепш!

</Note>

Спачатку, **экспартуйце** `Profile` з `Gallery.js` выкарыстоўваючы найменнае экспартаванне (без ключавога слова `default`):

```js
export function Profile() {
  // ...
}
```

Затым, **імпартуйце** `Profile` з `Gallery.js` у `App.js` выкарыстоўваючы найменнае імпартаванне (з фігурнымі дужкамі):

```js
import { Profile } from './Gallery.js';
```

Нарэшце, **дадайце** `<Profile />` унутр кампанента `App`:

```js
export default function App() {
  return <Profile />;
}
```

Цяпер `Gallery.js` змяшчае два кампаненты: прадвызначана экспартаваны `Gallery` і найменна экспартаваны `Profile`. `App.js` імпартуе іх разам. Паспрабуйце замяніць `<Profile />` на `<Gallery />` і назад у наступным прыкладзе:

<Sandpack>

```js App.js
import Gallery from './Gallery.js';
import { Profile } from './Gallery.js';

export default function App() {
  return (
    <Profile />
  );
}
```

```js Gallery.js
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
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

Цяпер вы выкарыстоўваеце адразу прадвызначанае і найменнае экспартаванні:

* `Gallery.js`:
  - Экспартуе кампанент `Profile`, выкарыстоўваючы **найменны экспарт**, экспартаваны кампанент мае назву **`Profile`**.
  - Экспартуе кампанент `Gallery`, выкарыстоўваючы **прадвызначанае экспартаванне**.
* `App.js`:
  - Імпартуе кампанент `Profile`, выкарыстоўваючы **найменнае імпартаванне, пад назвай `Profile`** з `Gallery.js`.
  - Імпартуе `Gallery`, выкарыстоўваючы **прадвызначанае экспартаванне**, з `Gallery.js`.
  - Экспартуе каранёвы кампаненты `App`, выкарыстоўваючы **прадвызначанае экспартаванне**.

<Recap>

На гэтай старонцы вы вывучылі:

* Што такое файл каранёвага кампанента
* Як імпартаваць і экспартаваць кампаненты
* Калі і як выкарыстоўваць прадвызначанае імпартаванне і экспартаванне, а калі найменнае.
* Як экспартаваць некалькі кампанентаў з аднаго файла

</Recap>



<Challenges>

#### Працягніце разбіванне кампанентаў {/*split-the-components-further*/}

На дадзены момант `Gallery.js` экспартуе абодва кампаненты: `Profile` і `Gallery`, што можа блытаць.

Перанясіце кампанент `Profile` ва ўласны файл `Profile.js`, затым змяніце кампанент `App`, каб ён рэндэрыў абодва кампаненты: `<Profile />` і `<Gallery />` адно пасля іншага.

Вы можаце выкарыстоўваць для кампанента `Profile` любы стыль экспартавання: прадвызначаны ці найменны, але не забудзьцеся пераканацца, што выкарыстоўваеце адпаведны сінтаксіс у абодвух файлах: `App.js` і `Gallery.js`! Звярніцеся да табліцы для прасцейшага разумення:

| Сінтаксіс           | Аператар экспарту                           | Аператар імпарту                          |
| -----------      | -----------                                | -----------                               |
| Default  | `export default function Button() {}` | `import Button from './Button.js';`     |
| Найменнае    | `export function Button() {}`         | `import { Button } from './Button.js';` |

<Hint>

Не забудзьцеся на імпартаванне вашых кампанентаў там, дзе вы іх выкарыстоўваеце. Ці не выкарыстоўваецца кампанент `Profile` таксама і ў кампаненце `Gallery`?

</Hint>

<Sandpack>

```js App.js
import Gallery from './Gallery.js';
import { Profile } from './Gallery.js';

export default function App() {
  return (
    <div>
      <Profile />
    </div>
  );
}
```

```js Gallery.js active
// Move me to Profile.js!
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
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

```js Profile.js
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Як толькі ў вас атрымаецца зрабіць гэта з дапамогай аднаго са стыляў экспартавання, паспрабуйце зрабіць тое ж самае, выкарыстоўваючы іншы стыль.

<Solution>

Вось варыянт рашэння з найменным экспартаваннем:

<Sandpack>

```js App.js
import Gallery from './Gallery.js';
import { Profile } from './Profile.js';

export default function App() {
  return (
    <div>
      <Profile />
      <Gallery />
    </div>
  );
}
```

```js Gallery.js
import { Profile } from './Profile.js';

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
export function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

Вось варыянт рашэння з прадвызначаным экспартаваннем:

<Sandpack>

```js App.js
import Gallery from './Gallery.js';
import Profile from './Profile.js';

export default function App() {
  return (
    <div>
      <Profile />
      <Gallery />
    </div>
  );
}
```

```js Gallery.js
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
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

</Solution>

</Challenges>

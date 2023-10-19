---
title: Рэагаванне на падзеі
---

<Intro>

React дазваляе дадаваць *апрацоўшчыкі падзей* у JSX. Апрацоўшчыкі падзей — гэта вашыя ўласныя функцыі, якія будуць запускацца ў адказ на ўзаемадзеянне карыстальніка, напрыклад, націсканне кнопак, навядзенне курсора на элементы, атрыманне фокусу на ўводах формы і гэтак далей.

</Intro>

<YouWillLearn>

* Розныя спосабы напісання апрацоўшчыкаў падзей
* Як перадаць логіку апрацоўкі падзей ад бацькоўскіх кампанентаў
* Як падзеі распаўсюджваюцца і як іх спыніць

</YouWillLearn>

## Дадаванне апрацоўшчыкаў падзей {/*adding-event-handlers*/}

Каб дадаць апрацоўшчык падзеі, спачатку трэба вызначыце функцыю, а потым [перадаць яе ў якасці пропса](/learn/passing-props-to-a-component) ў адпаведны JSX тэг. Напрыклад, вось кнопка, якая пакуль нічога не робіць:

<Sandpack>

```js
export default function Button() {
  return (
    <button>
      Я нічога не раблю
    </button>
  );
}
```

</Sandpack>

Для таго, каб пры націсканні на кнопку паказвалася паведамленне, выканайце наступныя тры крокі:

1. Аб'явіце функцыю `handleClick` *унутры* кампанента `Button`.
2. Рэалізуйце логіку ўнутры гэтай функцыі (выкарыстоўвайце `alert`, каб паказаць паведамленне).
3. Дадайце `onClick={handleClick}` у JSX тэг `<button>`.

<Sandpack>

```js
export default function Button() {
  function handleClick() {
    alert('Вы націснулі на мяне!');
  }

  return (
    <button onClick={handleClick}>
      Націсніце на мяне
    </button>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

Вы аб'явілі функцыю `handleClick` і затым [перадалі яе ў якасці пропса](/learn/passing-props-to-a-component) ў `<button>`. Функцыя `handleClick` — гэта **апрацоўшчык падзей.** Функцыі апрацоўшчыкі падзей:

* Звычайна аб'яўляюцца *ўнутры* вашых кампанентаў.
* Маюць назвы, якія пачынаецца са слова `handle`, за якім ідзе назва падзеі.

Назвы апрацоўшчыкаў падзей звычайна пачынаюцца са слова `handle`, за якім ідзе назва падзеі. Вы часта будзеце сустракаць `onClick={handleClick}`, `onMouseEnter={handleMouseEnter}` і т.п.

У якасці альтэрнатывы, вы можаце аб'явіць апрацоўшчык падзей унутры JSX:

```jsx
<button onClick={function handleClick() {
  alert('Вы націснулі на мяне!');
}}>
```

Або, больш сцісла, выкарыстоўваючы стрэлачную функцыю:

```jsx
<button onClick={() => {
  alert('Вы націснулі на мяне!');
}}>
```

Абодва гэтыя варыянты эквівалентныя. Апрацоўшчыкі падзей унутры JSX зручныя для кароткіх функцый.

<Pitfall>

Функцыі, якія перадаюцца ў апрацоўшчыкі падзей, павінны перадавацца, а не выклікацца. Напрыклад:

| перадача функцыі (правільна)     | выклік функцыі (няправільны)     |
| -------------------------------- | ---------------------------------- |
| `<button onClick={handleClick}>` | `<button onClick={handleClick()}>` |

Розніца тут невялікая. У першым прыкладзе функцыя `handleClick` перадаецца як апрацоўшчык падзеі `onClick`. Гэта кажа React запомніць яе і выклікаць толькі тады, калі карыстальнік націскае кнопку.

У другім прыкладзе дужкі `()` у канцы `handleClick()` выклікаюць функцыю *адразу* падчас [рэндэрынгу](/learn/render-and-commit), без націскання кнопкі. Так адбываецца таму, што JavaScript код унутры [`{` і `}` JSX](/learn/javascript-in-jsx-with-curly-braces) выконваецца адразу.

Калі вы пішаце ўбудаваны (inline) код, тая ж пастка праяўляецца па-іншаму:

| перадача функцыі (правільна)            | выклік функцыі (няправільны)    |
| --------------------------------------- | --------------------------------- |
| `<button onClick={() => alert('...')}>` | `<button onClick={alert('...')}>` |

Калі мы перададзім убудаваны код наступным чынам, то ён не будзе выкананы пры націску кнопкі — ён будзе выконвацца пры кожным рэндэрынгу кампанента:

```jsx
// Гэта паведамленне з'яўляецца пры рэндэрынгу кампанента, а не пры націску кнопкі!
<button onClick={alert('Вы націснулі на мяне!')}>
```

Калі вы хочаце аб'явіць апрацоўшчык падзей убудаваным чынам, абгарніце яго ў ананімную функцыю, як паказана ніжэй:

```jsx
<button onClick={() => alert('Вы націснулі на мяне!')}>
```

Такім чынам, замест выканання кода пры кожным рэндэрынгу, ствараецца функцыя, якая будзе выклікана пазней.

У абодвух выпадках вам трэба перадаваць функцыю:

* `<button onClick={handleClick}>` перадае функцыю `handleClick`.
* `<button onClick={() => alert('...')}>` перадае стрэлачную функцыю `() => alert('...')`.

[Падрабязней пра стрэлачныя функцыі.](https://javascript.info/arrow-functions-basics)

</Pitfall>

### Чытанне пропсаў у апрацоўшчыках падзей {/*reading-props-in-event-handlers*/}

Паколькі апрацоўшчыкі падзей аб'яўлены ўнутры кампанента, яны маюць доступ да пропсаў кампанента. Вось кнопка, пры націсканні на якую паказваецца паведамленне са значэннем пропса `message`:

<Sandpack>

```js
function AlertButton({ message, children }) {
  return (
    <button onClick={() => alert(message)}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div>
      <AlertButton message="Прайграваецца!">
        Прайграць фільм
      </AlertButton>
      <AlertButton message="Запампоўваецца!">
        Запампаваць відарыс
      </AlertButton>
    </div>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

Дзякуючы гэтаму дзве кнопкі паказваюць розныя паведамленні. Паспрабуйце змяніць значэнні, якія ім перадаюцца.

### Перадача апрацоўшчыкаў падзей у якасці пропсаў {/*passing-event-handlers-as-props*/}

Часта вам можа спатрэбіцца, каб бацькоўскі кампанент вызначаў апрацоўшчыка падзей для даччынага кампанента. Разгледзім кнопкі: у залежнасці ад таго, дзе вы выкарыстоўваеце кампанент `Button`, вам можа спатрэбіцца выконваць розныя функцыі — магчыма, адна кнопка павінна прайграваць фільм, а другая загружаць відарыс.

Каб зрабіць гэта, перадайце кампаненту пропс, які ён атрымлівае ад свайго бацькі, у якасці апрацоўшчыка падзей, наступным чынам:

<Sandpack>

```js
function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

function PlayButton({ movieName }) {
  function handlePlayClick() {
    alert(`Прайграваецца ${movieName}!`);
  }

  return (
    <Button onClick={handlePlayClick}>
      Прайграць "{movieName}"
    </Button>
  );
}

function UploadButton() {
  return (
    <Button onClick={() => alert('Запампоўваецца!')}>
      Запампаваць відарыс
    </Button>
  );
}

export default function Toolbar() {
  return (
    <div>
      <PlayButton movieName="Ведзьміна дастаўка" />
      <UploadButton />
    </div>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

Тут кампанент `Toolbar` рэндэрыць кампаненты `PlayButton` і `UploadButton`:

- `PlayButton` перадае `handlePlayClick` у якасці пропса `onClick` у кампанент `Button`.
- `UploadButton` перадае `() => alert('Запампоўваецца!')` у якасці пропса `onClick` у кампанент `Button`.

Нарэшце, ваш кампанент `Button` прымае пропс пад назвай `onClick`. Ён перадае гэты пропс непасрэдна ва ўбудаваны браўзерны кампанент `<button>` з дапамогай `onClick={onClick}`. Гэта загадвае React выклікаць перададзеную функцыю пры націску.

Калі вы выкарыстоўваеце [дызайн-сістэму](https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969), то звычайна такія кампаненты, як кнопкі, змяшчаюць стылі, але не вызначаюць паводзіны. Замест гэтага кампаненты, як `PlayButton` і `UploadButton`, будуць перадаваць апрацоўшчыкі падзей уніз па іерархіі.

### Называнне пропсаў апрацоўшчыка падзей {/*naming-event-handler-props*/}

Убудаваныя кампаненты, такія як `<button>` і `<div>`, падтрымліваюць толькі [назвы падзей браўзера](/reference/react-dom/components/common#common-props), напрыклад `onClick`. Аднак, пры стварэнні ўласных кампанентаў, вы можаце называць іх апрацоўшчыкі падзей так, як вам падабаецца.

Назва пропса апрацоўшчыка падзей павінна пачынацца з `on`, за якім ідзе вялікая літара.

Напрыклад, пропс для падзеі `onClick` у кампаненце `Button` мог бы называцца `onSmash`:

<Sandpack>

```js
function Button({ onSmash, children }) {
  return (
    <button onClick={onSmash}>
      {children}
    </button>
  );
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert('Прайграваецца!')}>
        Прайграць фільм
      </Button>
      <Button onSmash={() => alert('Запампоўваецца!')}>
        Запампаваць відарыс
      </Button>
    </div>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

У гэтым прыкладзе `<button onClick={onSmash}>` паказвае, што браўзерны кампанент `<button>` (з маленькай літары) па-ранейшаму патрэбуе пропс з назвай `onClick`, але назву пропса, які атрымлівае ваш карыстальніцкі кампанентам `Button`, выбіраеце вы!

Калі ваш кампанент падтрымлівае некалькі ўзаемадзеянняў, вы можаце назваць пропсы апрацоўшчыкаў падзей у адпаведнасці з канкрэтнымі канцэпцыямі вашай праграмы. Напрыклад, кампанент `Toolbar` атрымлівае апрацоўшчыкі падзей `onPlayMovie` і `onUploadImage`:

<Sandpack>

```js
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Прайграваецца!')}
      onUploadImage={() => alert('Запампоўваецца!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Прайграць фільм
      </Button>
      <Button onClick={onUploadImage}>
        Запампаваць відарыс
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

Звярніце ўвагу, што кампанент `App` не павінен ведаць, *што* кампанент `Toolbar` будзе рабіць з `onPlayMovie` або `onUploadImage`. Гэта дэталі рэалізацыі `Toolbar`. У гэтым прыкладзе `Toolbar` перадае іх у якасці апрацоўшчыкаў падзеі `onClick` сваім кампанентам `Button`, але пазней ён таксама можа выклікаць іх па спалучэнні клавіш. Называнне пропсаў у адпаведнасці са спецыфікай вашай праграмы, напрыклад `onPlayMovie`, дае вам магчымасць змяніць спосаб іх выкарыстання ў будучыні.
  
<Note>

Упэўніцеся, што вы выкарыстоўваеце адпаведныя HTML тэгі для апрацоўшчыкаў падзей. Напрыклад, для апрацоўкі націскання выкарыстоўвайце [`<button onClick={handleClick}>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button) замест `<div onClick={handleClick}>`. Выкарыстанне сапраўднага браўзернага кампанента `<button>` дазваляе выкарыстоўваць убудаваныя функцыі браўзера, напрыклад навігацыю з клавіятуры. Калі вам не падабаецца прадвызначаны стыль кнопкі ў браўзеры і вы хочаце зрабіць яе больш падобнай на спасылку або іншы элемент інтэрфейсу, вы можаце дасягнуць гэтага з дапамогай CSS. [Даведайцеся больш пра напісанне даступнай разметкі.](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/HTML)
  
</Note>

## Распаўсюджванне падзей {/*event-propagation*/}

Апрацоўшчыкі падзей таксама будуць перахопліваць падзеі ад любых даччыных кампанентаў. Кажуць, што падзея «ўсплывае» або «распаўсюджваецца» ўверх па дрэве: яна пачынаецца там, дзе адбылася падзея, а потым ідзе ўверх па дрэве.

Гэты `<div>` змяшчае дзве кнопкі. У `<div>` *і* ў кожнай кнопкі ёсць уласныя апрацоўшчыкі падзеі `onClick`. Як думаеце, якія апрацоўшчыкі будуць выкліканы, калі вы націснеце кнопку?

<Sandpack>

```js
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('Вы націснулі на панэль інструментаў!');
    }}>
      <button onClick={() => alert('Прайграваецца!')}>
        Прайграць фільм
      </button>
      <button onClick={() => alert('Запампоўваецца!')}>
        Запампаваць відарыс
      </button>
    </div>
  );
}
```

```css
.Toolbar {
  background: #aaa;
  padding: 5px;
}
button { margin: 5px; }
```

</Sandpack>

Калі вы націснеце на любую кнопку, то спачатку будзе выкліканы яе апрацоўшчык `onClick`, а пасля апрацоўшчык `onClick` бацькоўскага элемента `<div>`. Такім чынам, з'явяцца два паведамленні. Калі вы націснеце на саму панэль інструментаў, то будзе выкліканы толькі апрацоўшчык `onClick` бацькоўскага элемента `<div>`.

<Pitfall>

У React усе падзеі распаўсюджваюцца, за выключэннем падзеі `onScroll`, якая працуе толькі ў JSX тэгу, да якога яна прывязана.

</Pitfall>

### Спыненне распаўсюджвання падзей {/*stopping-propagation*/}

Апрацоўшчыкі падзей атрымліваюць **аб'ект падзеі** ў якасці адзінага аргумента. Звычайна гэты аб'ект называецца `e`, што расшыфроўваецца як «event» (падзея). З дапамогай гэтага аб'екта вы можаце атрымаць інфармацыю аб падзеі.

Аб'ект падзеі таксама дазваляе спыніць распаўсюджванне падзеі. Калі вы не хочаце, каб падзея дасягнула бацькоўскіх кампанентаў, вам трэба выклікаць `e.stopPropagation()`, як гэта робіць кампанент `Button`:

<Sandpack>

```js
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('Вы націснулі на панэль інструментаў!');
    }}>
      <Button onClick={() => alert('Прайграваецца!')}>
        Прайграць фільм
      </Button>
      <Button onClick={() => alert('Запампоўваецца!')}>
        Запампаваць відарыс
      </Button>
    </div>
  );
}
```

```css
.Toolbar {
  background: #aaa;
  padding: 5px;
}
button { margin: 5px; }
```

</Sandpack>

Пры націсканні на кнопку:

1. React выклікае апрацоўшчык `onClick`, перададзены ў `<button>`. 
2. Гэты апрацоўшчык, аб'яўлены ў `Button`, выконвае наступнае:
   * Выклікае `e.stopPropagation()`, што прадухіляе далейшае ўсплыванне падзеі.
   * Выклікае функцыю `onClick`, якая з'яўляецца пропсам, які перадалі з кампанента `Toolbar`.
3. Гэтая функцыя, аб'яўленая ў кампаненце `Toolbar`, паказвае ўсплывальнае акно `alert` для кнопкі.
4. Паколькі распаўсюджванне падзеі было спынена, апрацоўшчык `onClick` бацькоўскага элемента `<div>` *не* выклікаецца.

У выніку `e.stopPropagation()`, пры націсканні на кнопкі цяпер паказваецца толькі адно ўсплывальнае акно (ад `<button>`), а не два (ад `<button>` і ад бацькоўскага `<div>` у кампаненце `Toolbar`). Націсканне кнопкі — гэта не тое ж самае, што націсканне на панэль інструментаў, таму спыненне распаўсюджвання падзеі мае сэнс для гэтага інтэрфейсу.

<DeepDive>

#### Перахопліванне падзей {/*capture-phase-events*/}

У рэдкіх выпадках вам можа спатрэбіцца перахапіць усе падзеі на даччыных элементах, *нават калі яны спынілі распаўсюджванне*. Напрыклад, магчыма, вы хочаце зарэгістраваць кожны націск мышы ў аналітыцы, незалежна ад логікі распаўсюджвання. Для гэтага можна дадаць `Capture` у канец назвы падзеі:

```js
<div onClickCapture={() => { /* гэта адбываецца ў першую чаргу */ }}>
  <button onClick={e => e.stopPropagation()} />
  <button onClick={e => e.stopPropagation()} />
</div>
```

Кожная падзея распаўсюджваецца ў тры фазы:

1. Яна спускаецца ўніз, выклікаючы ўсе апрацоўшчыкі `onClickCapture`.
2. Яна выклікае апрацоўшчык `onClick` для націснутага элемента.
3. Яна падымаецца ўверх, выклікаючы ўсе апрацоўшчыкі `onClick`.

Перахопліванне падзей карысны пры напісанні маршрутызатараў або аналітыцы, але вы, верагодна, не будзеце выкарыстоўваць іх у сваёй праграме.

</DeepDive>

### Перадача апрацоўшчыкаў як альтэрнатыва распаўсюджванню падзей {/*passing-handlers-as-alternative-to-propagation*/}

Звярніце ўвагу, як гэты апрацоўшчык націскання кнопкі выконвае радок кода, _а потым_ выклікае перададзены бацькам пропс `onClick`:

```js {4,5}
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```

Вы таксама можаце дадаць больш кода ў гэты апрацоўшчык перад тым, як выклікаць бацькоўскі апрацоўшчык падзеі `onClick`. Гэты шаблон забяспечвае *альтэрнатыву* распаўсюджванню падзеі. Ён дазваляе даччынаму кампаненту апрацоўваць падзею, а таксама дазваляе бацькоўскаму кампаненту вызначаць дадатковыя паводзіны. У адрозненне ад распаўсюджвання, гэта не адбываецца аўтаматычна. Але перавага гэтага шаблону ў тым, што вы можаце дакладна прасачыць увесь ланцужок кода, які выконваецца ў выніку нейкай падзеі.

Калі вы разлічваеце на распаўсюджванне падзей і вам цяжка сачыць за тым, якія апрацоўшчыкі выконваюцца і чаму, паспрабуйце выкарыстоўваць гэты падыход.

### Прадухіленне прадвызначаных паводзін {/*preventing-default-behavior*/}

Некаторыя браўзеры маюць прадвызначаныя паводзіны для некаторых падзей. Напрыклад, падзея адпраўкі формы `<form>`, якая адбываецца пры націсканні кнопкі ўнутры яе, прадвызначана перазагружае ўсю старонку:

<Sandpack>

```js
export default function Signup() {
  return (
    <form onSubmit={() => alert('Адпраўка!')}>
      <input />
      <button>Адправіць</button>
    </form>
  );
}
```

```css
button { margin-left: 5px; }
```

</Sandpack>

Вы можаце выклікаць `e.preventDefault()` для аб'екта падзеі, каб прадухіліць гэта:

<Sandpack>

```js
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Адпраўка!');
    }}>
      <input />
      <button>Адправіць</button>
    </form>
  );
}
```

```css
button { margin-left: 5px; }
```

</Sandpack>

Не блытайце функцыі `e.stopPropagation()` і `e.preventDefault()`. Яны абедзве карысныя, але не звязаны паміж сабой:

* [`e.stopPropagation()`](https://developer.mozilla.org/docs/Web/API/Event/stopPropagation) спыняе спрацоўванне апрацоўшчыкаў падзей, прывязаных да тэгаў вышэй па іерархіі DOM.
* [`e.preventDefault()` ](https://developer.mozilla.org/docs/Web/API/Event/preventDefault) прадухіляе прадвызначаныя паводзіны браўзера для некаторых падзей.

## Ці могуць апрацоўшчыкі падзей мець пабочныя эфекты? {/*can-event-handlers-have-side-effects*/}

Вядома! Апрацоўшчыкі падзей — гэта лепшае месца для пабочных эфектаў.

У адрозненне ад функцый рэндэрынгу, апрацоўшчыкі падзей не павінны быць [чыстымі функцыямі](/learn/keeping-components-pure), таму гэта выдатнае месца, каб *змяніць* што-небудзь — напрыклад, змяніць значэнне ўводу ў адказ на набор тэксту або змяніць спіс у адказ на націск кнопкі. Аднак, каб змяніць нейкую інфармацыю, спачатку яе трэба неяк захаваць. У React для гэтага выкарыстоўваецца [стан — памяць кампанента.](/learn/state-a-components-memory) Пра ўсё гэта вы даведаецеся на наступнай старонцы.

<Recap>

* Вы можаце апрацоўваць падзеі, перадаючы функцыю ў якасці пропса элементу, напрыклад `<button>`.
* Апрацоўшчыкі падзей павінны перадавацца, а **не выклікацца!** Трэба `onClick={handleClick}`, а не `onClick={handleClick()}`.
* Вы можаце аб'явіць функцыю апрацоўшчыка падзей асобна або ў самім тэгу (убудаваным чынам).
* Апрацоўшчыкі падзей аб'яўляюцца ўнутры кампанента, таму яны маюць доступ да пропсаў.
* Вы можаце аб'явіць апрацоўшчык падзей у бацькоўскім кампаненце і перадаць яго ў якасці пропса даччынаму кампаненту.
* Вы можаце аб'яўляць свае ўласныя пропсы апрацоўшчыка падзей з назвамі, спецыфічнымі для вашай праграмы.
* Падзеі распаўсюджваюцца ўверх. Каб прадухіліць гэта, выклічце `e.stopPropagation()`.
* Падзеі могуць мець непажаданыя прадвызначаныя паводзіны браўзера. Каб прадухіліць гэта, выклічце `e.preventDefault()`.
* Яўны выклік апрацоўшчыка падзей з даччынага апрацоўшчыка з'яўляецца добрай альтэрнатывай распаўсюджванню падзеі.

</Recap>



<Challenges>

#### Выправіце апрацоўшчык падзей {/*fix-an-event-handler*/}

Пры націсканні гэтай кнопкі фон старонкі павінен пераключацца паміж белым і чорным. Аднак, нічога не адбываецца пры націсканні на яе. Выправіце гэту праблему. (Не хвалюйцеся наконт логікі ўнутры `handleClick` — яна ў парадку.)

<Sandpack>

```js
export default function LightSwitch() {
  function handleClick() {
    let bodyStyle = document.body.style;
    if (bodyStyle.backgroundColor === 'black') {
      bodyStyle.backgroundColor = 'white';
    } else {
      bodyStyle.backgroundColor = 'black';
    }
  }

  return (
    <button onClick={handleClick()}>
      Пераключыць тэму
    </button>
  );
}
```

</Sandpack>

<Solution>

Праблема заключаецца ў тым, што `<button onClick={handleClick()}>` _выклікае_ функцыю `handleClick` падчас рэндэрынгу, а не _перадае_ яе. Выдаленне выкліку `()` так, каб яно стала выглядаць наступным чынам `<button onClick={handleClick}>` вырашыць праблему:

<Sandpack>

```js
export default function LightSwitch() {
  function handleClick() {
    let bodyStyle = document.body.style;
    if (bodyStyle.backgroundColor === 'black') {
      bodyStyle.backgroundColor = 'white';
    } else {
      bodyStyle.backgroundColor = 'black';
    }
  }

  return (
    <button onClick={handleClick}>
      Пераключыць тэму
    </button>
  );
}
```

</Sandpack>

У якасці альтэрнатывы вы можаце абгарнуць выклік у іншую функцыю, напрыклад, `<button onClick={() => handleClick()}>`:

<Sandpack>

```js
export default function LightSwitch() {
  function handleClick() {
    let bodyStyle = document.body.style;
    if (bodyStyle.backgroundColor === 'black') {
      bodyStyle.backgroundColor = 'white';
    } else {
      bodyStyle.backgroundColor = 'black';
    }
  }

  return (
    <button onClick={() => handleClick()}>
      Пераключыць тэму
    </button>
  );
}
```

</Sandpack>

</Solution>

#### Падключыце апрацоўшчыкі падзей {/*wire-up-the-events*/}

Кампанент `ColorSwitch` адлюстроўвае кнопку, якая павінна змяняць колер старонкі. Падключыце кнопку да апрацоўшчыка падзей `onChangeColor`, які кампанент атрымлівае ад бацькоўскага кампанента ў якасці пропса, такім чынам, каб націск на кнопку змяніў колер старонкі.

Пасля таго, як вы гэта зробіце, звярніце ўвагу, што націсканне кнопкі таксама павялічвае лічыльнік націскаў на старонцы. Ваш калега, які напісаў бацькоўскі кампанент, настойвае на тым, што `onChangeColor` не павялічвае ніякіх лічыльнікаў. Што яшчэ можа адбывацца? Выправіце гэта так, каб націск на кнопку *толькі* змяняў колер, а _не_ павялічваў лічыльнік.

<Sandpack>

```js ColorSwitch.js active
export default function ColorSwitch({
  onChangeColor
}) {
  return (
    <button>
      Змяніць колер
    </button>
  );
}
```

```js App.js hidden
import { useState } from 'react';
import ColorSwitch from './ColorSwitch.js';

export default function App() {
  const [clicks, setClicks] = useState(0);

  function handleClickOutside() {
    setClicks(c => c + 1);
  }

  function getRandomLightColor() {
    let r = 150 + Math.round(100 * Math.random());
    let g = 150 + Math.round(100 * Math.random());
    let b = 150 + Math.round(100 * Math.random());
    return `rgb(${r}, ${g}, ${b})`;
  }

  function handleChangeColor() {
    let bodyStyle = document.body.style;
    bodyStyle.backgroundColor = getRandomLightColor();
  }

  return (
    <div style={{ width: '100%', height: '100%' }} onClick={handleClickOutside}>
      <ColorSwitch onChangeColor={handleChangeColor} />
      <br />
      <br />
      <h2>Колькасць націскаў: {clicks}</h2>
    </div>
  );
}
```

</Sandpack>

<Solution>

Спачатку трэба дадаць апрацоўшчык падзеі, напрыклад, так: `<button onClick={onChangeColor}>`.

Аднак пры гэтым узнікае праблема з павелічэннем лічыльніка пры націску на кнопку. Калі `onChangeColor`, як кажа ваш калега, не робіць гэтага, тады праблема заключаецца ў тым, што гэтая падзея распаўсюджваецца, «ўсплывае» ўверх, і нейкі апрацоўшчык падзей вышэй змяняе лічыльнік. Каб вырашыць гэтую праблему, трэба спыніць распаўсюджванне падзеі. Але не забывайце, што вы ўсё роўна павінны выклікаць `onChangeColor`.

<Sandpack>

```js ColorSwitch.js active
export default function ColorSwitch({
  onChangeColor
}) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onChangeColor();
    }}>
      Змяніць колер
    </button>
  );
}
```

```js App.js hidden
import { useState } from 'react';
import ColorSwitch from './ColorSwitch.js';

export default function App() {
  const [clicks, setClicks] = useState(0);

  function handleClickOutside() {
    setClicks(c => c + 1);
  }

  function getRandomLightColor() {
    let r = 150 + Math.round(100 * Math.random());
    let g = 150 + Math.round(100 * Math.random());
    let b = 150 + Math.round(100 * Math.random());
    return `rgb(${r}, ${g}, ${b})`;
  }

  function handleChangeColor() {
    let bodyStyle = document.body.style;
    bodyStyle.backgroundColor = getRandomLightColor();
  }

  return (
    <div style={{ width: '100%', height: '100%' }} onClick={handleClickOutside}>
      <ColorSwitch onChangeColor={handleChangeColor} />
      <br />
      <br />
      <h2>Колькасць націскаў: {clicks}</h2>
    </div>
  );
}
```

</Sandpack>

</Solution>

</Challenges>

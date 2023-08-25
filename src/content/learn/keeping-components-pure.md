---
title: Захоўванне кампанентаў чыстымі
---

<Intro>

Некаторыя JavaScript функцыі з'яўляюцца *чыстымі.* Чыстыя функцыі выконваюць толькі вылічэнне і больш нічога. Калі вы пільна сочыце за тым, каб вашыя кампаненты заставаліся чыстымі функцыямі, тады вы зможаце пазбегнуць цэлага класа незразумелых памылак і непрадказальных паводзін па меры росту вашай кодавай базы. Аднак, каб атрымаць гэтыя перавагі, трэба прытрымлівацца некалькіх правіл.

</Intro>

<YouWillLearn>

* Што такое чысціня і як яна дапамагае пазбегнуць памылак
* Як захоўваць кампаненты чыстымі, не дапускаючы змяненняў на этапе рэндэрынгу
* Як выкарыстоўваць строгі рэжым (Strict Mode) для пошуку памылак у кампанентах

</YouWillLearn>

## Чысціня: кампаненты як формулы {/*purity-components-as-formulas*/}

У інфарматыцы (і асабліва ў свеце функцыянальнага праграмавання) [чыстыя функцыі](https://wikipedia.org/wiki/Pure_function) — гэта функцыі з наступнымі характарыстыкамі:

* **Займаюцца сваёй справай.** Яны не змяняюць аб’екты або пераменныя, якія існавалі да выкліку функцыі.
* **Вяртаюць прадказальны вынік.** Пры аднолькавых уваходных даных чыстая функцыя заўсёды вяртае аднолькавы вынік.

Магчыма, вы ўжо знаёмыя з адным прыкладам чыстых функцый: матэматычнымі формуламі.

Разгледзім наступную матэматычную формулу: <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math>.

Калі <Math><MathI>x</MathI> = 2</Math>, тады <Math><MathI>y</MathI> = 4</Math>. Заўсёды.

Калі <Math><MathI>x</MathI> = 3</Math>, тады <Math><MathI>y</MathI> = 6</Math>. Заўсёды. 

Калі <Math><MathI>x</MathI> = 3</Math>, тады <MathI>y</MathI> не будзе часам роўным <Math>9</Math> або <Math>–1</Math> або <Math>2,5</Math> у залежнасці ад часу сутак або стану фондавага рынку.

Калі <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> і <Math><MathI>x</MathI> = 3</Math>, тады <MathI>y</MathI> _заўсёды_ будзе <Math>6</Math>. 

Калі б мы запісалі гэта як JavaScript функцыю, яна б выглядала наступным чынам:

```js
function double(number) {
  return 2 * number;
}
```

У прыкладзе вышэй `double` — **чыстая функцыя.** Калі вы перадасце ёй `3`, яна верне `6`. Заўсёды.

React пабудаваны вакол гэтай канцэпцыі. **React мяркуе, што кожны напісаны вамі кампанент з'яўляецца чыстай функцыяй.** Гэта азначае, што вашыя кампаненты React павінны для аднолькавага ўводу заўсёды вяртаць адзін і той жа JSX:

<Sandpack>

```js App.js
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Закіпяціце {drinkers} шклянкі(-ак) вады.</li>
      <li>Дадайце {drinkers} лыжку(-ак) чаю і {0,5 * drinkers} лыжку(-ак) спецый.</li>
      <li>Дадайце {0,5 * drinkers} шклянкі(-ак) кіпячонага малака і цукар па гусце.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Рэцэпт чаю са спецыямі</h1>
      <h2>Для дваіх</h2>
      <Recipe drinkers={2} />
      <h2>Для кампаніі</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

</Sandpack>

Калі вы перадаяце `drinkers={2}` у кампанент `Recipe`, ён верне JSX, які змяшчае `2 шклянкі(-ак) вады`. Заўсёды.

Калі вы перадаяце `drinkers={4}` — ён верне JSX, які змяшчае `4 шклянкі(-ак) вады`. Заўсёды.

Гэтак жа, як матэматычная формула.

Вы можаце разглядаць свой кампанент як рэцэпт: калі яго прытрымлівацца і не ўводзіць новыя інгрэдыенты ў працэсе гатавання, то кожны раз атрымаецца адна і тая ж страва. «Страва» — гэта JSX, які кампанент вяртае React для [рэндэру.](/learn/render-and-commit)

<Illustration src="/images/docs/illustrations/i_puritea-recipe.png" alt="Рэцэпт чаю для х чалавек: вазьміце х шклянак вады, дадайце х лыжак гарбаты і 0,5х лыжкі спецый і 0,5х шклянкі малака" />

## Пабочныя эфекты: (не)прадбачаныя наступствы {/*side-effects-unintended-consequences*/}

Працэс рэндэрынгу ў React заўсёды павінен быць чыстым. Кампаненты павінны толькі *вяртаць* свой JSX, а не *змяняць* якія-небудзь аб'екты або пераменныя, якія існавалі да рэндэрынгу — гэта зробіць кампаненты нячыстымі!

Вось кампанент, які парушае гэтае правіла:

<Sandpack>

```js
let guest = 0;

function Cup() {
  // Дрэнна: змяненне існуючай пераменнай!
  guest = guest + 1;
  return <h2>Кубак чаю для госця #{guest}</h2>;
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

Гэты кампанент чытае і запісвае пераменную `guest`, абвешчаную па-за яго межамі. Гэта азначае, што **выклікаўшы гэты кампанент некалькі разоў — вы кожны раз атрымаеце розны JSX!** І больш за тое, калі _іншыя_ кампаненты чытаюць пераменную `guest`, яны таксама будуць ствараць розны JSX, у залежнасці ад таго, калі адбываўся рэндэрынг! Гэта непрадказальна.

Вяртаючыся да нашай формулы <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math>, цяпер нават калі <Math><MathI>x</MathI> = 2</Math>, мы больш не можам верыць таму, што <Math><MathI>y</MathI> = 4</Math>. Нашы тэсты могуць перастаць працаваць, нашы карыстальнікі могуць быць збянтэжаны, самалёты могуць пачаць падаць з неба - вы бачыце, як гэта можа прывесці да нечаканых памылак!

Вы можаце выправіць гэты кампанент, [перадаўшы `guest` у якасці пропса](/learn/passing-props-to-a-component):

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Кубак чаю для госця #{guest}</h2>;
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

Цяпер кампанент стаў чыстым, бо JSX, які ён вяртае, залежыць толькі ад пропса `guest`.

Наогул, вы не павінны разлічваць на тое, што вашы кампаненты будуць рэндэрыцца ў пэўным парадку. Не мае значэння, калі вы выклікаеце <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> да або пасля <Math><MathI>y</MathI> = 5<MathI>x</MathI></Math>: абедзве формулы будуць разлічвацца незалежна адна ад адной. Такім да чынам, кожны кампанент павінен «думаць сам за сябе» і не спрабаваць каардынаваць дзеянні з іншымі кампанентамі або залежаць ад іх падчас рэндэрынгу. Рэндэрынг падобны на школьны экзамен: кожны кампанент павінен вылічваць JSX самастойна!

<DeepDive>

#### Выяўленне нячыстых вылічэнняў з дапамогай StrictMode {/*detecting-impure-calculations-with-strict-mode*/}

Хоць вы, магчыма, яшчэ не выкарыстоўвалі іх усе, але ў React ёсць тры віды ўваходных даных, якія вы можаце чытаць падчас рэндэрынгу: [пропсы](/learn/passing-props-to-a-component), [стан](/learn/state-a-components-memory) і [кантэкст.](/learn/passing-data-deeply-with-context) Вы заўсёды павінны разглядаць іх як даныя толькі для чытання.

Калі вы хочаце нешта *змяніць* у адказ на ўвод карыстальніка, вам трэба [задаць стан](/learn/state-a-components-memory) замест змены пераменнай. Вы ніколі не павінны змяняць раней існуючыя пераменныя або аб'екты падчас рэндэрынгу кампанента.

React прапануе «строгі рэжым» (Strict Mode), у якім ён двойчы выклікае функцыю кожнага кампанента падчас распрацоўкі. **Выклікаючы функцыі кампанента двойчы, строгі рэжым дапамагае знайсці кампаненты, якія парушаюць правілы чысціні.**

Звярніце ўвагу, як у арыгінальным прыкладзе адлюстроўваліся надпісы «для госця #2», «для госця #4» і «для госця #6» замест «для госця #1», «для госця #2» і «для госця #3». Зыходная функцыя не была чыстай, таму яе падвойны выклік не даў чаканы вынік. Але выпраўленая чыстая функцыя заўсёды працуе, нават калі яна выклікаецца двойчы. **Чыстыя функцыі толькі робяць вылічэнні, таму двайны іх выклік нічога не зменіць**--гэтак жа, як двайны выклік функцыі `double(2)` не змяняе тое, што вяртаецца, і рашэнне ўраўнення <Math><MathI>y</MathI> = 2<MathI>x</MathI></Math> зробленае двойчы не змяняе значэнне <MathI>y</MathI>. Пры аднолькавых уваходных даных — аднолькавы вынік. Заўсёды.

Строгі рэжым не ўплывае на працоўнае асяроддзе, таму ён не запаволіць працу праграмы для вашых карыстальнікаў. Каб уключыць строгі рэжым, вы можаце абгарнуць каранёвы кампанент у `<React.StrictMode>`. Некаторыя фрэймворкі робяць гэта прадвызначана.

</DeepDive>

### Лакальная мутацыя: маленькі сакрэт вашага кампанента {/*local-mutation-your-components-little-secret*/}

У прыведзеным вышэй прыкладзе праблема была ў тым, што кампанент змяніў *існуючую* пераменную падчас рэндэрынгу. Гэта часта называюць **"мутацыяй"**, каб гучала крыху страшней.
Чыстыя функцыі не муціруюць пераменныя па-за вобласцю бачнасці функцыі або аб'екты, якія былі створаны перад выклікам — гэта робіць іх нячыстымі!

Аднак **цалкам нармальна змяняць пераменныя і аб'екты, якія вы *толькі што* стварылі падчас рэндэрынгу.** У гэтым прыкладзе мы ствараем масіў `[]`, прысвойваем яго пераменнай `cups`, а затым дадаем (з дапамогай метаду `push`) у яго тузін «кубкаў»:

<Sandpack>

```js
function Cup({ guest }) {
  return <h2>Кубак чаю для госця #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

</Sandpack>

Калі б пераменная `cups` або масіў `[]` былі створаны па-за функцыяй `TeaGathering`, то гэта была б вялікая праблема! Вы б змянялі *існуючы* аб'ект, дадаючы элементы ў гэты масіў.

Аднак, усё ў парадку, бо вы стварылі іх *падчас таго ж рэндэру*, унутры кампанента `TeaGathering`. Ніхто па-за межамі `TeaGathering` ніколі не даведаецца, што адбылося. Гэта называецца **"лакальная мутацыя"** — гэта як маленькі сакрэт вашага кампанента.

## Дзе вы _можаце_ выклікаць пабочныя эфекты {/*where-you-_can_-cause-side-effects*/}

У той час як функцыянальнае праграмаванне ў значнай ступені абапіраецца на чысціню, у нейкі момант дзесьці _штосьці_ павінна змяніцца. У гэтым і ёсць сэнс праграмавання! Гэтыя змены — абнаўленне экрана, запуск анімацыі, змяненне даных — называюцца **пабочнымі эфектамі.** Гэта з'явы, якія адбываюцца _«збоку»_, а не падчас рэндэру.

У React **пабочныя эфекты звычайна знаходзяцца ўнутры [апрацоўшчыкаў падзей.](/learn/responding-to-events)** Апрацоўшчыкі падзей — гэта функцыі, якія React запускае, калі вы выконваеце некаторыя дзеянні, напрыклад, націскаеце кнопку. Нягледзячы на тое, што апрацоўшчыкі падзей аб'яўляюцца *ўнутры* вашага кампанента, яны не запускаюцца *падчас* рэндэрынгу! **Такім чынам, апрацоўшчыкі падзей не павінны быць чыстымі.**

Калі вы перабралі ўсе іншыя варыянты і ўсё роўна не можаце знайсці правільны апрацоўшчык падзей для вашага пабочнага эфекту, вы можаце далучыць яго да вернутага JSX з дапамогай выкліку [`useEffect`](/reference/react/useEffect) у вашым кампаненце. З дапамогай `useEffect` мы кажам React выканаць пабочны эфект пазней, пасля рэндэрынгу, калі пабочныя эфекты дазволены. **Аднак гэты падыход павінен быць вашым апошнім сродкам.**

Калі гэта магчыма, спрабуйце апісаць сваю логіку толькі з дапамогай рэндэрынгу. Вы будзеце здзіўлены, наколькі далёка гэта можа вас завесці!

<DeepDive>

#### Чаму React клапоціцца пра чысціню? {/*why-does-react-care-about-purity*/}

Напісанне чыстых функцый патрабуе пэўнай звычкі і дысцыпліны. Але таксама адкрывае надзвычайныя магчымасці:

* Вашы кампаненты могуць працаваць у іншым асяроддзі — напрыклад, на серверы! Паколькі яны вяртаюць аднолькавы вынік для аднолькавых уваходных даных, адзін кампанент можа абслугоўваць мноства запытаў карыстальнікаў.
* Вы можаце палепшыць прадукцыйнасць, [прапусціўшы рэндэрынг](/reference/react/memo) кампанентаў, уваходныя даныя якіх не змяніліся. Чыстыя функцыі бяспечна кэшаваць, таму што яны заўсёды вяртаюць аднолькавыя вынікі.
* Калі некаторыя даныя змяняюцца падчас рэндэрынгу глыбокага дрэва кампанентаў, React можа перазапусціць рэндэрынг, не марнуючы час на завяршэнне састарэлага рэндэрынгу. Чысціня дазваляе бяспечна спыніць вылічэнне у любы момант.

Кожная новая функцыя React, якую мы ствараем, выкарыстоўвае перавагі чысціні. Ад выбаркі даных да анімацыі і прадукцыйнасці, захаванне кампанентаў чыстымі раскрывае моц парадыгмы React.

</DeepDive>

<Recap>

* Кампаненты павінены быць чыстымі, што азначае:
  * **Займаюцца сваёй справай.** Яны не змяняюць аб’екты або пераменныя, якія існавалі да рэндэрынгу.
  * **Вяртаюць прадказальны вынік.** Пры аднолькавых уваходных даных кампанент заўсёды вяртае аднолькавы JSX.
* Рэндэрынг можа адбыцца ў любы момант, таму кампаненты не павінны залежаць ад іх паслядоўнасці рэндэрынгу.
* Вы не павінны змяняць уваходныя даныя, якія вашы кампаненты выкарыстоўваюць для рэндэрынгу. Гэта могуць быць пропсы, стан або кантэкст. Каб абнавіць экран, [«задайце» (set) стан](/learn/state-a-components-memory) замест таго, каб муціраваць раней існуючыя аб'екты.
* Імкніцеся апісваць логіку вашага кампанента ў JSX, які вы вяртаеце. Калі вам трэба нешта змяніць, лепш гэта зрабіць у апрацоўшчыку падзей. У крайнім выпадку, карыстайцеся `useEffect`.
* Напісанне чыстых функцый патрабуе практыкі, але гэта раскрывае моц парадыгмы React.

</Recap>


  
<Challenges>

#### Выправіць зламаны гадзіннік {/*fix-a-broken-clock*/}

Гэты кампанент спрабуе задаць для `<h1>` CSS клас `"night"` у перыяд з поўначы да шасці гадзін раніцы і `"day"` ва ўвесь астатні час. Аднак гэта не працуе. Ці можаце вы выправіць гэты кампанент?

Вы можаце праверыць, ці працуе ваша рашэнне, часова змяніўшы часавы пояс на вашым камп'ютары. Калі бягучы час паміж поўначчу і шасцю раніцы, гадзіннік павінен мець інвертаваныя колеры!

<Hint>

Рэндэрынг — гэта *вылічэнне*, тут не трэба спрабаваць нешта "рабіць". Ці можаце вы апісаць тую ж ідэю інакш?

</Hint>

<Sandpack>

```js Clock.js active
export default function Clock({ time }) {
  let hours = time.getHours();
  if (hours >= 0 && hours <= 6) {
    document.getElementById('time').className = 'night';
  } else {
    document.getElementById('time').className = 'day';
  }
  return (
    <h1 id="time">
      {time.toLocaleTimeString()}
    </h1>
  );
}
```

```js App.js hidden
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  return (
    <Clock time={time} />
  );
}
```

```css
body > * {
  width: 100%;
  height: 100%;
}
.day {
  background: #fff;
  color: #222;
}
.night {
  background: #222;
  color: #fff;
}
```

</Sandpack>

<Solution>

Вы можаце выправіць гэты кампанент, вылічыўшы `className` і ўключыўшы яго ў вывад рэндэрынгу:

<Sandpack>

```js Clock.js active
export default function Clock({ time }) {
  let hours = time.getHours();
  let className;
  if (hours >= 0 && hours <= 6) {
    className = 'night';
  } else {
    className = 'day';
  }
  return (
    <h1 className={className}>
      {time.toLocaleTimeString()}
    </h1>
  );
}
```

```js App.js hidden
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  return (
    <Clock time={time} />
  );
}
```

```css
body > * {
  width: 100%;
  height: 100%;
}
.day {
  background: #fff;
  color: #222;
}
.night {
  background: #222;
  color: #fff;
}
```

</Sandpack>

У гэтым прыкладзе наогул не быў неабходны пабочны эфект (мадыфікацыя DOM). Вам трэба было толькі вярнуць JSX.

</Solution>

#### Выправіць зламаны профіль {/*fix-a-broken-profile*/}

Два кампаненты `Profile` рэндэрацца побач з рознымі данымі. Націсніце «Згарнуць» на першым профілі, а затым «Разгарнуць». Вы заўважыце, што ў абодвух кампанентах цяпер паказаны адны і тыя ж даныя. Гэта памылка.

Знайдзіце прычыну памылкі і выпраўце яе.

<Hint>

Код з памылкамі знаходзіцца ў файле `Profile.js`. Пераканайцеся, што вы прачыталі яго з верху да нізу!

</Hint>

<Sandpack>

```js Profile.js
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

let currentPerson;

export default function Profile({ person }) {
  currentPerson = person;
  return (
    <Panel>
      <Header />
      <Avatar />
    </Panel>
  )
}

function Header() {
  return <h1>{currentPerson.name}</h1>;
}

function Avatar() {
  return (
    <img
      className="avatar"
      src={getImageUrl(currentPerson)}
      alt={currentPerson.name}
      width={50}
      height={50}
    />
  );
}
```

```js Panel.js hidden
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Згарнуць' : 'Разгарнуць'}
      </button>
      {open && children}
    </section>
  );
}
```

```js App.js
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Субрахманьян Чандрасекар',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Крэола Кэтрын Джонсан',
      }} />
    </>
  )
}
```

```js utils.js hidden
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
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

</Sandpack>

<Solution>

Праблема ў тым, што кампанент `Profile` змяняе значэнне існуючай пераменнай `currentPerson`, а кампаненты `Header` і `Avatar` чытаюць яе. Гэта робіць *усіх трох з іх* нячыстымі і непрадказальнымі.

Каб выправіць памылку, выдаліце пераменную `currentPerson`. Замест гэтага перадайце ўсю інфармацыю з `Profile` ў `Header` і `Avatar` праз пропсы. Вам трэба будзе дадаць пропс `person` да абодвух кампанентаў і перадаць яго праз увесь ланцужок.

<Sandpack>

```js Profile.js active
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

export default function Profile({ person }) {
  return (
    <Panel>
      <Header person={person} />
      <Avatar person={person} />
    </Panel>
  )
}

function Header({ person }) {
  return <h1>{person.name}</h1>;
}

function Avatar({ person }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={50}
      height={50}
    />
  );
}
```

```js Panel.js hidden
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Згарнуць' : 'Разгарнуць'}
      </button>
      {open && children}
    </section>
  );
}
```

```js App.js
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Субрахманьян Чандрасекар',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Крэола Кэтрын Джонсан',
      }} />
    </>
  );
}
```

```js utils.js hidden
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
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

</Sandpack>

Памятайце, што React не гарантуе, што функцыі кампанента будуць выконвацца ў пэўным парадку, таму вы не можаце наладзіць камунікацыю паміж імі праз змену пераменных. Уся камунікацыя павінна адбывацца праз пропсы.

</Solution>

#### Выправіць зламаны латок з гісторыямі {/*fix-a-broken-story-tray*/}

Генеральны дырэктар вашай кампаніі просіць вас дадаць «гісторыі» ў вашу праграму анлайн-гадзіннік, і вы не можаце сказаць «не». Вы напісалі кампанент `StoryTray`, які прымае спіс гісторый (`stories`), за якім ідзе запаўняльнік «Стварыць гісторыю».

Вы рэалізавалі запаўняльнік «Стварыць гісторыю», дадаўшы яшчэ адну «фальшывую» гісторыю ў канец масіву `stories`, які вы атрымалі ў якасці пропса. Але чамусьці «Стварыць гісторыю» з'яўляецца некалькі разоў. Вырашыце праблему.

<Sandpack>

```js StoryTray.js active
export default function StoryTray({ stories }) {
  stories.push({
    id: 'create',
    label: 'Стварыць гісторыю'
  });

  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

```js App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

let initialStories = [
  {id: 0, label: "Гісторыя Анкіта" },
  {id: 1, label: "Гісторыя Тэйлара" },
];

export default function App() {
  let [stories, setStories] = useState([...initialStories])
  let time = useTime();

  // HACK: Прадухіленне вечнага росту памяці падчас чытання дакументаў.
  // Тут мы парушаем свае ўласныя правілы.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>It is {time.toLocaleTimeString()} now.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

```js sandbox.config.json hidden
{
  "hardReloadOnChange": true
}
```

</Sandpack>

<Solution>

Звярніце ўвагу, як кожны раз, калі гадзіннік абнаўляецца, «Стварыць гісторыю» дадаецца *двойчы*. Гэта служыць намёкам на тое, што мы маем мутацыю падчас рэндэрынгу — строгі рэжым двойчы выклікае кампаненты, каб зрабіць такія праблемы больш прыкметнымі.

Функцыя `StoryTray` не з'яўляецца чыстай. Выклікаючы метад `push` на атрыманым масіве `stories` (гэта пропс!), вы змяняеце аб'ект, які быў створаны *да таго*, як `StoryTray` пачаў рэндэрынг. Праз гэта кампанент становіцца ненадзейным і амаль непрадказальным.

Самае простае рашэнне — увогуле не чапаць масіў і рэндэрыць запаўняльнік «Стварыць гісторыю» асобна:

<Sandpack>

```js StoryTray.js active
export default function StoryTray({ stories }) {
  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
      <li>Стварыць гісторыю</li>
    </ul>
  );
}
```

```js App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

let initialStories = [
  {id: 0, label: "Гісторыя Анкіта" },
  {id: 1, label: "Гісторыя Тэйлара" },
];

export default function App() {
  let [stories, setStories] = useState([...initialStories])
  let time = useTime();

  // HACK: Прадухіленне вечнага росту памяці падчас чытання дакументаў.
  // Тут мы парушаем свае ўласныя правілы.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>It is {time.toLocaleTimeString()} now.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

</Sandpack>

У якасці альтэрнатывы вы можаце стварыць _новы_ масіў (шляхам капіравання існуючага), перш чым дадаць у яго элемент:

<Sandpack>

```js StoryTray.js active
export default function StoryTray({ stories }) {
  // Скапіруйце масіў!
  let storiesToDisplay = stories.slice();

  // Не ўплывае на зыходны масіў:
  storiesToDisplay.push({
    id: 'create',
    label: 'Create Story'
  });

  return (
    <ul>
      {storiesToDisplay.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

```js App.js hidden
import { useState, useEffect } from 'react';
import StoryTray from './StoryTray.js';

let initialStories = [
  {id: 0, label: "Гісторыя Анкіта" },
  {id: 1, label: "Гісторыя Тэйлара" },
];

export default function App() {
  let [stories, setStories] = useState([...initialStories])
  let time = useTime();

  // HACK: Прадухіленне вечнага росту памяці падчас чытання дакументаў.
  // Тут мы парушаем свае ўласныя правілы.
  if (stories.length > 100) {
    stories.length = 100;
  }

  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <h2>It is {time.toLocaleTimeString()} now.</h2>
      <StoryTray stories={stories} />
    </div>
  );
}

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}
```

```css
ul {
  margin: 0;
  list-style-type: none;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  margin-bottom: 20px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

</Sandpack>

Дзякуючы гэтаму мутацыя цяпер стала лакальнай, а функцыя рэндэрынга — чыстай. Тым не менш, вы ўсё яшчэ трэба быць асцярожнымі: напрыклад, калі вы паспрабуеце змяніць якія-небудзь з існуючых элементаў масіва, вам таксама трэба будзе кланаваць гэтыя элементы.

Карысна памятаць, якія аперацыі над масівамі змяняюць іх, а якія не. Напрыклад, метады `push`, `pop`, `reverse` і `sort` зменяць зыходны масіў, але метады `slice`, `filter` і `map` створаць новы.

</Solution>

</Challenges>

---
title: Даданне інтэрактыўнасці
---

<Intro>

Некаторыя рэчы на экране абнаўляюцца ў адказ на ўвод карыстальніка. Напрыклад, націснуўшы любы відарыс у галерэі відарысаў вы пераключаеце актыўны відарыс. У React даныя, якія змяняюцца з часам, называюцца *станам.* Вы можаце дадаць стан любому кампаненту і абнавіць яго калі вам трэба. У гэтай главе вы даведаецеся, як пісаць кампаненты, якія апрацоўваюць узаемадзеянне, абнаўляюць свой стан і змяняюць свой выгляд з цягам часу.

</Intro>

<YouWillLearn isChapter={true}>

* [Як апрацоўваць падзеі, ініцыяваныя карыстальнікам](/learn/responding-to-events)
* [Як прымусіць кампаненты «запамінаць» інфармацыю з дапамогай стану](/learn/state-a-components-memory)
* [Як React абнаўляе карыстальніцкі інтэрфейс у два этапы](/learn/render-and-commit)
* [Чаму стан не абнаўляецца адразу ж пасля яго змены](/learn/state-as-a-snapshot)
* [Як паставіць у чаргу некалькі абнаўленняў стану](/learn/queueing-a-series-of-state-updates)
* [Як абнавіць аб'ект захаваны ў стане](/learn/updating-objects-in-state)
* [Як абнавіць масіў захаваны ў стане](/learn/updating-arrays-in-state)

</YouWillLearn>

## Рэагаванне на падзеі {/*responding-to-events*/}

React дазваляе дадаваць *апрацоўшчыкаў падзей* у ваш JSX. Апрацоўшчыкі падзей — гэта вашыя ўласныя функцыі, якія будуць запускацца ў адказ на ўзаемадзеянне карыстальніка, напрыклад, націсканне кнопак, навядзенне курсора на элементы, атрыманне фокусу на ўводах формы і гэтак далей.

Убудаваныя кампаненты, такія як `<button>`, падтрымліваюць толькі ўбудаваныя падзеі браўзера, такія як `onClick`. Аднак, вы таксама можаце ствараць свае ўласныя кампаненты і даваць іх апрацоўшчыкам падзей любыя назвы, спецыфічныя для праграмы, на ваш густ.

<Sandpack>

```js
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Ідзе прайграванне!')}
      onUploadImage={() => alert('Ідзе запампоўванне!')}
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

<LearnMore path="/learn/responding-to-events">

Даведайцеся ў артыкуле **«[Рэагаванне на падзеі](/learn/responding-to-events)»** аб тым, як дадаваць апрацоўшчыкі падзей.

</LearnMore>

## Стан: памяць кампанента {/*state-a-components-memory*/}

У выніку ўзаемадзеяння кампаненты часта павінны змяніць тое, што знаходзіцца на экране. Увод у форму павінен абнавіць поле ўводу, націсканне кнопкі «далей» на каруселі відарысаў павінна змяніць відарыс, які адлюстроўваецца, націсканне кнопкі «купіць» кладзе прадукт у кошык. Кампанентам трэба «запамінаць» рознае: бягучае ўваходнае значэнне, бягучы відарыс, кошык для пакупак. У React такая спецыфічная для кампанентаў памяць называецца *стан.*

Вы можаце дадаць стан да кампанента з дапамогай хука [`useState`](/reference/react/useState). *Хукі* — гэта такія спецыяльныя функцыі, што дазваляюць вашым кампанентам выкарыстоўваць функцыі React (стан — адна з такіх функцый). Хук `useState` дазваляе аб'явіць пераменную стану. Ён прымае пачатковае значэнне стану і вяртае пару значэнняў: бягучае значэнне стану і функцыю для задавання стану, якая дазваляе абнаўляць яго значэнне.

```js
const [index, setIndex] = useState(0);
const [showMore, setShowMore] = useState(false);
```

Вось як галерэя відарысаў выкарыстоўвае і абнаўляе стан пры націсканні:

<Sandpack>

```js
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);
  const hasNext = index < sculptureList.length - 1;

  function handleNextClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Схаваць' : 'Паказаць'} падрабязнасці
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </>
  );
}
```

```js data.js
export const sculptureList = [{
  name: 'Даніна нейрахірургіі',
  artist: 'Марта Колвін Андрадэ',
  description: 'Хаця Колвін пераважна вядомая сваімі абстрактнымі тэмамі, якія спасылаюцца на да-іспанскія сімвалы, гэтая гіганцкая скульптура, даніна павагі нейрахірургіі, з\'яўляецца адным з яе самых вядомых твораў мастацтва.',
  url: 'https://i.imgur.com/Mx7dA2Y.jpg',
  alt: 'Бронзавая статуя дзвюх скрыжаваных рук, якія далікатна трымаюць чалавечы мозг на кончыках пальцаў.'
}, {
  name: 'Родавы Флораліс',
  artist: 'Эдуарда Каталана',
  description: 'Гэтая велізарная (75 футаў або 23 метры) серабрыстая кветка знаходзіцца ў Буэнас-Айрэсе. Яна спраектавана так, што можа закрываць пялёсткі ўвечары або пры моцным ветры і раскрываць іх раніцай.',
  url: 'https://i.imgur.com/ZF6s192m.jpg',
  alt: 'Гіганцкая металічная скульптура кветкі з люстэркавымі пялёсткамі і моцнымі тычынкамі.'
}, {
  name: 'Вечная прысутнасць',
  artist: 'Джон Вудра Вільсан',
  description: 'Уілсан быў вядомы сваёй заклапочанасцю тэмамі роўнасці, сацыяльнай справядлівасці, а таксама істотнымі і духоўнымі якасцямі чалавецтва. Гэтая масіўная (7 футаў або 2,13 метра) бронзавая статуя ўяўляе тое, што ён назваў "сімвалічнай прысутнасцю чарнаскурых, прасякнутай пачуццём універсальнай чалавечнасці".',
  url: 'https://i.imgur.com/aTtVpES.jpg',
  alt: 'Скульптура з выявай чалавечай галавы здаецца ўсюдыіснай і панурай. Яна выпраменьвае спакой і ціхамірнасць.'
}, {
  name: 'Мааі',
  artist: 'Невядомы мастак',
  description: 'На востраве Пасхі знаходзіцца 1000 мааі, або захаваных манументальных статуй, створаных раннім народам Рапа-Нуі, якія, на думку некаторых, прадстаўлялі абагаўлёных продкаў.',
  url: 'https://i.imgur.com/RCwLEoQm.jpg',
  alt: 'Тры манументальныя каменныя бюсты з непрапарцыянальна вялікімі галовамі і змрочнымі тварамі.'
}, {
  name: 'Блакітная нана',
  artist: 'Нікі дэ Сен-Фаль',
  description: 'Наны — трыумфуючыя істоты, сімвалы жаноцкасці і мацярынства. Першапачаткова Сен-Фаль выкарыстаў тканіну і знайшоў прадметы для Нан, а пазней увёў поліэстэр для дасягнення больш яркага эфекту.',
  url: 'https://i.imgur.com/Sd1AgUOm.jpg',
  alt: 'Вялікая мазаічная скульптура мудрагелістай танцуючай жаночай фігуры ў маляўнічым касцюме, якая выпраменьвае радасць.'
}, {
  name: 'Канчатковая форма',
  artist: 'Барбара Хепворт',
  description: 'Гэтая абстрактная бронзавая скульптура з\'яўляецца часткай серыі «Сям\'я чалавека», размешчанай у Ёркшырскім парку скульптур. Хепворт вырашыла не ствараць літаральныя ўяўленні аб свеце, а распрацавала абстрактныя формы, натхнёныя людзьмі і ландшафтамі.',
  url: 'https://i.imgur.com/2heNQDcm.jpg',
  alt: 'Высокая скульптура з трох пастаўленых адзін на аднаго элементаў, якія нагадваюць фігуру чалавека.'
}, {
  name: 'Кавалер',
  artist: 'Ламідзі Олонадэ Фэйкі',
  description: "Праца Фэйкі, які з'яўляецца разьбяром па дрэве ў чатцвёртым пакаленні, змяшала традыцыйныя і сучасныя тэмы ёруба.",
  url: 'https://i.imgur.com/wIdGuZwm.png',
  alt: 'Складаная драўляная скульптура воіна са сканцэнтраваным тварам на кані, упрыгожаная ўзорамі.'
}, {
  name: 'Вялікі пузы',
  artist: 'Аліна Шапачнікаў',
  description: "Шапачнікаў вядомая сваімі скульптурамі фрагментаванага цела як метафарай крохкасці і нясталасці маладосці і прыгажосці. Гэтая скульптура адлюстроўвае два вельмі рэалістычныя вялікія жываты, пакладзеныя адзін на аднаго, кожны вышынёй каля пяці футаў (1,5 м).",
  url: 'https://i.imgur.com/AlHTAdDm.jpg',
  alt: 'Скульптура нагадвае каскад зморшчын, зусім іншы, чым жываты ў класічных скульптурах.'
}, {
  name: 'Тэракотовая армія',
  artist: 'Невядомы мастак',
  description: 'Тэракотавая армія — гэта калекцыя тэракотавых скульптур, якія адлюстроўваюць армію Цынь Шы Хуандзі, першага імператара Кітая. Войска налічвала больш за 8 тысяч воінаў, 130 калясніц з 520 коньмі і 150 кавалерыйскіх коней.',
  url: 'https://i.imgur.com/HMFmH6m.jpg',
  alt: '12 тэракотавых скульптур урачыстых воінаў, кожная з унікальным выразам твару і даспехамі.'
}, {
  name: 'Месяцовы пейзаж',
  artist: 'Луіза Нэвельсан',
  description: 'Нэвельсан была вядомая тым, што ачышчала аб\'екты з руін Нью-Ёрка, якія пазней яна збярэ ў манументальныя канструкцыі. У гэтай статуі яна выкарыстала разрозненыя часткі, такія як ложак, шпільку для жангліравання і фрагмент сядзення, прыбіўшы і склеіўшы іх у скрынкі, якія адлюстроўваюць уплыў геаметрычнай абстракцыі прасторы і формы кубізму.',
  url: 'https://i.imgur.com/rN7hY6om.jpg',
  alt: 'Чорная матавая скульптура, дзе асобныя элементы першапачаткова неадрозныя.'
}, {
  name: 'Арэол',
  artist: 'Ранджані Шэтар',
  description: 'Shettar спалучае ў сабе традыцыйнае і сучаснае, натуральнае і індустрыяльнае. Яе мастацтва засяроджана на ўзаемаадносінах чалавека і прыроды. Яе праца была апісана як пераканаўчая як у абстрактным, так і ў вобразным сэнсе, якая кідае выклік гравітацыі і ўяўляе сабой «вытанчаны сінтэз малаверагодных матэрыялаў».',
  url: 'https://i.imgur.com/okTpbHhm.jpg',
  alt: 'Бледная скульптура, падобная на дрот, усталяваная на бетоннай сцяне і спускаецца на падлогу. Здаецца вельмі лёгкай.'
}, {
  name: 'Бегемоты',
  artist: 'Тайбэйскі заапарк',
  description: 'Заапарк Тайбэя заказаў плошчу бегемотаў, на якой гулялі пагружаныя ў ваду бегемоты.',
  url: 'https://i.imgur.com/6o5Vuyu.jpg',
  alt: 'Група бронзавых скульптур бегемотаў, якія выходзяць з тратуара, нібы яны плывуць.'
}];
```

```css
h2 { margin-top: 10px; margin-bottom: 0; }
h3 {
 margin-top: 5px;
 font-weight: normal;
 font-size: 100%;
}
img { width: 120px; height: 120px; }
button {
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

<LearnMore path="/learn/state-a-components-memory">

Даведайцеся ў артыкуле **«[Стан: памяць кампанента](/learn/state-a-components-memory)»** аб тым, як запамінаць значэнне і абнаўляць яго пры ўзаемадзеянні.

</LearnMore>

## Рэндэрынг і фіксацыя {/*render-and-commit*/}

Перш чым вашыя кампаненты будуць адлюстраваны на экране, React мусіць адрэндэрыць іх. Разуменне этапаў гэтага працэсу дапаможа вам зразумець, як выконваецца ваш код, і растлумачыць яго паводзіны.

Уявіце, што вашы кампаненты — гэта кухары на кухні, якія збіраюць смачныя стравы з інгрэдыентаў. У гэтым сцэнарыі React — гэта афіцыянт, які запісвае запыты ад кліентаў і пасля прыносіць ім іх заказы. Гэты працэс запыту і «падачы» карыстальніцкага інтэрфейсу складаецца з трох этапаў:

1. **Ініцыяванне** рэндэру (дастаўка заказу на кухню)
2. **Рэндэрынг** кампанента (гатаванне заказу на кухні)
3. **Фіксацыя** ў DOM (падаванне заказу на стол)

<IllustrationBlock sequential>
  <Illustration caption="Ініцыяванне" alt="React выконвае ролю афіцыянта ў рэстаране, які атрымлівае заказы ад карыстальніка і перадае іх кампаненту Kitchen." src="/images/docs/illustrations/i_render-and-commit1.png" />
  <Illustration caption="Рэндэрынг" alt="Шэф-повар дае React свежы кампанент Card." src="/images/docs/illustrations/i_render-and-commit2.png" />
  <Illustration caption="Фіксацыя" alt="React дастаўляе кампанент Card карыстальніку на яго столік." src="/images/docs/illustrations/i_render-and-commit3.png" />
</IllustrationBlock>

<LearnMore path="/learn/render-and-commit">

Даведайцеся ў артыкуле **«[Адлюстраванне і фіксацыя](/learn/render-and-commit)»** аб тым, як працуе жыццёвы цыкл абнаўлення карыстальніцкага інтэрфейсу.

</LearnMore>

## Стан як хуткі здымак {/*state-as-a-snapshot*/}

У адрозненне ад звычайных пераменных у JavaScript, стан у React паводзіць сябе больш як  хуткі здымак (snapshot). Заданне новага значэння стану не змяняе саму пераменную стану, якая ў вас ужо ёсць, а запускае паўторны рэндэрынг. Гэта можа здзівіць спачатку!

```js
console.log(count);  // 0
setCount(count + 1); // Запытвае паўторны рэндэрынг са значэннем 1
console.log(count);  // Усё яшчэ 0!
```

Такія паводзіны дапамагаюць пазбегнуць няўлоўных памылак. Вось невялікая праграма-чат. Паспрабуйце здагадацца, што адбудзецца, калі вы спачатку націснеце "Адправіць", а *затым* зменіце атрымальніка на Боба. Чыё імя з'явіцца ў акне `alert` праз пяць секунд?

<Sandpack>

```js
import { useState } from 'react';

export default function Form() {
  const [to, setTo] = useState('Аліса');
  const [message, setMessage] = useState('Вітаю');

  function handleSubmit(e) {
    e.preventDefault();
    setTimeout(() => {
      alert(`Вы сказалі ${to}: ${message}`);
    }, 5000);
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        To:{' '}
        <select
          value={to}
          onChange={e => setTo(e.target.value)}>
          <option value="Аліса">Аліса</option>
          <option value="Боб">Боб</option>
        </select>
      </label>
      <textarea
        placeholder="Паведамленне"
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <button type="submit">Адправіць</button>
    </form>
  );
}
```

```css
label, textarea { margin-bottom: 10px; display: block; }
```

</Sandpack>


<LearnMore path="/learn/state-as-a-snapshot">

Даведайцеся ў артыкуле **«[Стан як хуткі здымак](/learn/state-as-a-snapshot)»** аб тым, чаму стан здаецца "фіксаваным" і нязменным у апрацоўшчыках падзей.

</LearnMore>

## Чарга абнаўленняў стану {/*queueing-a-series-of-state-updates*/}

Гэты кампанент не правільна працуе: націсканне кнопкі «+3» павялічвае бал толькі адзін раз.

<Sandpack>

```js
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(score + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Балы: {score}</h1>
    </>
  )
}
```

```css
button { display: inline-block; margin: 10px; font-size: 20px; }
```

</Sandpack>

Артыкул «[Стан як хуткі здымак](/learn/state-as-a-snapshot)» тлумачыць, чаму так адбываецца. Задаванне новага значэння стану запытвае новы рэндэрынг, але не змяняе існуючае значэнне стану ва ўжо запушчаным кодзе. Таму пераменная `score` працягвае быць роўнай `0` адразу пасля таго, як вы выклікаеце `setScore(score + 1)`.

```js
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
```

Вы можаце выправіць гэта, перадаўшы *функцыю абнаўлення* пры ўсталяванні стану. Звярніце ўвагу, як замена `setScore(score + 1)` на `setScore(s => s + 1)` выпраўляе паводзіны кнопкі «+3». Гэта дазваляе вам ставіць у чаргу некалькі абнаўленняў стану.

<Sandpack>

```js
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(s => s + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Балы: {score}</h1>
    </>
  )
}
```

```css
button { display: inline-block; margin: 10px; font-size: 20px; }
```

</Sandpack>

<LearnMore path="/learn/queueing-a-series-of-state-updates">

Даведайцеся ў артыкуле **«[Чарга абнаўленняў стану](/learn/queueing-a-series-of-state-updates)»** аб тым, як паставіць у чаргу некалькі абнаўленняў стану.

</LearnMore>

## Абнаўленне аб'ектаў у стане {/*updating-objects-in-state*/}

Стан можа ўтрымліваць любы тып значэння JavaScript, у тым ліку аб'екты. Але вы не мусіце змяняць напрамую аб'екты і масівы, якія вы захоўваеце ў стане React. Замест гэтага, калі вы хочаце абнавіць аб'ект і масіў, вам трэба стварыць новы (або зрабіць копію існуючага), а затым абнавіць стан так, каб ён пачаў выкарыстоўваць гэтую копію.

Звычайна для капіравання аб'ектаў і масіваў, якія вы хочаце змяніць, вы будзеце выкарыстоўваць сінтаксіс пашырэння (spread syntax) `...`. Напрыклад, абнаўленне ўкладзенага аб'екта можа выглядаць так:

<Sandpack>

```js
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    name: 'Нікі дэ Сен-Фаль',
    artwork: {
      title: 'Блакітная нана',
      city: 'Гамбург',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    setPerson({
      ...person,
      name: e.target.value
    });
  }

  function handleTitleChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value
      }
    });
  }

  function handleCityChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value
      }
    });
  }

  function handleImageChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        image: e.target.value
      }
    });
  }

  return (
    <>
      <label>
        Імя:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Назва:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        Горад:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Відарыс:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' '}
        {person.name}
        <br />
        (размешчана ў горадзе {person.artwork.city})
      </p>
      <img
        src={person.artwork.image}
        alt={person.artwork.title}
      />
    </>
  );
}
```

```css
label { display: block; }
input { margin-left: 5px; margin-bottom: 5px; }
img { width: 200px; height: 200px; }
```

</Sandpack>

Калі капіраванне аб'ектаў у кодзе становіцца нудным, вы можаце выкарыстоўваць бібліятэку [Immer](https://github.com/immerjs/use-immer), каб паменшыць колькасць кода, што паўтараецца:

<Sandpack>

```js
import { useImmer } from 'use-immer';

export default function Form() {
  const [person, updatePerson] = useImmer({
    name: 'Нікі дэ Сен-Фаль',
    artwork: {
      title: 'Блакітная нана',
      city: 'Гамбург',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    updatePerson(draft => {
      draft.name = e.target.value;
    });
  }

  function handleTitleChange(e) {
    updatePerson(draft => {
      draft.artwork.title = e.target.value;
    });
  }

  function handleCityChange(e) {
    updatePerson(draft => {
      draft.artwork.city = e.target.value;
    });
  }

  function handleImageChange(e) {
    updatePerson(draft => {
      draft.artwork.image = e.target.value;
    });
  }

  return (
    <>
      <label>
        Імя:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Назва:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        Горад:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Відарыс:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' '}
        {person.name}
        <br />
        (размешчана ў горадзе {person.artwork.city})
      </p>
      <img
        src={person.artwork.image}
        alt={person.artwork.title}
      />
    </>
  );
}
```

```json package.json
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "use-immer": "0.5.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

```css
label { display: block; }
input { margin-left: 5px; margin-bottom: 5px; }
img { width: 200px; height: 200px; }
```

</Sandpack>

<LearnMore path="/learn/updating-objects-in-state">

Даведайцеся ў артыкуле **«[Абнаўленне аб'ектаў у стане](/learn/updating-objects-in-state)»** аб тым, як правільна абнаўляць аб'екты.

</LearnMore>

## Абнаўленне масіваў у стане {/*updating-arrays-in-state*/}

Масівы — гэта яшчэ адзін тып зменлівых аб'ектаў JavaScript, які можна захоўваць у стане і які мусіць разглядацца як значэнне толькі для чытання. Як і ў выпадку з аб'ектамі, калі вы хочаце абнавіць масіў, які захоўваецца ў стане, вам трэба стварыць новы (або зрабіць копію існуючага), а затым абнавіць стан так, каб ён пачаў выкарыстоўваць новы масіў:

<Sandpack>

```js
import { useState } from 'react';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Вялікі пузы', seen: false },
  { id: 1, title: 'Месяцовы пейзаж', seen: false },
  { id: 2, title: 'Тэракотовая армія', seen: true },
];

export default function BucketList() {
  const [list, setList] = useState(
    initialList
  );

  function handleToggle(artworkId, nextSeen) {
    setList(list.map(artwork => {
      if (artwork.id === artworkId) {
        return { ...artwork, seen: nextSeen };
      } else {
        return artwork;
      }
    }));
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>Мой спіс мастацтва для прагляду:</h2>
      <ItemList
        artworks={list}
        onToggle={handleToggle} />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              checked={artwork.seen}
              onChange={e => {
                onToggle(
                  artwork.id,
                  e.target.checked
                );
              }}
            />
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

</Sandpack>

Калі капіраванне аб'ектаў у кодзе становіцца нудным, вы можаце выкарыстоўваць бібліятэку [Immer](https://github.com/immerjs/use-immer), каб паменшыць колькасць кода, што паўтараецца:

<Sandpack>

```js
import { useState } from 'react';
import { useImmer } from 'use-immer';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Вялікі пузы', seen: false },
  { id: 1, title: 'Месяцовы пейзаж', seen: false },
  { id: 2, title: 'Тэракотовая армія', seen: true },
];

export default function BucketList() {
  const [list, updateList] = useImmer(initialList);

  function handleToggle(artworkId, nextSeen) {
    updateList(draft => {
      const artwork = draft.find(a =>
        a.id === artworkId
      );
      artwork.seen = nextSeen;
    });
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>Мой спіс мастацтва для прагляду:</h2>
      <ItemList
        artworks={list}
        onToggle={handleToggle} />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              checked={artwork.seen}
              onChange={e => {
                onToggle(
                  artwork.id,
                  e.target.checked
                );
              }}
            />
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

```json package.json
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "use-immer": "0.5.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

</Sandpack>

<LearnMore path="/learn/updating-arrays-in-state">

Даведайцеся ў артыкуле **«[Абнаўленне масіваў у стане](/learn/updating-arrays-in-state)»** аб тым, як правільна абнаўляць масівы.

</LearnMore>

## Што далей {/*whats-next*/}

Перайдзіце да старонкі «[Рэагаванне на падзеі](/learn/responding-to-events)», каб пачаць чытаць гэтую главу старонка за старонкай!

Або, калі вы ўжо знаёмыя з гэтымі тэмамі, чаму б не пачытаць пра «[Кіраванне станам](/learn/managing-state)»?

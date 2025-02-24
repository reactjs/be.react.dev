---
title: Рэндэр і фіксацыя
---

<Intro>

React павінен адрэндэрыць вашы кампаненты перш чым яны будуць выведзены на экран. Калі вы ведаеце этапы гэтага працэсу, вам будзе прасцей зразумець, як выконваецца ваш код, і растлумачыць яго паводзіны.

</Intro>

<YouWillLearn>

- Што азначае рэндэрынг у React
- Калі і чаму React рэндэрыць кампанент
- Этапы адлюстравання кампанента на экране
- Чаму рэндэрынг не заўсёды прыводзіць да абнаўлення DOM

</YouWillLearn>

Уявіце, што вашы кампаненты — гэта кухары на кухні, якія збіраюць смачныя стравы з інгрэдыентаў. У такім выпадку React — гэта афіцыянт, які прымае запыты кліентаў, а потым прыносіць ім іх стравы. Гэты працэс запыту і падачы карыстальніцкага інтэрфейсу складаецца з трох этапаў:

1. **Ініцыяцыя** рэндэрынгу (перадача заказа госця на кухню)
2. **Рэндэрынг** кампанента (гатаванне заказу на кухні)
3. **Фіксацыя** ў DOM (падача стравы на стол)

<IllustrationBlock sequential>
  <Illustration caption="Ініцыяцыя" alt="React — гэта афіцыянт у рэстаране, які атрымлівае заказы ад карыстальнікаў і дастаўляе іх на кухню кампанентаў." src="/images/docs/illustrations/i_render-and-commit1.png" />
  <Illustration caption="Рэндэрынг" alt="Шэф дае React свежы экземпляр кампанента Card." src="/images/docs/illustrations/i_render-and-commit2.png" />
  <Illustration caption="Фіксацыя" alt="React дастаўляе кампанент Cart карыстальніку за яго сталом." src="/images/docs/illustrations/i_render-and-commit3.png" /></IllustrationBlock>

## Этап 1: Ініцыяцыя рэндэрынгу {/*step-1-trigger-a-render*/}

Рэндэр кампанента адбываецца па дзвюх прычынах:

1. Гэта яго **першапачатковы рэндэр**.
2. Яго **стан** (або стан яго бацькоўскіх кампанентаў) **быў абноўлены**.

### Першапачатковы рэндэр {/*initial-render*/}

Калі ваша праграма запускаецца, неабходна запусціць першапачатковы рэндэрынг. Фрэймворкі і пясочніцы часам хаваюць гэты код, але першапачатковы рэндэр робіцца шляхам выкліку функцыі [`createRoot`](/reference/react-dom/client/createRoot) з мэтавым DOM-вузлом, а затым выклікам яго метаду `render` з вашым кампанентам:

<Sandpack>

```js src/index.js active
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

```js src/Image.js
export default function Image() {
  return (
    <img
      src="https://i.imgur.com/ZF6s192.jpg"
      alt="'Родавы Флораліс' Эдуарда Каталана: гіганцкая металічная скульптура кветкі з люстэркавымі пялёсткамі"
    />
  );
}
```

</Sandpack>

Паспрабуйце закаментаваць выклік `root.render()` і ўбачыце, як кампанент знікне!

### Паўторны рэндэр пры абнаўленні стану {/*re-renders-when-state-updates*/}

Пасля таго як кампанент быў першапачаткова адрэндэраны, вы можаце ініцыяваць наступныя рэндэры, абнаўляючы яго стан з дапамогай функцыі [`set`.](/reference/react/useState#setstate) Абнаўленне стану кампанента аўтаматычна ставіць яго ў чаргу на рэндэр. (Гэта падобна на наведвальніка рэстарана, які пасля першапачатковага заказу, дазаказвае сабе чай, дэсерт і разнастайныя рэчы, у залежнасці ад стану смагі ці голаду.)

<IllustrationBlock sequential>
<<<<<<< HEAD
  <Illustration caption="Абнаўленне стану..." alt="React — гэта афіцыянт у рэстаране, які дастаўляе карыстальніцкі інтэрфейс кампанета Card кліенту.Кліент кажа, што хоча ружовы Card, а не чорны!" src="/images/docs/illustrations/i_rerender1.png" />
  <Illustration caption="...запускае..." alt="React вяртаецца на кухню кампанентаў і кажа шэфу, што яму патрэбны ружовы Card." src="/images/docs/illustrations/i_rerender2.png" />
  <Illustration caption="...рэндэр!" alt="Шэф дае React ружовы Card." src="/images/docs/illustrations/i_rerender3.png" /></IllustrationBlock>
=======
  <Illustration caption="State update..." alt="React as a server in a restaurant, serving a Card UI to the user, represented as a patron with a cursor for their head. The patron expresses they want a pink card, not a black one!" src="/images/docs/illustrations/i_rerender1.png" />
  <Illustration caption="...triggers..." alt="React returns to the Component Kitchen and tells the Card Chef they need a pink Card." src="/images/docs/illustrations/i_rerender2.png" />
  <Illustration caption="...render!" alt="The Card Chef gives React the pink Card." src="/images/docs/illustrations/i_rerender3.png" />
</IllustrationBlock>
>>>>>>> fc29603434ec04621139738f4740caed89d659a7

## Этап 2: React рэндэрыць вашы кампаненты {/*step-2-react-renders-your-components*/}

Пасля запуску рэндэру React выклікае вашы кампаненты, каб вызначыць, што паказаць на экране. **«Рэндэрынг» — гэта калі React выклікае вашы кампаненты.**

- **Пры першапачатковым рэндэры** React выкліча каранёвы кампанент.
- **Пры наступных рэндэрах** React будзе выклікаць функцыю кампанента, абнаўленне стану якога выклікала рэндэр.

Гэта рэкурсіўны працэс: калі абноўлены кампанент вяртае нейкі іншы кампанент, React адрэндэрыць _гэты_ кампанент наступным, і калі гэты кампанент таксама вяртае што-небудзь, ён адрэндэрыць ужо _гэты_ кампанент наступным, і гэтак далей. Працэс будзе працягвацца датуль, пакуль не застанецца ўкладзеных кампанентаў і React не будзе дакладна ведаць, што павінна быць адлюстравана на экране.

<<<<<<< HEAD
У наступным прыкладзе React выклікае `Gallery()` і `Image()` некалькі разоў:
=======
In the following example, React will call `Gallery()` and `Image()` several times:
>>>>>>> fc29603434ec04621139738f4740caed89d659a7

<Sandpack>

```js src/Gallery.js active
export default function Gallery() {
  return (
    <section>
      <h1>Inspiring Sculptures</h1>
      <Image />
      <Image />
      <Image />
    </section>
  );
}

function Image() {
  return (
    <img
      src="https://i.imgur.com/ZF6s192.jpg"
      alt="'Родавы Флораліс' Эдуарда Каталана: гіганцкая металічная скульптура кветкі з люстэркавымі пялёсткамі"
    />
  );
}
```

```js src/index.js
import Gallery from './Gallery.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Gallery />);
```

```css
img { margin: 0 10px 10px 0; }
```

</Sandpack>

- **Пры першапачатковым рэндэры** React [створыць DOM-вузлы](https://developer.mozilla.org/docs/Web/API/Document/createElement) для `<section>`, `<h1>` і трох `<img>` тэгаў.
- **Падчас паўторнага рэндэру** React падлічыць, якія з іх уласцівасцей, калі такія маюцца, змяніліся з моманту папярэдняга рэндэру. Ён нічога не будзе рабіць з гэтай інфармацыяй да наступнага кроку, фазы фіксацыі.

<Pitfall>

Рэндэрынг заўсёды павінен быць [чыстым вылічэннем](/learn/keeping-components-pure):

- **Аднолькавыя ўваходныя даныя, аднолькавы вынік.** Пры аднолькавых уваходных даных кампанент павінен заўсёды вяртаць адзін і той жа JSX. (Калі нехта заказвае салат з памідорамі, то ён не павінен атрымаць салат з цыбуляй!)
- **Займаецца толькі сваёй задачай.** Ён не змяняе аб'екты або пераменныя, якія існавалі да рэндэру. (Адзін заказ не мусіць змяняць іншы заказ.)

У адваротным выпадку вы можаце сутыкнуцца з незразумелымі памылкамі і непрадказальнымі паводзінамі па меры росту складанасці вашай кодавай базы. Пры распрацоўцы ў «строгім рэжыме» (Strict Mode) React выклікае функцыю кожнага кампанента двойчы, што можа дапамагчы выявіць памылкі, выкліканыя нячыстымі функцыямі.

</Pitfall>

<DeepDive>

#### Аптымізацыя прадукцыйнасці {/*optimizing-performance*/}

Прадвызначаныя паводзіны, пры якіх адлюстроўваюцца ўсе кампаненты, укладзеныя ў абноўлены кампанент, не з'яўляюцца аптымальнымі з пункта гледжання прадукцыйнасці, калі абноўлены кампанент знаходзіцца вельмі высока ў дрэве. Калі вы сутыкнуліся з праблемай прадукцыйнасці, то ёсць некалькі спосабаў яе вырашэння, апісаных у раздзеле [Прадукцыйнасць](https://reactjs.org/docs/optimizing-performance.html). **Не аптымізуйце раней часу!**

</DeepDive>

## Этап 3: React фіксуе змены ў DOM {/*step-3-react-commits-changes-to-the-dom*/}

<<<<<<< HEAD
Пасля рэндэрынгу (выкліку) вашых кампанентаў React змяняе DOM.

- **Пры першапачатковым рэндэры** React выкарыстоўвае функцыю DOM API [`appendChild()`](https://developer.mozilla.org/docs/Web/API/Node/appendChild), каб вывесці на экран усе створаныя ім DOM-вузлы.
- **Пры паўторным рэндэры** React прыменіць мінімальна неабходныя аперацыі (якія вылічаюцца падчас рэндэрынгу!), каб DOM адпавядаў апошняму вываду рэндэрынгу.
=======
After rendering (calling) your components, React will modify the DOM.

* **For the initial render,** React will use the [`appendChild()`](https://developer.mozilla.org/docs/Web/API/Node/appendChild) DOM API to put all the DOM nodes it has created on screen.
* **For re-renders,** React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.
>>>>>>> fc29603434ec04621139738f4740caed89d659a7

**React змяняе DOM-вузлы толькі калі ёсць розніца паміж рэндэрамі.** Напрыклад, вось кампанент, які рэндэрыцца з рознымі пропсамі, якія перадаюцца ад бацькоўскага кампанента кожную секунду. Звярніце ўвагу, што вы можаце дадаць тэкст у `<input>`, абнаўляючы яго `значэнне`, і што пры паўторным рэндэры кампанента тэкст не знікае:

<Sandpack>

```js src/Clock.js active
export default function Clock({ time }) {
  return (
    <>
      <h1>{time}</h1>
      <input />
    </>
  );
}
```

```js src/App.js hidden
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
    <Clock time={time.toLocaleTimeString()} />
  );
}
```

</Sandpack>

Гэта працуе, таму што падчас апошняга кроку React абнаўляе толькі змесціва `<h1>` новым значэннем `time`. Ён бачыць, што `<input>` з'яўляецца ў JSX у тым жа месцы, што і ў мінулы раз, таму React не чапае `<input>` ці яго `значэнне`!

## Эпілог: Браўзерная адмалёўка {/*epilogue-browser-paint*/}

Пасля таго як рэндэр завершаны і React абнавіў DOM, браўзер перамалёўвае экран. Хоць гэты працэс і вядомы як «браўзерны рэндэрынг», мы будзем называць яго «маляваннем», каб пазбегнуць блытаніны ў дакументацыі.

<Illustration alt="Браўзер малюе карціну 'Нацюрморт з элементам Card'." src="/images/docs/illustrations/i_browser-paint.png" />

<Recap>

- Любое абнаўленне экрана ў React праграме адбываецца ў тры этапы:
  1. Ініцыяцыя
  2. Рэндэр
  3. Фіксацыя
- Для пошуку памылак у кампанентах можна выкарыстоўваць строгі рэжым (Strict Mode)
- React не змяняе DOM, калі вынік рэндэрынгу такі ж, як і ў мінулы раз

</Recap>

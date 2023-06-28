---
title: Напісанне разметкі з JSX
---

<Intro>

*JSX* — гэта пашырэнне сінтаксісу для JavaScript, якое дазваляе вам пісаць падобную на HTML разметку ўнутры JavaScript файла. Хай ёсць і іншыя спосабы напісання кампанентаў, большасць распрацоўшчыкаў React аддаюць перавагу лаканічнасці JSX, і большасць кодавых баз выкарыстоўваюць яго.

</Intro>

<YouWillLearn>

* Чаму React сумяшчае разметку з логікай рэндэру
* Чым JSX адрозніваецца ад HTML
* Як паказваць інфармацыю з дапамогай JSX

</YouWillLearn>

## JSX: дадаванне разметкі ў JavaScript {/*jsx-putting-markup-into-javascript*/}

Вэб пабудаваны на HTML, CSS. JavaScript. На працягу многіх гадоў вэб-распрацоўшчыкі захоўвалі змесціва ў HTML, дызайн у CSS, а логіку ў JavaScript. Найчасцей ў розных файлах! Змесціва знаходзілася ўнутры HTML разметкі, у час пакуль логіка змяшчалася асобна ў JavaScript:

<DiagramGroup>

<Diagram name="writing_jsx_html" height={237} width={325} alt="HTML разметка на фіялетавым фоне, дзе div змяшчае два дачынных элементы: p і form.">

HTML

</Diagram>

<Diagram name="writing_jsx_js" height={237} width={325} alt="Тры JavaScript апрацоўшчыкі на жоўтым фоне: onSubmit, onLogin і onClick.">

JavaScript

</Diagram>

</DiagramGroup>

Але з павялічэннем інтэрактыўнасці ў Вэб, колькасць логікі стала перавышаць перавышаць колькасць кантэнту. JavaScript кіраваў HTML! Таму **ў React логіка рэндэру і разметка жывуць разам у адным месцы — у кампанентах**

<DiagramGroup>

<Diagram name="writing_jsx_sidebar" height={330} width={325} alt="Кампанент React з HTML і JavaScript з папярэдняга прыкладу злучанымі разам. Функцыя Sidebar выклікае функцыю isLoggedIn, яны вылучаныя жоўтым. Унутры змяшчаюцца элементы p і Form, што паказаны на наступным відарысе, яны вылучаны філяетавым.">

React кампанент `Sidebar.js`

</Diagram>

<Diagram name="writing_jsx_form" height={330} width={325} alt="Кампанент React з HTML і JavaScript з папярэдняга прыкладу злучанымі разам. Функцыя Form змяшчае два апрацоўшчыкі onClick і onSubmit, яны вылучаныя жоўтым. За імі HTML вылучаныя фіялетавым. HTML змяшчае элемент form і два элементы input унутры, кожны з якіх мае проп onClick.">

React кампанент `Form.js`

</Diagram>

</DiagramGroup>

Захоўванне логікі рэндэру кнопкі разам з разметкай гарантуе, што яны застануцца сінхранізаванымі пасля зменаў. І наадварот, дэталі, якія не маюць дачынення адныя да іншых, такія, як разметка кнопкі і разметка бакавой панэлі, ізаляваныя, што дазваляе больш бяспечна змяняць іх незалежна адно ад аднаго.

Кожны кампанент React — функцыя JavaScript, якая можа змяшчаць разметку, якую React будзе рэндэрыць у браўзеры. Кампаненты React выкарыстоўваюць пашырэнне сінтаксісу, якое называецца JSX каб апісваць разметку. Разметка JSX вельмі падобная да HTML, але яна больш строгая і можа паказваць даныя дынамічна. Найлепшы спосаб зразумець гэта — паспрабаваць перарабіць HTML разметку ў JSX разметку.

<Note>

JSX і React — дзве аосбныя рэчы. Яны часта выкарыстоўваюцца разам, але вы *можаце* [выкарыстоўваць іх незалежна](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform) адно ад іншага. JSX — пашырэнне сінтаксісу, React жа — JavaScript бібліятэка.

</Note>

## Канвертаванне HTML у JSX {/*converting-html-to-jsx*/}

Уявім, што ў вас ёсць (цалкам правільны) HTML:

```html
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
```

І вы хочаце выкарыстоўваць яго ў сваім кампаненце:

```js
export default function TodoList() {
  return (
    // ???
  )
}
```

Калі вы яго проста скапіюеце і ўставіце, ён не спрацуе:


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
img { height: 90px }
```

</Sandpack>

Гэта адбываецца таму што JSX стражэйшы за HTML і мае шэраг дадатковых правілаў! Калі вы паспрабуеце чытаць паведамленні пра памылкі вышэй, яны дапамогуць вам выправіць разметку, таксама вы можаце скарыстацца дапаможнікам ніжэй.

<Note>

У большасці выпадкаў React будзе паказваць вам карысныя паведамленні пра памылкі прама на экане. Калі вы захраслі, паспрабуйце з імі азнаёміцца!

</Note>

## Правілы JSX {/*the-rules-of-jsx*/}

### 1. Заўсёды вяртайце адзін каранёвы элемент {/*1-return-a-single-root-element*/}

Каб вяртуць некалькі элементаў у межах аднаго кампанента, **абгарніце іх у адзін агульны тэг**.

Напрыклад, вы можаце выкарыстоўваць `<div>`:

```js {1,11}
<div>
  <h1>Спіс задач Хедзі Ламар</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</div>
```


Калі вы не хочаце дадаваць яшчэ адзін `<div>` у сваю разметку, вы можаце выкарыстоўваць пару тэгаў `<>` і `</>` замест:

```js {1,11}
<>
  <h1>Спіс задач Хедзі Ламар</h1>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
  >
  <ul>
    ...
  </ul>
</>
```

Гэты пусты тэг называецца *[Фрагмент.](/reference/react/Fragment)* Фрагменты дазваляюць вам групаваць рэчы, не пакідаючы слядоў у дрэвы HTML у браўзеры.

<DeepDive>

#### Чаму некалькі JSX тэгаў павінны быць абгорнутыя ў адзін? {/*why-do-multiple-jsx-tags-need-to-be-wrapped*/}

JSX выглядае як HTML, але «пад капотам» трансфармуецца ў звычайныя JavaScript аб’екты. Як вы не можаце вярнуць з функцыі два аб’екты, не абгарнуўшы іх у масіў, такім жа чынам нельга вярнуць два JSX тэгі, не абгарнуўшы іх у іншы тэг ці Фрагмент.

</DeepDive>

### 2. Закрывайце ўсе свае тэгі {/*2-close-all-the-tags*/}

JSX падрабуе, каб усе тэгі былі закрытыя: няпарныя тэгі, такія як `<img>`, ператвараюцца ў `<img />`, і абгортачныя тэгі, такія як `<li>апельсіны`, павінны быць абавязкова закрытымі: `<li>апельсіны</li>`.

Тэгі фота Хедзі Ламар і элементаў спіса павінны закрывацца вось такім чынам:

```js {2-6,8-10}
<>
  <img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
   />
  <ul>
    <li>Вынайсці новыя святлафоры</li>
    <li>Адрэпетаваць сцэну з фільма</li>
    <li>Палепшыць тэхналогію спектра</li>
  </ul>
</>
```

### 3. camelCase <s>для усяго</s> амаль для! {/*3-camelcase-salls-most-of-the-things*/}

JSX ператвараецца ў JavaScript і атрыбуты, напісаныя ў JSX, ператвараюцца ў ключы JavaScript аб’ектаў. Ва ўласных кампанентах у вас часта можа ўзнікаць патрэбнасць у чытанні гэтых атрыбутаў з пераменных. Але JavaScript мае абмежаванні ў назвах пераменных. Напрыклад, назвы не могуць уключаць злучнікі ці зарэзерваваныя словы, такія як `class`.

Таму ў React шмат якія HTML і SVG атрыбуты напісаныя ў camelCase. Напрыклад, замест `stroke-width` трэба выкарыстошваць `strokeWidth`. Так як `class` — зарэзерваванае слова, у React трэба пісаць `className` замест яго, што суадносіцца з [адпаведнай уласцівасцю DOM](https://developer.mozilla.org/en-US/docs/Web/API/Element/className):

```js {4}
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

Вы можаце [знайсці ўсе гэтыя атрыбуты ў спісе пропсаў DOM кампанентаў](/reference/react-dom/components/common). Калі вы штосьці напісалі не так, не хвалюйцеся, React паведаміць вам пра гэта і прапануе магчыма правільны варыянт у [кансолі вашага браўзера](https://developer.mozilla.org/docs/Tools/Browser_Console).

<Pitfall>

Так гістарычна склалася, што атрыбуты [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) і [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) напісаныя праз злучнікі, як і ў HTML.

</Pitfall>

### Падказка: карыстайцеся JSX канвертарам {/*pro-tip-use-a-jsx-converter*/}

Перапісваць усе гэтыя атрыбуты ва ўжо існуючай разметцы можа быць стомным! Мы раім карыстацца [канвертарам](https://transform.tools/html-to-jsx) каб перакласці вашыя існуючыя HTML і SVG у JSX. Канвертары вельмі карысныя на практыцы, але ўсё яшчэ варта разумець, што адбываецца, каб пісаць JSX было камфортна.

Вось так мае выглядаць канчатковы вынік:

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
img { height: 90px }
```

</Sandpack>

<Recap>

Цяпер вы ведаеце чаму JSX існуе і як выкарыстошваць яго ў кампанентах:

* Кампаненты React групуюць логіку рэндэру разам з разметкай таму што яны звязаныя паміж сабой.
* JSX падобны да HTML, але мае адрозненні. Вы можаце выкарыстоўваць [канвертар](https://transform.tools/html-to-jsx), калі тое патрэбна.
* Паведамленні пра памылкі часта будуць падказваць вам накірунак хутчэйшага вырашэння праблемы.

</Recap>



<Challenges>

#### Канвертуйце HTML у JSX {/*convert-some-html-to-jsx*/}

Дадзены HTML быў устаўлены ў кампанент, але ён не з’яўляецца правільным JSX. Выпраўце гэта:

<Sandpack>

```js
export default function Bio() {
  return (
    <div class="intro">
      <h1>Вітаю на сваім вэб-сайце!</h1>
    </div>
    <p class="summary">
      Тут вы можаце азнаёміцца з маімі думкамі.
      <br><br>
      <b>Таксама тут ёсць <i>фотаздымкі</b></i> навукоўцаў!
    </p>
  );
}
```

```css
.intro {
  background-image: linear-gradient(to left, violet, indigo, blue, green, yellow, orange, red);
  background-clip: text;
  color: transparent;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.summary {
  padding: 20px;
  border: 10px solid gold;
}
```

</Sandpack>

Карыстацца канвертарам ці зрабіць самастойна — ваш асабісты выбар!

<Solution>

<Sandpack>

```js
export default function Bio() {
  return (
    <div>
      <div className="intro">
        <h1>Вітаю на сваім вэб-сайце!</h1>
      </div>
      <p className="summary">
        Тут вы можаце азнаёміцца з маімі думкамі.
        <br /><br />
        <b>Таксама тут ёсць <i>фотаздымкі</i></b> навукоўцаў!
      </p>
    </div>
  );
}
```

```css
.intro {
  background-image: linear-gradient(to left, violet, indigo, blue, green, yellow, orange, red);
  background-clip: text;
  color: transparent;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.summary {
  padding: 20px;
  border: 10px solid gold;
}
```

</Sandpack>

</Solution>

</Challenges>

---
title: Ваш першы кампанент
---

<Intro>

*Кампаненты* -- адно з асноўных паняццяў React. Яны з'яўляюцца асновай, на якой вы будуеце карыстальніцкі інтэрфейс (UI), што робіць іх ідэальным месцам для пачатку вашага падарожжа па React!

</Intro>

<YouWillLearn>

* Што такое кампанент
* Якую ролю адыгрываюць кампаненты ў React
* Як напісаць свой першы React кампанент

</YouWillLearn>

## Кампаненты: будаўнічыя блокі UI {/*components-ui-building-blocks*/}

У інтэрнэце HTML дазваляе нам ствараць структураваныя дакументы, выкарыстоўваючы ўбудаваны набор тэгаў, напрыклад `<h1>` і `<li>`:

```html
<article>
  <h1>Мой першы кампанент</h1>
  <ol>
    <li>Кампаненты: будаўнічыя блокі UI</li>
    <li>Аб’яўленне кампанента</li>
    <li>Выкарыстанне кампанента</li>
  </ol>
</article>
```

Дадзеная разметка паказвае гэты артыкул `<article>`, яго загаловак `<h1>` і (скарочаны) змест як упарадкаваны спіс `<ol>`. Такая разметка ў спалучэнні з CSS для стылізаціі і JavaScript для дадавання інтэрактыўнасці хаваецца ў кожнай бакавой панэлі, аватары, мадальным меню, выпадаючым спісе — у кожнай частцы карыстальніцкага інтэрфейсу, якую вы бачыце ў інтэрнэце.

React дазваляе камбінаваць разметку, CSS і JavaScript у карыстальніцкія «кампаненты» — **прыдатныя да паўторнага выкарыстання элементы інтэрфейсу для вашай праграмы.** Код зместу вышэй, можна пераўтварыць у кампанент `<TableOfContents />`, які можна рэндэрыць на кожнай старонцы. «Пад капотам» ён па-ранейшаму выкарыстоўвае тыя ж тэгі HTML, такія як `<article>`, `<h1>` і г.д.

Гэтак жа, як і HTML тэгі, кампаненты можна камбінаваць, упарадкоўваць і ўкладаць адзін у аднаго для стварэння цэлых старонак. Напрыклад, старонка дакументацыі, якую вы зараз чытаеце, складаецца з кампанентаў React:

```js
<PageLayout>
  <NavigationHeader>
    <SearchBar />
    <Link to="/docs">Дакументацыя</Link>
  </NavigationHeader>
  <Sidebar />
  <PageContent>
    <TableOfContents />
    <DocumentationText />
  </PageContent>
</PageLayout>
```

Па меры росту вашага праекта, вы заўважыце, што многія з вашых старонак можна стварыць, паўторна выкарыстоўваючы ўжо гатовыя кампаненты, што паскарае распрацоўку. Наш змест вышэй можа быць дададзены на любы экран з дапамогай тэга `<TableOfContents />`! Можна нават хутка запусціць свой праект з дапамогай тысяч кампанентаў з адкрытым зыходным кодам, якія былі створаны супольнасцю React, напрыклад [Chakra UI](https://chakra-ui.com/) і [Material UI.](https://material-ui.com/)

## Аб’яўленне кампанента {/*defining-a-component*/}

Раней пры стварэнні вэб-старонак распрацоўшчыкі размячалі свой кантэнт, а затым дадавалі інтэрактыўнасць з дапамогай JavaScript. Гэта выдатна працавала, бо інтэрактыўнасць у інтэрнэце была проста прыемнай дробяззю. Сёння ж гэта абавязковая частка для многіх сайтаў і ўсіх праграм. React ставіць інтэрактыўнасць на першае месца, але пры гэтым выкарыстоўвае тую ж самую тэхналогію: **React кампанент — гэта JavaScript функцыя, якую вы можаце _прыпудрыць разметкай_.** Вось як гэта выглядае (вы можаце рэдагаваць прыклад ніжэй):

<Sandpack>

```js
export default function Profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Кэтрын Джонсан"
=======
      src="https://react.dev/images/docs/scientists/MK3eW3Am.jpg"
      alt="Katherine Johnson"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
    />
  )
}
```

```css
img { height: 200px; }
```

</Sandpack>

А вось як стварыць кампанент:

### Крок 1: Экспартаваць кампанент {/*step-1-export-the-component*/}

Прэфікс `export default` — гэта [стандартны сінтаксіс JavaScript](https://developer.mozilla.org/docs/web/javascript/reference/statements/export) (не з'яўляецца спецыфікай React). Ён дазваляе пазначыць галоўную функцыю ў файле, каб потым яе можна было імпартаваць з іншых файлаў. (Больш падрабязна пра імпартаванне можна пачытаць у раздзеле «[Імпартаванне і экспартаванне кампанентаў](/learn/importing-and-exporting-components)»!)

### Крок 2: Аб’явіць функцыю {/*step-2-define-the-function*/}

З дапамогай `function Profile() { }` вы аб’яўляеце JavaScript функцыю з назвай `Profile`.

<Pitfall>

Кампаненты React — гэта звычайныя JavaScript функцыі, але **іх назвы павінны пачынацца з вялікай літары**, інакш яны не будуць працаваць!

</Pitfall>

### Крок 3: Дадаць разметку {/*step-3-add-markup*/}

Кампанент вяртае тэг `<img />` з атрыбутамі `src` і `alt`. Тэг `<img />` выглядае як HTML, але насамрэч пад капотам гэта JavaScript! Гэты сінтаксіс называецца [JSX](/learn/writing-markup-with-jsx), і ён дазваляе вам устаўляць разметку ў JavaScript.

Аператар `return` можа быць запісаны ў адзін радок, як у гэтым кампаненце:

```js
<<<<<<< HEAD
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Кэтрын Джонсан" />;
=======
return <img src="https://react.dev/images/docs/scientists/MK3eW3As.jpg" alt="Katherine Johnson" />;
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
```

Але калі ўся ваша разметка не знаходзіцца ў тым жа радку, што і ключавое слова `return`, то вы павінны заключыць яе ў дужкі:

```js
return (
  <div>
<<<<<<< HEAD
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Кэтрын Джонсан" />
=======
    <img src="https://react.dev/images/docs/scientists/MK3eW3As.jpg" alt="Katherine Johnson" />
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
  </div>
);
```

<Pitfall>

Без круглых дужак любы код у радках пасля `return` [будзе ігнаравацца](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi) !

</Pitfall>

## Выкарыстанне кампанента {/*using-a-component*/}

Цяпер, калі вы аб’явілі свой кампанент `Profile`, вы можаце ўкладаць яго ў іншыя кампаненты. Напрыклад, вы можаце экспартаваць кампанент `Gallery`, які выкарыстоўвае некалькі кампанентаў `Profile`:

<Sandpack>

```js
function Profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Кэтрын Джонсан"
=======
      src="https://react.dev/images/docs/scientists/MK3eW3As.jpg"
      alt="Katherine Johnson"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
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

### Што бачыць браўзер {/*what-the-browser-sees*/}

Звярніце ўвагу на розніцу ў рэгістры:

* Тэг `<section>` пішацца ў ніжнім рэгістры, таму React ведае, што мы спасылаемся на тэг HTML.
* Тэг `<Profile />` пачынаецца з вялікай літары `P`, таму React ведае, што мы хочам выкарыстоўваць наш кампанент пад назвай `Profile`.

А `Profile` змяшчае яшчэ нават больш HTML: `<img />`. У рэшце, вось што бачыць браўзер:

```html
<section>
<<<<<<< HEAD
  <h1>Выбітныя навукоўцы</h1>
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Кэтрын Джонсан" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Кэтрын Джонсан" />
  <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Кэтрын Джонсан" />
=======
  <h1>Amazing scientists</h1>
  <img src="https://react.dev/images/docs/scientists/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://react.dev/images/docs/scientists/MK3eW3As.jpg" alt="Katherine Johnson" />
  <img src="https://react.dev/images/docs/scientists/MK3eW3As.jpg" alt="Katherine Johnson" />
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
</section>
```

### Укладанне і арганізацыя кампанентаў {/*nesting-and-organizing-components*/}

Кампаненты — гэта звычайныя JavaScript функцыі, таму вы можаце мець некалькі кампанентаў у адным файле. Гэта зручна, калі кампаненты адносна невялікія або цесна звязаны адзін з адным. Калі файл становіцца перапоўненым, вы заўсёды можаце перанесці `Profile` у асобны файл. Хутка вы даведаецеся, як гэта зрабіць на [старонцы пра імпартаванне.](/learn/importing-and-exporting-components)

Паколькі кампаненты `Profile` рэндэрыцца ўнутры `Gallery` (нават некалькі разоў!), мы можам сказаць, што `Gallery` з'яўляецца **бацькоўскім кампанентам**, які рэндэрыць кожны кампанент `Profile` як «даччыны». Гэта частка магіі React: вы можаце аб’явіць кампанент адзін раз, а потым выкарыстоўваць яго ў любой колькасці месцаў і колькі хочаце разоў.

<Pitfall>

Кампаненты могуць рэндэрыць іншыя кампаненты, але **вы ніколі не павінны ўкладаць іх аб’яўленні:**

```js {2-5}
export default function Gallery() {
  // 🔴 Ніколі не аб’яўляйце кампанент унутры іншага кампанента!
  function Profile() {
    // ...
  }
  // ...
}
```

Код вышэй [вельмі павольны і выклікае памылкі.](/learn/preserving-and-resetting-state#different-components-at-the-same-position-reset-state) Замест гэтага аб’яўляйце кожны кампанент на верхнім узроўні:

```js {5-8}
export default function Gallery() {
  // ...
}

// ✅ Аб’яўляйце кампаненты на верхнім узроўні
function Profile() {
  // ...
}
```

Калі даччынаму кампаненту патрэбны некаторыя даныя ад бацькоўскага, [перадайце іх праз пропсы](/learn/passing-props-to-a-component) замест укладзеных аб’яўленняў.

</Pitfall>

<DeepDive>

#### Кампаненты паўсюль {/*components-all-the-way-down*/}

Ваша React праграма пачынаецца з «каранёвага» кампанента. Звычайна ён ствараецца аўтаматычна, пры стварэнні новага праекта. Напрыклад, калі вы выкарыстоўваеце [CodeSandbox](https://codesandbox.io/) або калі вы выкарыстоўваеце фрэймворк [Next.js](https://nextjs.org/), каранёвы кампанент аб’яўляецца ў файле `pages/index.js`. У прыкладах вышэй вы экспартавалі каранёвыя кампаненты.

Большасць React праграм выкарыстоўваюць кампаненты паўсюль. Гэта азначае, што вы будзеце выкарыстоўваць кампаненты не толькі для элементаў, што паўторна выкарыстоўваюцца, такіх як кнопкі, але і для буйнейшых элементаў: бакавых панэляў, спісаў і, у рэшце, цэлых старонак! Кампаненты — гэта зручны спосаб арганізаваць код карыстальніцкага інтэрфейсу і разметку, нават калі некаторыя з іх выкарыстоўваюцца толькі адзін раз.

[Фрэймворкі на базе React](/learn/creating-a-react-app) пайшлі яшчэ далей. Замест таго, каб выкарыстоўваць пусты HTML файл і дазволіць React «узяць на сябе» кіраванне старонкай з дапамогай JavaScript, яны *таксама* аўтаматычна ствараюць HTML з вашых кампанентаў React. Гэта дазваляе вашай праграме паказваць частку змесціва да таго, як JavaScript код загрузіцца.

Тым не менш, многія вэб-сайты выкарыстоўваюць React толькі для [дадавання інтэрактыўнасці да існуючых HTML старонак.](/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page) Яны маюць шмат каранёвых кампанентаў замест аднаго для ўсёй старонкі. Вы можаце браць ад React столькі, колькі вам трэба.

</DeepDive>

<Recap>

Вы толькі што пазнаёміліся з React! Давайце паўторым некаторыя ключавыя моманты.

* React дазваляе вам ствараць кампаненты — **прыдатныя да паўторнага выкарыстання элементы карыстальніцкага інтэрфейсу для вашай праграмы.**
* У React праграме кожны элемент UI з'яўляецца кампанентам.
* Кампаненты React з'яўляюцца звычайнымі JavaScript функцыямі, за выключэннем таго, што:

  1. Іх назвы заўсёды пачынаюцца з вялікай літары.
  2. Яны вяртаюць JSX разметку.

</Recap>



<Challenges>

#### Экспартуйце кампанент {/*export-the-component*/}

Гэты прыклад не працуе, таму што каранёвы кампанент не экспартуецца:

<Sandpack>

```js
function Profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/lICfvbD.jpg"
      alt="Аклілу Лема"
=======
      src="https://react.dev/images/docs/scientists/lICfvbD.jpg"
      alt="Aklilu Lemma"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
    />
  );
}
```

```css
img { height: 181px; }
```

</Sandpack>

Паспрабуйце выправіць яго самастойна, перш чым глядзець у рашэнне!

<Solution>

Дадайце `export default` перад аб’яўленнем функцыі наступным чынам:

<Sandpack>

```js
export default function Profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/lICfvbD.jpg"
      alt="Аклілу Лема"
=======
      src="https://react.dev/images/docs/scientists/lICfvbD.jpg"
      alt="Aklilu Lemma"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
    />
  );
}
```

```css
img { height: 181px; }
```

</Sandpack>

Вы можаце задацца пытаннем, чаму напісання толькі `export` недастаткова, каб выправіць гэты прыклад. Вы можаце даведацца пра розніцу паміж `export` і `export default` у раздзеле [Імпартаванне і экспартаванне кампанентаў.](/learn/importing-and-exporting-components)

</Solution>

#### Выправіце аператар вяртання {/*fix-the-return-statement*/}

З гэтым аператарам `return` штосьці не так. Вы можаце яго выправіць?

<Hint>

Пры спробе выправіць аператар, вы можаце атрымаць памылку «Unexpected token». У такім выпадку пераканайцеся, што кропка з коскай з'яўляецца *пасля* закрывальнай дужкі. Пакінуўшы кропку з коскай унутры `return ( )`, вы атрымаеце памылку.

</Hint>


<Sandpack>

```js
export default function Profile() {
  return
<<<<<<< HEAD
    <img src="https://i.imgur.com/jA8hHMpm.jpg" alt="Кацуко Сарухасі" />;
=======
    <img src="https://react.dev/images/docs/scientists/jA8hHMpm.jpg" alt="Katsuko Saruhashi" />;
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
}
```

```css
img { height: 180px; }
```

</Sandpack>

<Solution>

Вы можаце выправіць гэты кампанент, запісаўшы аператар `return` у адзін радок, вось так:

<Sandpack>

```js
export default function Profile() {
<<<<<<< HEAD
  return <img src="https://i.imgur.com/jA8hHMpm.jpg" alt="Кацуко Сарухасі" />;
=======
  return <img src="https://react.dev/images/docs/scientists/jA8hHMpm.jpg" alt="Katsuko Saruhashi" />;
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
}
```

```css
img { height: 180px; }
```

</Sandpack>

Або заключыўшы вернутую разметку JSX у дужкі, якія адкрываюцца адразу пасля аператара `return`:

<Sandpack>

```js
export default function Profile() {
  return (
<<<<<<< HEAD
    <img 
      src="https://i.imgur.com/jA8hHMpm.jpg" 
      alt="Кацуко Сарухасі" 
=======
    <img
      src="https://react.dev/images/docs/scientists/jA8hHMpm.jpg"
      alt="Katsuko Saruhashi"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
    />
  );
}
```

```css
img { height: 180px; }
```

</Sandpack>

</Solution>

#### Знайдзіце памылку {/*spot-the-mistake*/}

Нешта не так з тым, як дэкларуецца і выкарыстоўваецца кампанент `Profile`. Ці можаце вы знайсці памылку? (Паспрабуйце ўспомніць, як React адрознівае кампаненты ад звычайных HTML тэгаў!)

<Sandpack>

```js
function profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Алан Л. Харт"
=======
      src="https://react.dev/images/docs/scientists/QIrZWGIs.jpg"
      alt="Alan L. Hart"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Выбітныя навукоўцы</h1>
      <profile />
      <profile />
      <profile />
    </section>
  );
}
```

```css
img { margin: 0 10px 10px 0; height: 90px; }
```

</Sandpack>

<Solution>

Назвы кампанентаў React павінны пачынацца з вялікай літары.

Замяніце `function profile()` на `function Profile()`, а затым кожны `<profile />` на `<Profile />`:

<Sandpack>

```js
function Profile() {
  return (
    <img
<<<<<<< HEAD
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Алан Л. Харт"
=======
      src="https://react.dev/images/docs/scientists/QIrZWGIs.jpg"
      alt="Alan L. Hart"
>>>>>>> abe931a8cb3aee3e8b15ef7e187214789164162a
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
img { margin: 0 10px 10px 0; }
```

</Sandpack>

</Solution>

#### Ваш уласны кампанент {/*your-own-component*/}

Напішыце кампанент з нуля. Вы можаце даць яму любую дапушчальную назву і вярнуць любую разметку. Калі ў вас скончыліся ідэі, вы можаце напісаць кампанент `Congratulations`, які паказвае `<h1>Малайчына!</h1>`. Не забудзьцеся экспартаваць свой кампанент!

<Sandpack>

```js
// Напішыце свой кампанент ніжэй!

```

</Sandpack>

<Solution>

<Sandpack>

```js
export default function Congratulations() {
  return (
    <h1>Малайчына!</h1>
  );
}
```

</Sandpack>

</Solution>

</Challenges>

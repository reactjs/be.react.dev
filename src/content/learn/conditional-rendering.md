---
title: Умоўны рэндэрынг
---

<Intro>

Вашым кампанентам часта трэба будзе адлюстроўваць розныя рэчы ў залежнасці ад розных умоў. У React вы можаце ўмоўна рэндэрыць JSX, выкарыстоўваючы такія JavaScript аператары, як `if`, `&&` і `?:`.

</Intro>

<YouWillLearn>

* Як вярнуць розны JSX у залежнасці ад умовы
* Як у залежнасці ад умоў уключаць або выключаць частку JSX
* Распаўсюджаныя скарачэнні сінтаксісу ўмоўных выразаў, з якімі вы сутыкняцеся ў праектах на React.

</YouWillLearn>

## Умоўнае вяртанне JSX {/*conditionally-returning-jsx*/}

Дапусцім, у вас ёсць кампанент `PackingList`, які рэндэрыць некалькі элементаў `Item`, якія могуць быць пазначаны як упакаваныя ці не:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<<<<<<< HEAD
Звярніце ўвагу, што ў некаторых кампанентаў `Item` пропс `isPacked` мае значэнне `true` замест `false`. Калі `isPacked={true}`, мы хочам дадаць галачку(✔) да спакаваных рэчаў.
=======
Notice that some of the `Item` components have their `isPacked` prop set to `true` instead of `false`. You want to add a checkmark (✅) to packed items if `isPacked={true}`.
>>>>>>> 7d50c3ffd4df2dc7903f4e41069653a456a9c223

Вы можаце зрабіць гэта з дапамогай [канструкцыі `if`/`else`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) такім чынам:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Калі пропс `isPacked` мае значэнне `true`, гэты код **верне іншае дрэва JSX.** Разам з гэтай зменай, некаторыя элементы атрымаюць галачку ў канцы:

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Паспрабуйце адрэдагаваць тое, што вяртаецца ў кожным выпадку, і паглядзіце, як зменіцца вынік!

Звярніце ўвагу, як вы ствараеце разгалінаваную логіку з дапамогай JavaScript аператараў `if` і `return`. У React паток кіравання (напрыклад, умовы) апрацоўваецца JavaScript.

### Умоўнае вяртанне нічога, з дапамогай `null` {/*conditionally-returning-nothing-with-null*/}

У некаторых сітуацыях вам можа спатрэбіцца ўвогуле нічога не рэндэрыць. Напрыклад, вы наогул не хочаце паказваць спакаваныя рэчы. Але кампанент павінен нешта вяртаць. У гэтым выпадку вы можаце вярнуць `null`:

```js
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

Калі `isPacked` мае значэнне `true` — кампанент нічога не вяртае, `null`. У адваротным выпадку ён верне JSX для рэндэрынгу.

<Sandpack>

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

На практыцы вяртанне `null` з кампанента не з'яўляецца звычайнай практыкай, таму што гэта можа здзівіць распрацоўшчыка, які спрабуе рэндэрыць яго. Часцей вы будзеце ўмоўна ўключаць або выключаць кампанент у JSX бацькоўскага кампанента. Вось як гэта можна зрабіць!

## Умоўнае ўключэнне JSX {/*conditionally-including-jsx*/}

У папярэднім прыкладзе вы кантралявалі, якое JSX дрэва будзе вернута кампанентам (калі нешта будзе вернута ўвогуле!). Магчыма, вы ўжо заўважылі нейкае дубліраванне ў вывадзе рэндэрынгу:

```js
<li className="item">{name} ✅</li>
```

вельмі падобна на

```js
<li className="item">{name}</li>
```

Абедзве галіны вяртаюць `<li className="item">...</li>`:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Хоць гэта дубліраванне і не шкоднае, але яно можа ўскладніць падтрыманне вашага кода. Што калі вы захочаце змяніць `className`? Вам трэба будзе зрабіць гэта ў двух месцах у вашым кодзе! У такой сітуацыі вы можаце ўмоўна ўключыць невялікі JSX, каб зрабіць свой код больш [DRY](Https://en.wikipedia.org/wiki/don%27t_repeat_yourself) (сухім).

### Умоўны (тэрнарны) аператар (`? :`) {/*conditional-ternary-operator--*/}

JavaScript мае кампактны сінтаксіс для запісу ўмоўнага выражэння -- [умоўны аператар](https://developer.mozilla.org/en-us/docs/web/javascript/reference/operators/conditional_operator) або «тэрнарны аператар».

Замест гэтага:

```js
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

Вы можаце напісаць гэта:

```js
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

<<<<<<< HEAD
Гэта можна прачытаць як: *«Калі `ispacked` роўны `true`, тады (`?`) рэндэрым `name + ' ✔'`, інакш (`:`) рэндэрым `name`»*.
=======
You can read it as *"if `isPacked` is true, then (`?`) render `name + ' ✅'`, otherwise (`:`) render `name`"*.
>>>>>>> 7d50c3ffd4df2dc7903f4e41069653a456a9c223

<DeepDive>

#### Ці з'яўляюцца гэтыя два прыклады цалкам эквівалентнымі? {/*are-these-two-examples-fully-equivalent*/}

Калі вы знаёмы з аб'ектна-арыентаваным праграмаваннем, вы можаце выказаць здагадку, што два прыведзеныя вышэй прыклады крыху адрозніваюцца, таму што адзін з іх можа стварыць два розныя «асобнікі» `<li>`. Але JSX элементы не з'яўляюцца «асобнікамі», таму што яны не маюць ніякага ўнутранага стану і не з'яўляюцца сапраўднымі вузламі DOM. Гэта лёгкія апісанні, як чарцяжы. Такім чынам, гэтыя два прыклады, на самай справе, *цалкам эквівалентныя*. [Захаванне і скідванне стану](/learn/preserving-and-resetting-state) падрабязна распавядае пра тое, як гэта працуе.

</DeepDive>

Зараз дапусцім, што вы жадаеце абгарнуць завершаны тэкст элемента ў іншы HTML тэг, напрыклад `<del>`, каб выкрасліць яго. Вы можаце дадаць новыя радкі і круглыя дужкі, каб было лягчэй укладваць JSX у кожным з выпадкаў:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✅'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Гэты стыль добра працуе для простых умоў, але выкарыстоўвайце яго ў меру. Калі вашы кампаненты становяцца занадта заблытанымі з-за вялікай колькасці ўкладзенай умоўнай разметкі, падумайце аб вынясенні часткі кода ў даччыныя кампаненты, каб навесці парадак. У React разметка з'яўляецца часткай вашага кода, таму вы можаце выкарыстоўваць такія інструменты, як пераменныя і функцыі, каб упарадкаваць складаныя выразы.

### Лагічны аператар І (`&&`) {/*logical-and-operator-*/}

Яшчэ адно скарачэнне, якое часта сустракаецца ў JavaScript — гэта [лагічны аператар І (`&&`).](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#:~:text=The%20logical%20AND%20(%20%26%26%20)%20operator,it%20returns%20a%20Boolean%20value.) Унутры кампанентаў React ён часта выкарыстоўваецца, калі вам трэба адрэндэрыць нейкі JSX, калі ўмова роўная `true`, **або нічога не рэндэрыць інакш.** З дапамогай `&&` вы можаце ўмоўна рэндэрыць «птушку», толькі калі `isPacked` мае значэнне `true`:

```js
return (
  <li className="item">
    {name} {isPacked && '✅'}
  </li>
);
```

Вы можаце чытаць гэта як *«калі `isPacked`, тады (`&&`) рэндэрым «птушку», у адваротным выпадку — нічога не рэндэрым»*.

Вось яно ў дзеянні:

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

[JavaScript выраз &&](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) вяртае значэнне правага боку (у нашым выпадку «птушка»), калі левы бок (наша ўмова) з'яўляецца `true`. Але калі наша ўмова — `false`, тады ўвесь выраз становіцца `false`. React разглядае `false` як «дзірку» ў дрэве JSX, зусім як `null` або `undefined`, і нічога не рэндэрыць на гэтым месцы.


<Pitfall>

**Не стаўце лічбы злева ад `&&`.**

Каб праверыць умову, JavaScript аўтаматычна пераўтворыць левы бок у булевае значэнне. Аднак, калі левы бок роўны `0`, то ўвесь выраз атрымлівае гэта значэнне (`0`), і React з задавальненнем рэндэрыць `0` замест нічога.

Напрыклад, распаўсюджанай памылкай з'яўляецца напісанне кода накшталт `messageCount && <p>Новыя паведамленні</p>`. Лёгка выказаць здагадку, што ён нічога не рэндэрыць, калі `messageCount` роўны `0`, але на самай справе ён рэндэрыць `0`!

Каб выправіць гэта, зрабіце левы бок булевым значэннем: `messageCount > 0 && <p>Новыя паведамленні</p>`.

</Pitfall>

### Умоўнае прысваенне JSX да пераменнай {/*conditionally-assigning-jsx-to-a-variable*/}

Калі скарачэнні перашкаджаюць напісанню зразумелага кода, паспрабуйце выкарыстоўваць аператар `if` і пераменную. Вы можаце пераназначыць пераменныя, аб'яўленыя з дапамогай [`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let), таму пачніце з прадастаўлення прадвызначанага змесціва, якое вы хочаце паказаць, напрыклад, name:

```js
let itemContent = name;
```

Выкарыстоўвайце аператар `if`, каб пераназначыць JSX выраз `itemContent`, калі `isPacked` мае значэнне `true`:

```js
if (isPacked) {
  itemContent = name + " ✅";
}
```

[Фігурныя дужкі «адчыняюць акенца» ў JavaScript.](/learn/javascript-in-jsx-with-curly-braces#using-curly-braces-a-window-into-the-javascript-world) Устаўце пераменную з дапамогай фігурных дужак у JSX дрэва, якое вяртаецца з кампанента, уклаўшы раней вылічаны выраз унутр JSX:

```js
<li className="item">
  {itemContent}
</li>
```

Гэты стыль найбольш шматслоўны, але і найбольш гнуткі. Вось ён у дзеянні:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Як і раней, гэта працуе не толькі для тэксту, але і для адвольнага JSX:

<Sandpack>

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✅"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Калі вы не знаёмыя з JavaScript, то такая разнастайнасць стыляў спачатку можа здацца ашаламляльнай. Аднак іх вывучэнне дапаможа вам чытаць і пісаць любы JavaScript код, а не толькі React кампаненты! Для пачатку абярыце той, які вам больш падабаецца, і пры неабходнасці звярніцеся да гэтага даведніка, калі забудзецеся, як працуюць іншыя.

<Recap>

* У React вы кіруеце логікай галінавання з дапамогай JavaScript.
* Вы можаце ўмоўна вярнуць JSX выраз з дапамогай аператара `if`.
* Вы можаце ўмоўна захаваць JSX у пераменную, а потым уключыць яе ў іншы JSX, выкарыстоўваючы фігурныя дужкі.
* У JSX выраз `{cond ? <A /> : <B />}` азначае *«калі `cond`, рэндэрыць `<A />`, інакш `<B />`»*.
* У JSX выраз`{cond && <A />}` азначае *«калі `cond`, рэндэрыць `<A />`, інакш нічога»*.
* Скарачэнні часта выкарыстоўваюцца, але вы не абавязаны выкарыстоўваць іх, калі аддаяце перавагу простаму `if`.

</Recap>



<Challenges>

#### Паказаць значок для неспакаваных рэчаў з дапамогай `? :` {/*show-an-icon-for-incomplete-items-with--*/}

Выкарыстоўвайце ўмоўны аператар (`cond? a : b`), каб паказаць ❌, калі `isPacked` не мае значэнне `true`.

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<Solution>

<Sandpack>

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked ? '✅' : '❌'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Касмічны скафандр" 
        />
        <Item 
          isPacked={true} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          isPacked={false} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

</Solution>

#### Паказаць важнасць рэчы з дапамогай `&&` {/*show-the-item-importance-with-*/}

У гэтым прыкладзе кожны `Item` атрымлівае лікавы пропс `importance`. Выкарыстоўвайце аператар `&&`, каб паказаць «_(Важнасць: X)_» курсівам, але толькі для элементаў з ненулявой важнасцю. Ваш спіс рэчаў павінен выглядаць наступным чынам:

* Касмічны скафандр _(Важнасць: 9)_
* Шлем з залатым лістом
* Фота Тэма _(Важнасць: 6)_

Не забудзьцеся дадаць прабел паміж дзвюма меткамі!

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          importance={9} 
          name="Касмічны скафандр" 
        />
        <Item 
          importance={0} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          importance={6} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

<Solution>

Вось як гэта можна зрабіць:

<Sandpack>

```js
function Item({ name, importance }) {
  return (
    <li className="item">
      {name}
      {importance > 0 && ' '}
      {importance > 0 &&
        <i>(Важнасць: {importance})</i>
      }
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Спіс рэчаў Салі Райд</h1>
      <ul>
        <Item 
          importance={9} 
          name="Касмічны скафандр" 
        />
        <Item 
          importance={0} 
          name="Шлем з залатым лістом" 
        />
        <Item 
          importance={6} 
          name="Фота Тэма" 
        />
      </ul>
    </section>
  );
}
```

</Sandpack>

Звярніце ўвагу, што трэба пісаць `importance > 0 && ...`, а не `importance && ...`, каб пры `importance` роўным `0` у выніку не рэндэрылася лічба `0`!

У гэтым рашэнні дзве асобныя ўмовы выкарыстоўваюцца для ўстаўкі прабелу паміж іменем і меткай важнасці. Але замест гэтага вы таксама можаце выкарыстоўваць фрагмент з прабелам у пачатку: `importance > 0 && <> <i>...</i></>` або дадаць прабел непасрэдна ўнутры `<i>`: `importance > 0 && <i> ...</i>`.

</Solution>

#### Замяніце шэраг тэрнарных аператараў `? :` на `if` і пераменныя {/*refactor-a-series-of---to-if-and-variables*/}

Кампанент `Drink` выкарыстоўвае шэраг умоў `? :` для паказу рознай інфармацыі ў залежнасці ад значэння пропса `name` (можа быць `"tea"` або `"coffee"`). Праблема заключаецца ў тым, што інфармацыя аб кожным напоі раскідана па некалькіх умовах. Перапішыце код так, каб выкарыстоўваўся толькі адзін аператар `if` замест трох умоў `? :`.

<Sandpack>

```js
function Drink({ name }) {
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Частка расліны</dt>
        <dd>{name === 'tea' ? 'ліст' : 'зерне'}</dd>
        <dt>Колькасць кафеіну</dt>
        <dd>{name === 'tea' ? '15–70 мг/кубак' : '80–185 мг/кубак'}</dd>
        <dt>Узрост</dt>
        <dd>{name === 'tea' ? '4,000+ гадоў' : '1,000+ гадоў'}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

Пасля таго, як вы перапісалі код з выкарыстаннем `if`, ці ёсць у вас далейшыя ідэі, як яго спрасціць?

<Solution>

Вы можаце зрабіць гэта некалькімі спосабамі, але вось адзін спосаб з якога можна пачаць:

<Sandpack>

```js
function Drink({ name }) {
  let part, caffeine, age;
  if (name === 'tea') {
    part = 'ліст';
    caffeine = '15–70 мг/кубак';
    age = '4,000+ гадоў';
  } else if (name === 'coffee') {
    part = 'зерне';
    caffeine = '80–185 мг/кубак';
    age = '1,000+ гадоў';
  }
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Частка расліны</dt>
        <dd>{part}</dd>
        <dt>Колькасць кафеіну</dt>
        <dd>{caffeine}</dd>
        <dt>Узрост</dt>
        <dd>{age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

Тут інфармацыя аб кожным напоі згрупавана разам, а не раскідана па некалькіх умовах. Гэта палягчае дадаванне новых напояў у будучыні.

Іншым рашэннем было б цалкам выдаліць умову, перамясціўшы інфармацыю ў аб'екты:

<Sandpack>

```js
const drinks = {
  tea: {
    part: 'ліст',
    caffeine: '15–70 мг/кубак',
    age: '4,000+ гадоў'
  },
  coffee: {
    part: 'зерне',
    caffeine: '80–185 мг/кубак',
    age: '1,000+ гадоў'
  }
};

function Drink({ name }) {
  const info = drinks[name];
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Частка расліны</dt>
        <dd>{info.part}</dd>
        <dt>Колькасць кафеіну</dt>
        <dd>{info.caffeine}</dd>
        <dt>Узрост</dt>
        <dd>{info.age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

</Sandpack>

</Solution>

</Challenges>

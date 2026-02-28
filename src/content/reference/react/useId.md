---

## title: useId {/*title-useid*/}



`useId` — хук React для генерацыі ўнікальных ідэнтыфікатараў, якія далей можна выкарыстоўваць у атрыбутах даступнасці.

```js
const id = useId()
```





---

## Апісанне {/*reference*/}

### `useId()` {/*useid*/}

Выклічце `useId` на верхнім узроўні вашага кампанента, каб згенераваць унікальны ідэнтыфікатар:

```js
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();
  // ...
```

[Болей прыкладаў глядзіце ніжэй.](#usage)

#### Параметры {/*parameters*/}

`useId` не прымае аніякіх параметраў.

#### Значэнні, якія вяртаюцца {/*returns*/}

`useId` вяртае радок з унікальным ідэнтыфікатарам, які праасацыяваны з гэтым канкрэтным выклікам `useId` у гэтым канкрэтным кампаненце.

#### Агаворкі {/*caveats*/}

- `useId` — хук, а значыць, вы можаце яго выклікаць толькі **на верхнім узроўні вашага кампанента** ці ўнутры ўласнага хука. Ëн не можа быць выкліканы ўнутры цыкла альбо ўмовы. Калі вы гэтага патрабуеце, выміце ў новы кампанент і перанясіце стан туды.
- `useId` **не мусіць быць выкарыстаны для генерацыі ключоў кэша** для [use()](/reference/react/use). Ідэнтыфікатар стабільны пакуль кампанент прымантаваны, але можа змяняцца паміж рэндарамі. Ключы кэша мусяць быць згенераванымі на падставе вашых даных.
- `useId` **не мусіць быць выкарыстаны для генерацыі ключоў** у спісах. [Ключы мусяць быць згенераванымі на падставе вашых даных](/learn/rendering-lists#where-to-get-your-key).
- `useId` на дадзены момант не можа быць выкарыстаны ўнутры [асінхронных серверных кампанентаў](/reference/rsc/server-components#async-components-with-server-components).

---

## Выкарыстанне {/*usage*/}



**Не выкарыстоўвайце `useId` для генерацыі ключоў у спісах.** [Ключы мусяць быць згенераванымі на падставе вашых даных](/learn/rendering-lists#where-to-get-your-key).



### Генерацыя ўнікальных ідэнтыфікатараў для атрыбутаў даступнасці {/*generating-unique-ids-for-accessibility-attributes*/}

Выклічце `useId` на верхнім узроўні вашага кампанента каб згенераваць унікальны ідэнтыфікатар:

```js [[1, 4, "passwordHintId"]]
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();
  // ...
```

Далей вы можаце перадаць згенераваны ідэнтыфікатар у розныя атрыбуты:

```js [[1, 2, "passwordHintId"], [1, 3, "passwordHintId"]]
<>
  <input type="password" aria-describedby={passwordHintId} />
  <p id={passwordHintId}>
</>
```

**Давайце разгледзім прыклад, калі гэта можа быць карысна.**

[Атрыбуты даступнасці ў HTML](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) як, напрыклад, `[aria-describedby](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-describedby)` дазваляюць пазначыць, што два тэгі звязаныя адно з адным. Такім чынам, як варыянт, вы можаце адзначыць, што элемент (напрыклад, поле ўводу) апісаны іншым элементам (напрыклад, параграфам).

У звычайным HTML, вы б апісалі гэта так:

```html {5,8}
<label>
  Пароль:
  <input
    type="password"
    aria-describedby="password-hint"
  />
</label>
<p id="password-hint">
  Пароль мусіць змяшчаць як найменш 18 сімвалаў
</p>
```

Але ўказваць ідэнтыфікатар наўпрост у кодзе не з’яўляецца добрай практыкай у React. Кампанент можа быць адрэндараны болей за адзін раз на старонцы, а ідэнтыфікатары маюць быць унікальнымі! Замест указання пастаяннага ідэнтыфікатара, згенеруйце ўнікальны з дапамогай `useId`:

```js {4,11,14}
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();
  return (
    <>
      <label>
        Пароль:
        <input
          type="password"
          aria-describedby={passwordHintId}
        />
      </label>
      <p id={passwordHintId}>
        Пароль мусіць змяшчаць як найменш 18 сімвалаў
      </p>
    </>
  );
}
```

Цяпер, нават калі `PasswordField` з’явіцца на старонцы некалькі раз, згенераваныя ідэнтыфікатары не будуць канфліктаваць.



```js
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();
  return (
    <>
      <label>
        Пароль:
        <input
          type="password"
          aria-describedby={passwordHintId}
        />
      </label>
      <p id={passwordHintId}>
        Пароль мусіць змяшчаць як найменш 18 сімвалаў
      </p>
    </>
  );
}

export default function App() {
  return (
    <>
      <h2>Прыдумайце пароль</h2>
      <PasswordField />
      <h2>Пацвердзіце пароль</h2>
      <PasswordField />
    </>
  );
}
```

```css
input { margin: 5px; }
```



[Азнаёмцеся з відэа](https://www.youtube.com/watch?v=0dNzNcuEuOo), каб пабачыць розніцу ў карыстанні з ужываннем дапаможных тэхналогій.



Пры [серверным рэндарынгу](/reference/react-dom/server), `**useId` патрабуе ідэнтычнага дрэва кампанентаў на серверы і на кліенце**. Калі дрэвы, што вы рэндарыце і кліенце і серверы не суадносяцца, згенераваныя ідэнтыфікатары будуць адрознымі.





#### Чым useId лепей за нарастальнага лічыльнік? {/*why-is-useid-better-than-an-incrementing-counter*/}

Вы можаце задумацца: чым `useId` лепей за нарастальную глабальную пераменную накшталт `nextId++`.

Асноўная перавага `useId` у тым, што React забяспечваю працу пры [серверным рэндарынгу](/reference/react-dom/server). Падчас сервернага рэндарынгу, з вашых кампанентаў генеруецца HTML. Потым, на кліенце, падчас [гідратацыі](/reference/react-dom/client/hydrateRoot) адбываецца прывязка апрацоўшчыкаў падзей да згенераванага HTML. Каб гідратацыя спрацавала, вынік кліента мусіць супадаць з HTML сервера.

Гэта вельмі складана гарантаваць праз нарастальны лічыльнік, бо парадак, у якім кліент робіць гідратацыю кампанентаў, можа не адпавядаць парадку, у якім сервер складае HTML. Карыстаючыся `useId`, можна гарантаваць, што гідрадацыя спрацуе, і вынік будзе аднолькавым на серверы і кліенце.

Унутры React, `useId` генеруецца на падставе размяшчэння бацькоўскага кампанента». Менавіта таму, калі дрэвы кліента і сервера ідэнтычныя, «размяшчэнне бацькоўскага кампанента» будзе адпавядаць незалежна ад парадку рэндара.



---

### Генерацыя ідэнтыфікатараў для некалькі звязаных элементаў {/*generating-ids-for-several-related-elements*/}

Калі вам трэба даць ідэнтыфікатары некалькім звязаным элементам, вы можаце выкарыстаць `useId` каб згенераваць агульны прэфікс для іх:



```js
import { useId } from 'react';

export default function Form() {
  const id = useId();
  return (
    <form>
      <label htmlFor={id + '-firstName'}>Імя:</label>
      <input id={id + '-firstName'} type="text" />
      <hr />
      <label htmlFor={id + '-lastName'}>Прозвішча:</label>
      <input id={id + '-lastName'} type="text" />
    </form>
  );
}
```

```css
input { margin: 5px; }
```



Гэта дазволіць вам пазбегнуць выклікаў `useId` для кожнага элемента, які патрабуе ўнікальны ідэнтыфікатар.

---

### Вызначэнне агульнага прэфікса для ўсіх згенераваных ідэнтыфікатараў {/*specifying-a-shared-prefix-for-all-generated-ids*/}

Калі вы рэндарыце некалькі незалежных праграм React на адной старонцы, перадайце `identifierPrefix` у опцыях да вашых выклікаў `[createRoot](/reference/react-dom/client/createRoot#parameters)` ці `[hydrateRoot](/reference/react-dom/client/hydrateRoot)`. Гэта гарантуе, што ідэнтыфікатары з дзвюх розных праграм не будуць канфліктаваць, бо кожны згенераваны праз `useId` ідэнтыфікатар будзе пачынацца з адрознага прэфікса, які вы ўказалі.



```html public/index.html
<!DOCTYPE html>
<html>
  <head><title>Мая праграма</title></head>
  <body>
    <div id="root1"></div>
    <div id="root2"></div>
  </body>
</html>
```

```js
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();
  console.log('Згенераваны ідэнтыфікатар:', passwordHintId)
  return (
    <>
      <label>
        Пароль:
        <input
          type="password"
          aria-describedby={passwordHintId}
        />
      </label>
      <p id={passwordHintId}>
        Пароль мусіць змяшчаць як найменш 18 сімвалаў
      </p>
    </>
  );
}

export default function App() {
  return (
    <>
      <h2>Прыдумайце пароль</h2>
      <PasswordField />
    </>
  );
}
```

```js src/index.js active
import { createRoot } from 'react-dom/client';
import App from './App.js';
import './styles.css';

const root1 = createRoot(document.getElementById('root1'), {
  identifierPrefix: 'my-first-app-'
});
root1.render(<App />);

const root2 = createRoot(document.getElementById('root2'), {
  identifierPrefix: 'my-second-app-'
});
root2.render(<App />);
```

```css
#root1 {
  border: 5px solid blue;
  padding: 10px;
  margin: 5px;
}

#root2 {
  border: 5px solid green;
  padding: 10px;
  margin: 5px;
}

input { margin: 5px; }
```



---

### Выкарыстанне аднолькавых прэфіксаў для ідэнтыфікатараў на кліенце і на серверы {/*using-the-same-id-prefix-on-the-client-and-the-server*/}

Калі вы [рэндарыце некалькі незалежных праграм React на адной старонцы](#specifying-a-shared-prefix-for-all-generated-ids), і некаторыя з гэтых праграм рэндарацца на серверы, пераканайцеся, што `identifierPrefix`, які вы перадаяце ў выклік `[hydrateRoot](/reference/react-dom/client/hydrateRoot)` на кліенце, ідэнтычны `identifierPrefix`, які вы перадаяце праз [API сервера](/reference/react-dom/server), такія як `[renderToPipeableStream`.](/reference/react-dom/server/renderToPipeableStream)

```js
// Server
import { renderToPipeableStream } from 'react-dom/server';

const { pipe } = renderToPipeableStream(
  <App />,
  { identifierPrefix: 'react-app1' }
);
```

```js
// Client
import { hydrateRoot } from 'react-dom/client';

const domNode = document.getElementById('root');
const root = hydrateRoot(
  domNode,
  reactNode,
  { identifierPrefix: 'react-app1' }
);
```

У `identifierPrefix` няма патрэбы, калі вы маеце толькі адну праграму React на старонцы.
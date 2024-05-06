---
title: Выкарыстанне TypeScript
re: https://github.com/reactjs/react.dev/issues/5960
---

<Intro>

TypeScript — папулярны спосаб дадаць тыпізацыю ў ваш JavaScript код. TypeScript [падтрымлівае JSX](/learn/writing-markup-with-jsx) з каробкі, вы можаце дадаць паўнавартасную падтрымку React для вэб, дадаўшы да праекта [`@types/react`](https://www.npmjs.com/package/@types/react) і [`@types/react-dom`](https://www.npmjs.com/package/@types/react-dom).

</Intro>

<YouWillLearn>

* [TypeScript з кампанентамі React](/learn/typescript#typescript-with-react-components)
* [Прыклад тыпізацыі хукаў](/learn/typescript#example-hooks)
* [Агульныя тыпы з `@types/react`](/learn/typescript/#useful-types)
* [Іншыя рэсурсы для вывучэння](/learn/typescript/#further-learning)

</YouWillLearn>

## Усталёўка {/*installation*/}

Усе [React фрэймворкі, гатовыя для выкарыстання ў працоўным асяроддзі](/learn/start-a-new-react-project#production-grade-react-frameworks) прапануюць падтрымку TypeScript. Для кожнага фрэймворка трымайцеся ягоных уласных інструкцый:

- [Next.js](https://nextjs.org/docs/app/building-your-application/configuring/typescript)
- [Remix](https://remix.run/docs/en/1.19.2/guides/typescript)
- [Gatsby](https://www.gatsbyjs.com/docs/how-to/custom-configuration/typescript/)
- [Expo](https://docs.expo.dev/guides/typescript/)

### Дадаванне TypeScript у існуючы праект React {/*adding-typescript-to-an-existing-react-project*/}

Для ўсталявання апошняй версіі тыпаў React выкарыстоўвайце:

<TerminalBlock>
npm install @types/react @types/react-dom
</TerminalBlock>

Ваш `tsconfig.json` мае змяшчаць наступныя налады:

1. `dom` мае быць уключанае ў [`lib`](https://www.typescriptlang.org/tsconfig/#lib) (Заўвага: калі значэнне для `lib` не зададзенае, `dom` прадвызначана уключанае).
1. [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) мае мець любое валіднае значэнне. `preserve` мае падыходзіць для большасці выпадкаў.
  Калі вы публікуеце бібліятэку, звярніцеся да [дакументацыі па полю `jsx`](https://www.typescriptlang.org/tsconfig/#jsx) каб падабраць правільнае значэнне.

## TypeScript з кампанентамі React {/*typescript-with-react-components*/}

<Note>

Усе файлы, што змяшчаюць JSX, маюць мець пашырэнне `.tsx`. Гэта спецыфічнае для TypeScript пашырэнне, якое кажа TypeScript, што файл змяшчае JSX.

</Note>

Напісанне TypeScript з React вельмі падобнае да напісання JavaScript з React. Асноўнае адрозненне ў тым, што падчас працы з кампанентамі вы зможаце ўказаць тыпы пропсаў. Гэтыя тыпы могуць быць скарыстаныя для праверкі правільнасці і прадастаўлення ўнутранай дакументацыі для рэдактараў кода.

Узяўшы [кампанент `MyButton`](/learn#components) са старонкі «[Хуткі старт](/learn)», мы можам дадаць апісанне тыпу для пропса `title` кнопкі:

<Sandpack>

```tsx src/App.tsx active
function MyButton({ title }: { title: string }) {
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Вітаю ў маёй праграме</h1>
      <MyButton title="Я — кнопка" />
    </div>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```
</Sandpack>

 <Note>

Дадзеныя пясочніцы ўмеюць працаваць з TypeScript кодам, але яны не робяць праверку тыпаў. Гэта значыць, вы можаце ўносіць змены ў TypeScript пясочніцы дзеля вывучэння, але вы не будзеце бачыць памылкі тыпізацыі ці папярэджанні. Для праверкі тыпізацыі, можаце выкарыстоўваць [тэставую пляцоўку TypeScript](https://www.typescriptlang.org/play) ці выкарыстоўваць анлайн-пясочніцы, што маюць больш магчымасцяў.

</Note>

Дадзены аднарадковы сінтаксіс — найпрасцейшы спосаб тыпізацыі кампанента, але з павелічэннем колькасці пропсаў, дадзеная канструкцыя пачне станавіцца грузнай. Замест гэтага можна выкарыстоўваць `interface` ці `type` для апісання пропсаў кампанента:

<Sandpack>

```tsx src/App.tsx active
interface MyButtonProps {
  /** Тэкст, які будзе паказвацца ў кнопцы */
  title: string;
  /** Задае, ці можна ўзаемадзейнічаць з кнопкай */
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return (
    <button disabled={disabled}>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Вітаю ў маёй праграме</h1>
      <MyButton title="Я — адключаная кнопка" disabled={true}/>
    </div>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

Тып, якім вы апісваеце пропсы кампанента, можа быць на столькі простым ці складаным, на колькі вам тое патрэбна, але гэта заўсёды мае быць апісанне тыпу аб’екта праз `type` ці `interface`. Вы можаце даведацца больш аб тым, як апісваць аб’екты ў TypeScript на старонцы «[Тыпы аб’ектаў](https://www.typescriptlang.org/docs/handbook/2/objects.html)». Таксама вас могуць зацікавіць [аб’яднаныя тыпы](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) для апісання пропсаў, якія могуць адпавядаць аднаму з некалькіх тыпаў, ці інструкцыі як [ствараць тыпы на аснове іншых тыпаў](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) для больш складаных выпадкаў.


## Прыклады хукаў {/*example-hooks*/}

Апісанні тыпаў у `@types/react` уключае таксама і тыпізацыю ўбудаваных хукаў, таму вы можаце выкарыстоўваць іх у сваіх кампанентах без дадатковай налады. Яны пабудаваныя такім чынам, каб улічваць код, ужо напісаны ў кампаненце, такім чынам вы атрымаеце [аўтаматычна вызначаныя тыпы](https://www.typescriptlang.org/docs/handbook/type-inference.html) і ў большасці выпадкаў вам не давядзецца разбірацца з дробнай тыпізацыяй самастойна. 

Але далей мы разгледзім некалькі прыкладаў, як тыпізаваць хукі.

### `useState` {/*typing-usestate*/}

[Хук `useState`](/reference/react/useState) будзе выкарыстоўваць значэнне, зададзенае пры ініцыалізацыі, для аўтаматычнага вызначэння тыпу. Напрыклад:

```ts
// Тут будзе вызначаны тып boolean
const [enabled, setEnabled] = useState(false);
```

<<<<<<< HEAD
Для `enabled` будзе вызначаны тып `boolean`, а `setEnabled` будзе прымаць або `boolean`, або функцыю, якая вяртае `boolean`. Калі вы хочаце відавочна перадаць тып для вашага стану, вы можаце задаць яго пры выкліку `useState`:
=======
This will assign the type of `boolean` to `enabled`, and `setEnabled` will be a function accepting either a `boolean` argument, or a function that returns a `boolean`. If you want to explicitly provide a type for the state, you can do so by providing a type argument to the `useState` call:
>>>>>>> 556063bdce0ed00f29824bc628f79dac0a4be9f4

```ts 
// Тып відавочна зададзены як boolean
const [enabled, setEnabled] = useState<boolean>(false);
```

Гэта не надта карысна ў дадзеным выпадку, але вам можа спатрэбіцца падобнае, калі вы маеце аб’яднаны тып. Напрыклад, тут статус можа прымаць толькі пэўныя радковыя значэнні:

```ts
type Status = "idle" | "loading" | "success" | "error";

const [status, setStatus] = useState<Status>("idle");
```

Ці, як рэкамендавана ў [прынцыпах структуравання коду](/learn/choosing-the-state-structure#principles-for-structuring-state), вы можаце групаваць звязаныя станы ў аб’екты і апісваць іх як розныя магчымыя тыпы аб’екта:

```ts
type RequestState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success', data: any }
  | { status: 'error', error: Error };

const [requestState, setRequestState] = useState<RequestState>({ status: 'idle' });
```

### `useReducer` {/*typing-usereducer*/}

[Хук `useReducer`](/reference/react/useReducer) — больш скаладаны хук, які прымае функцыю рэд’юсара і першапачатковы стан. Аўтаматычна тып будзе вызначаны па тыпу значэння стану. Таксама вы можаце ўказаць тып стану пры выкліку `useReducer`, але звычайна лепей пакінуць першапачатковы стан:

<Sandpack>

```tsx src/App.tsx active
import {useReducer} from 'react';

interface State {
   count: number 
};

type CounterAction =
  | { type: "reset" }
  | { type: "setCount"; value: State["count"] }

const initialState: State = { count: 0 };

function stateReducer(state: State, action: CounterAction): State {
  switch (action.type) {
    case "reset":
      return initialState;
    case "setCount":
      return { ...state, count: action.value };
    default:
      throw new Error("Unknown action");
  }
}

export default function App() {
  const [state, dispatch] = useReducer(stateReducer, initialState);

  const addFive = () => dispatch({ type: "setCount", value: state.count + 5 });
  const reset = () => dispatch({ type: "reset" });

  return (
    <div>
      <h1>Вітаю на старонцы свайго лічыльніка</h1>

      <p>Колькасць: {state.count}</p>
      <button onClick={addFive}>Дадаць 5</button>
      <button onClick={reset}>Скінуць</button>
    </div>
  );
}

```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>


Мы выкарыстоўваем TypeScript у некаторых ключавых момантах:

 - `interface State` апісвае стан рэд’юсара.
 - `type CounterAction` апісвае розныя дзеянні, якія могуць быць адпраўленыя рэд’юсару.
 - `const initialState: State` задае тып першапачатковага стану, а таксама тып, які будзе выкарыстоўвацца ў `useReducer`.
 - `stateReducer(state: State, action: CounterAction): State` задае тып для функцыі рэд’юсара і значэння, якое будзе з яе вяртацца.

Больш відавочным спосабам задаць тып стану `initialState` будзе перадаць яго непасрэдна `useReducer`:

```ts
import { stateReducer, State } from './your-reducer-implementation';

const initialState = { count: 0 };

export default function App() {
  const [state, dispatch] = useReducer<State>(stateReducer, initialState);
}
```

### `useContext` {/*typing-usecontext*/}

[Хук `useContext`](/reference/react/useContext) — тэхніка для перадачы даных кампанентам уніз па дрэве без неабходнасці перадаваць пропсы цераз кампаненты. Выкарыстоўваецца шляхам стварэння кампанента-правайдара і хука для атрымання даных у даччыных кампанентах.

Аўтаматычна тып будзе вызначаны згодна з тыпам даных, што былі перададзеныя пры выкліку `createContext`:

<Sandpack>

```tsx src/App.tsx active
import { createContext, useContext, useState } from 'react';

type Theme = "light" | "dark" | "system";
const ThemeContext = createContext<Theme>("system");

const useGetTheme = () => useContext(ThemeContext);

export default function MyApp() {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  )
}

function MyComponent() {
  const theme = useGetTheme();

  return (
    <div>
      <p>Цяперашняя тэма: {theme}</p>
    </div>
  )
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

Дадзеная тэхніка працуе калі маецца прадвызначанае значэнне, якое мае сэнс, але бываюць сітуацыі, калі такога няма. У такім выпадку разумна будзе скарыстацца `null` у якасці прадвызначанага значэння. Але каб сістэме тыпізацыі было зразумела, трэба відавочна ўказаць тып `ContextShape | null` для `createContext`.

Разам з тым з’яўляецца патрэба выключаць `| null` пры атрыманні значэння. Мы раім дадаць у хук праверку, якая будзе падчас выканання правяраць наяўнасць значэння і выкідваць памылку пры яго адсутнасці:

```js {5, 16-20}
import { createContext, useContext, useState, useMemo } from 'react';

// Гэта просты прыклад, але вы можаце прыдумаць больш складаны аб’ект
type ComplexObject = {
  kind: string
};

// Кантэкст створаны з ужываннем `| null`, каб дакладна адлюстроўваць прадвызначанае значэнне
const Context = createContext<ComplexObject | null>(null);

// `| null` будзе выключаны пасля таго, як спрацуе хук.
const useGetComplexObject = () => {
  const object = useContext(Context);
  if (!object) { throw new Error("useGetComplexObject must be used within a Provider") }
  return object;
}

export default function MyApp() {
  const object = useMemo(() => ({ kind: "complex" }), []);

  return (
    <Context.Provider value={object}>
      <MyComponent />
    </Context.Provider>
  )
}

function MyComponent() {
  const object = useGetComplexObject();

  return (
    <div>
      <p>Бягучы аб’ект: {object.kind}</p>
    </div>
  )
}
```

### `useMemo` {/*typing-usememo*/}

[Хук `useMemo`](/reference/react/useMemo) дапамагае ствараць і паўторна атрымліваць доступ да запомненых вынікаў запуску функцыі. Функцыя будзе выконвацца наноў толькі ў выпадках, калі зменіцца залежнае значэнне, перададзенае другім параметрам. Вынік выкліку функцыі будзе аўтаматычна тыпізаваны адпаведна функцыі, што была перададзеная першым параметрам. Вы можаце тыпізаваць хук відавочна, перадаўшы яму тып.

```ts
// Тып visibleTodos вызначаны аўтаматычна з тыпу значэння, што вяртае функцыя filterTodos
const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
```


### `useCallback` {/*typing-usecallback*/}

[Хук `useCallback`](/reference/react/useCallback) забяспечвае стабільную спасылку на функцыю пакуль залежнасці, перададзеныя другім параметрам, не змяняюцца. Як і `useMemo`, тып функцыі вызначаецца па тыпе функцыі, што перададзеная першым параметрам, але болей відавочна ўказаць тып можна перадаўшы яго ў хук.


```ts
const handleClick = useCallback(() => {
  // ...
}, [todos]);
```

Пры працы з TypeScript у строгім рэжыме, `useCallback` патрабуе дадатковай тыпізацыі параметраў функцыі. Гэта таму што тып функцыі вызначаны па тыпе вяртаемага значэння і не можа быць цалкам зразумелым.

У залежнасці ад вашых пераваг у стылі кода, вы можаце выкарыстоўваць функцыі `*EventHandler` з тыпаў React для забеспячэння тыпізацыі апрацоўшчыкаў падзей прама падчас аб’яўлення: 

```ts
import { useState, useCallback } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  const handleChange = useCallback<React.ChangeEventHandler<HTMLInputElement>>((event) => {
    setValue(event.currentTarget.value);
  }, [setValue])
  
  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Значэнне: {value}</p>
    </>
  );
}
```

## Карысныя тыпы {/*useful-types*/}

Пакет `@types/react` змяшчае даволі вялізны набор тыпаў, якія вартыя вывучэння, калі ўзаемадзеянне паміж React і TypeScript стане камфортным для вас. Вы можаце знайсці іх [у папцы React праекта DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts). Тут мы разгледзім некаторыя тыпы, якія найчасцей бываюць патрэбнымі.

### Падзеі DOM {/*typing-dom-events*/}

Калі працуеце з падзеямі DOM у React, тып падзеі ў апрацоўшчыку падзеі можа быць вызначаны аўтаматычна. Але ж, калі вы хочаце вынесці функцыю-апрацоўшчык вонкі, вам давядзецца відавочна ўказаць тып падзеі.

<Sandpack>

```tsx src/App.tsx active
import { useState } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
    setValue(event.currentTarget.value);
  }

  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Значэнне: {value}</p>
    </>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

React таксама прадастаўляе шмат тыпаў падзей. Поўны іх спіс можна знайсці [тут](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/b580df54c0819ec9df62b0835a315dd48b8594a9/types/react/index.d.ts#L1247C1-L1373), ён змяшчае [самыя папулярныя падзеі DOM](https://developer.mozilla.org/en-US/docs/Web/Events).

Калі вызначаецеся, які тып вы шукаеце, вам можа дапамагчы інфармацыя, што паказваецца пры навядзенні на апрацоўшчык падзеі, што вы выкарыстоўваеце, яна пакажа тып вашай падзеі.

Калі вам трэба скарыстаць падзею, якая не ўключаная ў спіс, вы можаце скарыстацца тыпам `React.SyntheticEvent`, які з’яўляецца базавым тыпам для ўсіх падзей.

### Даччыныя элементы {/*typing-children*/}

Існуюць два распаўсюджаныя шляхі апісання даччыных элементаў. Першы — выкарыстоўваць тып `React.ReactNode`, які ўключае ў сябе ўсе магчымыя тыпы, якія можа перадаць у JSX у якасці даччынага элемента:

```ts
interface ModalRendererProps {
  title: string;
  children: React.ReactNode;
}
```

Гэта вельмі шырокае вызначэнне даччыных элементаў. Іншы шлях — выкарыстоўваць тып `React.ReactElement`, якому адпавядаюць толькі элементы JSX, выключаючы прымітывы JavaScript, такія як радкі ці лічбы:

```ts
interface ModalRendererProps {
  title: string;
  children: React.ReactElement;
}
```

Заўважце, што немагчыма выкарыстоўваць TypeScript для апісання нейкага пэўнага элемента JSX. То-бок вы не можаце выкарыстоўваць сістэму тыпізацыі каб апісаць кампанент, які прымае, напрыклад, толькі даччыныя элементы `<li>`.

Вы можаце пабачыць прыклады абодвух тыпаў `React.ReactNode` і `React.ReactElement` з праверкай тыпаў на [гэтай тэставай пляцоўцы TypeScript](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgIilQ3wChSB6CxYmAOmXRgDkIATJOdNJMGAZzgwAFpxAR+8YADswAVwGkZMJFEzpOjDKw4AFHGEEBvUnDhphwADZsi0gFw0mDWjqQBuUgF9yaCNMlENzgAXjgACjADfkctFnYkfQhDAEpQgD44AB42YAA3dKMo5P46C2tbJGkvLIpcgt9-QLi3AEEwMFCItJDMrPTTbIQ3dKywdIB5aU4kKyQQKpha8drhhIGzLLWODbNs3b3s8YAxKBQAcwXpAThMaGWDvbH0gFloGbmrgQfBzYpd1YjQZbEYARkB6zMwO2SHSAAlZlYIBCdtCRkZpHIrFYahQYQD8UYYFA5EhcfjyGYqHAXnJAsIUHlOOUbHYhMIIHJzsI0Qk4P9SLUBuRqXEXEwAKKfRZcNA8PiCfxWACecAAUgBlAAacFm80W-CU11U6h4TgwUv11yShjgJjMLMqDnN9Dilq+nh8pD8AXgCHdMrCkWisVoAet0R6fXqhWKhjKllZVVxMcavpd4Zg7U6Qaj+2hmdG4zeRF10uu-Aeq0LBfLMEe-V+T2L7zLVu+FBWLdLeq+lc7DYFf39deFVOotMCACNOCh1dq219a+30uC8YWoZsRyuEdjkevR8uvoVMdjyTWt4WiSSydXD4NqZP4AymeZE072ZzuUeZQKheQgA).

### Пропсы стыляў {/*typing-style-props*/}

Калі вы выкарыстоўваеце ўбудаваныя стылі ў React, вы можаце выкарыстоўваць `React.CSSProperties` для апісання аб’екта, перадаваемага ў пропс `style`. Дадзены тып уключае ўсе магчымыя CSS параметры і з’яўляецца добрым спосабам пераканацца, што вы перадаяце правільны пропс `style`, і атрымаць аўтадапаўненне ў рэдактары кода.

```ts
interface MyComponentProps {
  style: React.CSSProperties;
}
```

## Далейшае вывучэнне {/*further-learning*/}

Дадзеная старонка тлумачыць базавае выкарыстанне TypeScript з React, але ёсць яшчэ шмат, што вартае вывучэння.
Асобныя старонкі дакументацыі API могуць змяшчаць больш дэтальную дакументацыю, як выкарыстоўваць іх з TypeScript.

Мы раім наступныя рэсурсы:

 - [The TypeScript handbook](https://www.typescriptlang.org/docs/handbook/) — афіцыйная дакументацыя TypeScript, якая тлумачыць большасць асноўных функцый мовы.

 - [The TypeScript release notes](https://devblogs.microsoft.com/typescript/) падрабязна тлумачыць кожную новую функцыю.

 - [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/) — мадэруемая супольнасцю шпаргалка па выкарыстанні TypeScript з React, што тлумачыць шмат карысных выпадкаў і забяспечвае ахоп большы за гэтую старонку.

 - [TypeScript супольнасць у Discord](https://discord.com/invite/typescript) — выдатнае месца, каб задаць пытанні і атрымаць дапамогу па пытаннях  TypeScript і React.

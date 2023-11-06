---
<<<<<<< HEAD
title: "Убудаваныя ў React хукі"
---

<Intro>

*Хукі* дазваляюць выкарыстоўваць вам некаторыя магчымасці React у вашых кампанентах. Вы можаце як выкарыстоўваць убудаваныя хукі, так і камбінаваць іх у свае ўласныя. Дадзеная старонка пералічвае ўбудаваныя ў React хукі.

=======
title: React Reference Overview
---

<Intro>
This section provides detailed reference documentation for working with React. 
For an introduction to React, please visit the [Learn](/learn) section. 
>>>>>>> a8790ca810c1cebd114db35a433b90eb223dbb04
</Intro>

Our The React reference documentation is broken down into functional subsections: 

<<<<<<< HEAD
## Хукі стану {/*state-hooks*/}

*Стан* дазваляе кампаненту [«запамінаць» даныя, напрыклад, уведзеныя карыстальнікам](/learn/state-a-components-memory). Напрыклад, кампанент з формай можа выкарыстоўваць стан для захавання ўведзеных значэнняў, у той час як кампанент з галерэяй відарысаў можа выкарыстоўваць стан для захавання бягучага выбранага відарыса.

Каб дадаць стан у кампанент, выкарыстоўвайце адзін з дадзеных хукаў:

* [`useState`](/reference/react/useState) задае пераменную стану, якую вы можаце абнаўляць напрамую.
* [`useReducer`](/reference/react/useReducer) задае пераменную стану, логіка абнаўленне якой апісваецца праз [функцыю рэд’юсара](/learn/extracting-state-logic-into-a-reducer).

```js
function ImageGallery() {
  const [index, setIndex] = useState(0);
  // ...
```

---

## Хукі кантэксту {/*context-hooks*/}

*Кантэкст* дазваляе кампаненту [атрымліваць інфармацыю ад далёкіх бацькоўскіх кампанентаў, не перадаючы яе ў якасці пропсаў](/learn/passing-props-to-a-component). Напрыклад, ваш кампанент верхняга ўзроўню можа перадаваць бягучую тэма інтэрфейсу ўсім даччыным элементам на любым узроўні ўкладзенасці.

* [`useContext`](/reference/react/useContext) чытае і падпісваецца на абнаўленні кантэксту.

```js
function Button() {
  const theme = useContext(ThemeContext);
  // ...
```

---

## Хукі рэфаў {/*ref-hooks*/}

*Рэфы* дазваляюць кампаненту [ўтрымліваць інфармацыю, якая не выкарыстоўваецца падчас рэндэрынгу](/learn/referencing-values-with-refs), напрыклад: вузлы DOM або ідэнтыфікатары тайм-аўтаў. У адрозненні ад стану, абнаўленне ref не прыводзіць на перарэндэрынгу кампанента. Рэфы — своеасаблівы «пралаз», каб выбрацца з парадыгмы React. Яны карысныя, калі даводзіцца працаваць з не React сістэмамі, напрыклад з API браўзера.

* [`useRef`](/reference/react/useRef) аб’яўляе рэф. Вы можаце захоўваць у ім любыя даныя, але ў большасці выпадкаў у рэфах захоўваюць вузлы DOM.
* [`useImperativeHandle`](/reference/react/useImperativeHandle) дазваляе наладзіць рэф вашага кампанента. Выкарыстоўваецца рэдка.

```js
function Form() {
  const inputRef = useRef(null);
  // ...
```

---

## Хукі эфектаў {/*effect-hooks*/}

*Эфекты* дазваляюць кампаненту [падключацца і сінхранізавацца са знешнімі сістэмамі](/learn/synchronizing-with-effects). Гэта ўключае ў сябе працу з сеткай, DOM браўзера, анімацыямі, віджэтамі, напісанымі з дапамогай іншых бібліятэк для пабудовы інтэрфейсаў, ці любым іншым не React кодам.

* [`useEffect`](/reference/react/useEffect) падключае кампанент да знешняй сістэмы.

```js
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
  // ...
```

Эфекты — «пралазы», што дазваляюць выбрацца з парадыгмы React. Не выкарыстоўвайце Эфекты для арганізацыі патокаў даных унутры праграмы. Калі вы не ўзаемадзейнічаеце са знешняй сістэмай, [імаверна, вам не патрэбны эфект.](/learn/you-might-not-need-an-effect)

Існуюць таксама два іншыя варыянты `useEffect`, якія спрацоўваюць у іншы час, яны выкарыстоўваюцца не так часта:

* [`useLayoutEffect`](/reference/react/useLayoutEffect) спрацоўвае перад тым, як браўзер абновіць экран. Можна выкарыстоўваць для вымярэнняў элементаў старонкі.
* [`useInsertionEffect`](/reference/react/useInsertionEffect) спрацоўвае перад тым, як React абновіць DOM. Бібліятэкі могуць выкарыстоўваць яго, каб дынамічна падстаўляць CSS.

---

## Хукі прадукцыйнасці {/*performance-hooks*/}

Для аптымізацыі прадукцыйнасці пры перарэндэрынгу часта прапускаюць выкананне непатрэбнай працы. Напрыклад, вы можаце сказаць React выкарыстоўваць кэшаваныя вынікі вылічэнняў або прапусціць перарэндэрынг, калі даныя не змяніліся з моманту папярэдняга рэндэрынгу.

Каб прапусціць вылічэнні і непатрэбны перарэндэрынг, выкарыстоўвайце наступныя хукі:

- [`useMemo`](/reference/react/useMemo) дазваляе кэшаваць вынік складаных вылічэнняў.
- [`useCallback`](/reference/react/useCallback) дазваляе кэшаваць аб’яўленую функцыю перад тым, як перадаць яе далей у аптымізаваны кампанент.

```js
function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```

Часам бывае, што нельга прапусціць перарэндэрынг, бо экран мае быць абноўленым. У такім выпадку, вы можаце палепшыць прадукцыйнасць, раздзяліўшы сінхронныя абнаўленні, што блакіруюць карыстальніцкі інтэрфейс, (напрыклад, набор тэксту) і абнаўленні, якія не павінны яго блакіраваць (напрыклад, абнаўленне графіка).  

Для прыярытэтызацыі рэндэрынгу, выкарыстоўвайце адзін з наступных хукаў:

- [`useTransition`](/reference/react/useTransition) дазваляе пазначыць, што пераход стану не блакіруе паток і дазваляе іншым абнаўленням перарываць яго.
- [`useDeferredValue`](/reference/react/useDeferredValue) дазваляе адкласці абнаўленне некрытычных часткак інтэрфейсу на карысць іншых.

---

## Хукі рэсурсаў {/*resource-hooks*/}

*Рэсурсы* — штосьці, да чаго кампаненты могуць атрымаць доступ, не маючы іх як частку ўласнага стану. Напрыклад, кампанент можа прачытаць паведамленне з Promise або інфармацыю пра тэму з кантэксту.

Для атрымання даных з рэсурсу, выкарыстоўвайце наступны хук:

- [`use`](/reference/react/use) дазваляе атрымліваць значэнні з такіх рэсурсаў, як [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) або [кантэкст](/learn/passing-data-deeply-with-context).

```js
function MessageComponent({ messagePromise }) {
  const message = use(messagePromise);
  const theme = use(ThemeContext);
  // ...
}
```

---

## Іншыя хукі {/*other-hooks*/}

Гэтыя хукі болей карысныя аўтарам бібліятэк і не часта выкарыстоўваюцца ў кодзе праграм. 

- [`useDebugValue`](/reference/react/useDebugValue) дазваляе наладзіць пазнаку, якую React DevTools будзе паказваць для вашага хука.
- [`useId`](/reference/react/useId) дазваляе кампаненту атрымаць унікальны ідэнтыфікатар. Звычайна выкарыстоўваецца з API спецыяльных магчымасцяў.
- [`useSyncExternalStore`](/reference/react/useSyncExternalStore) дазваляе кампаненту падпісацца на знешняе сховішча.

---

## Уласныя хукі {/*your-own-hooks*/}

Вы таксама можаце [аб’яўляць свае самастойна створаныя хукі](/learn/reusing-logic-with-custom-hooks#extracting-your-own-custom-hook-from-a-component) як JavaScript функцыі.
=======
## React {/*react*/}
Programmatic React features:  
* [Hooks](/reference/react/hooks) - Use different React features from your components.
* [Components](/reference/react/components) - Documents built-in components that you can use in your JSX.
* [APIs](/reference/react/apis) - APIs that are useful for defining components. 
* [Directives](/reference/react/directives) - Provide instructions to bundlers compatible with React Server Components.

## React DOM {/*react-dom*/}
React-dom contains features that are only supported for web applications 
(which run in the browser DOM environment). This section is broken into the following:

* [Hooks](/reference/react-dom/hooks) - Hooks for web applications which run in the browser DOM environment.
* [Components](/reference/react-dom/components) - React supports all of the browser built-in HTML and SVG components.
* [APIs](/reference/react-dom) - The `react-dom` package contains methods supported only in web applications.
* [Client APIs](/reference/react-dom/client) - The `react-dom/client` APIs let you render React components on the client (in the browser). 
* [Server APIs](/reference/react-dom/server) - The `react-dom/server` APIs let you render React components to HTML on the server.

## Legacy APIs {/*legacy-apis*/}
* [Legacy APIs](/reference/react/legacy) - Exported from the react package, but not recommended for use in newly written code. 
>>>>>>> a8790ca810c1cebd114db35a433b90eb223dbb04

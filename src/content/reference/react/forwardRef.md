---
title: forwardRef
---

<Deprecated>

In React 19, `forwardRef` is no longer necessary. Pass `ref` as a prop instead.

`forwardRef` will be deprecated in a future release. Learn more [here](/blog/2024/04/25/react-19#ref-as-a-prop).

</Deprecated>

<Intro>

<<<<<<< HEAD
`forwardRef` дазваляе кампаненту раскрыць вузел DOM для бацькоўскага кампанента праз [ref.](/learn/manipulating-the-dom-with-refs)
=======
`forwardRef` lets your component expose a DOM node to the parent component with a [ref.](/learn/manipulating-the-dom-with-refs)
>>>>>>> 0d05d9b6ef0f115ec0b96a2726ab0699a9ebafe1

```js
const SomeComponent = forwardRef(render)
```

</Intro>

<InlineToc />

---

## Даведка {/*reference*/}

### `forwardRef(render)` {/*forwardref*/}

Выклічце `forwardRef()` каб ваш кампанент змог атрымаць рэф і перадаць яго даччынаму:

```js
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  // ...
});
```

[Болей прыкладаў глядзіце ніжэй.](#usage)

#### Параметры {/*parameters*/}

* `render`: Функцыя рэндэрынгу вашага кампанента. React выклікае дадзеную функцыю, перадаючы пропсы і `ref`, перададзены ў ваш кампанент з бацькоўскага. JSX, які вяртае функцыя, будзе разметкай вашага кампанента.

#### Значэнні, якія вяртаюцца {/*returns*/}

`forwardRef` вяртае кампанент React, які можа быць адрэндэраны праз JSX. У адрозненні ад кампанентаў, вызначаных з дапамогай звычайных функцый, кампанент, вызначаны праз `forwardRef`, здольны таксама прымаць пропс `ref`.

#### Перасцярогі {/*caveats*/}

* У Строгім рэжыме, React будзе **выклікаць функцыю рэндэрынгу двойчы** каб [дапамагчы выявіць магчымыя пабочныя эфекты](/reference/react/useState#my-initializer-or-updater-function-runs-twice). Такія паводзіны адбываюцца толькі ў асяроддзі для распрацоўкі і ніяк не ўплываюць на працоўнае. Калі функцыя вашага кампанента чыстая (якой яна мае быць), гэта не павінна ўплываць на логіку кампанента. Вынік аднаго з выклікаў будзе праігнараваны.

---

### Функцыя `render` {/*render-function*/}

`forwardRef` прымае функцыю рэндэрынгу як аргумент. React выклікае гэтую функцыю, перадаючы `props` і `ref`:

```js
const MyInput = forwardRef(function MyInput(props, ref) {
  return (
    <label>
      {props.label}
      <input ref={ref} />
    </label>
  );
});
```

#### Параметры {/*render-parameters*/}

* `props`: Пропсы, перададзеныя з бацькоўскага кампанента.

* `ref`:  Атрыбут `ref`, перададзены з бацькоўскага кампанента. `ref` можа быць аб’ектам ці функцыяй. Калі бацькоўскі кампанент не перадаў рэф, значэннем будзе `null`. Вы маеце перадаць атрыманы `ref` у іншы кампанент ці ў [`useImperativeHandle`.](/reference/react/useImperativeHandle)

#### Значэнні, якія вяртаюцца {/*render-returns*/}

`forwardRef` вяртае кампанент React, які можа быць адрэндэраны праз JSX. У адрозненні ад кампанентаў, вызначаных з дапамогай звычайных функцый, кампанент, вызначаны праз `forwardRef` здольны таксама прымаць пропс `ref`.

---

## Выкарыстанне {/*usage*/}

### Раскрыццё вузла DOM кампанента бацькоўскаму {/*exposing-a-dom-node-to-the-parent-component*/}

Першапачаткова вузлы DOM кожнага з кампанентаў прыватныя. Але ж, часам бывае карысна адкрыць вузел для бацькоўскага кампанента. Напрыклад, каб дазволіць сфакусіравацца на ім. Каб гэта зрабіць, абгарніце вызначэнне вашага кампанента ў `forwardRef()`:

```js {3,11}
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} />
    </label>
  );
});
```

Вы атрымаеце <CodeStep step={1}>ref</CodeStep> другім аргументам пасля пропсаў. Перадайце яго ў вузел DOM, які вы хочаце раскрыць:

```js {8} [[1, 3, "ref"], [1, 8, "ref", 30]]
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});
```

Гэта дазволіць бацькоўскаму кампаненту `Form` атрымаць доступ да <CodeStep step={2}>вузла DOM `<input>`</CodeStep> раскрытага ў `MyInput`:

```js [[1, 2, "ref"], [1, 10, "ref", 41], [2, 5, "ref.current"]]
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Увядзіце вашае імя:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Рэдагаваць
      </button>
    </form>
  );
}
```

Дадзены кампанент `Form` [задае рэф](/reference/react/useRef#manipulating-the-dom-with-a-ref) для `MyInput`. Кампанент `MyInput` *перанакіроўвае* гэты рэф да тэга `<input>`. У выніку кампанент `Form` можа атрымаць доступ да вузла DOM `<input>` і выклікаць ягоны метад [`focus()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus).

Памятайце, што раскрыццё рэфа вузла DOM унутры кампанента ўскладняе змену ўнутраных часцей кампанента пазней. Звычайна вам можа спатрэбіцца раскрыццё вузлоў DOM для нізкаўзроўневых кампанентаў, якія вы будзеце перавыкарыстоўваць, такіх як кнопкі ці палі ўводу тэксту, але не для болей высоўкаўзроўневых, такіх як аватары ці каментарыі.

<Recipes titleText="Прыклады перанакіроўвання рэфа">

#### Факусіраванне на полі ўводу тэксту {/*focusing-a-text-input*/}

Націсканне кнопкі прывядзе да факусіравання на полі ўводу. Кампанент `Form` вызначае рэф і перадае яго ў кампанент `MyInput`. Кампанент `MyInput` перанакіроўвае гэты рэф да элемента `<input>`. Гэта дазваляе кампаненту `Form` зрабіць факусіраванне на `<input>`.

<Sandpack>

```js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Увядзіце вашае імя:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Рэдагаваць
      </button>
    </form>
  );
}
```

```js src/MyInput.js
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

export default MyInput;
```

```css
input {
  margin: 5px;
}
```

</Sandpack>

<Solution />

#### Прайграванне і прыпыненне відэа {/*playing-and-pausing-a-video*/}

Націсканне кнопак будзе выклікаць метады [`play()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play) і [`pause()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/pause) вузла DOM `<video>`. Кампанент `App` вызначае рэф і перадае кампаненту `MyVideoPlayer`. Кампанент `MyVideoPlayer`перанакіроўвае гэты рэф да вузла `<video>`. Гэта дазваляе кампаненту `App` прайграваць і прыпыняць `<video>`.

<Sandpack>

```js
import { useRef } from 'react';
import MyVideoPlayer from './MyVideoPlayer.js';

export default function App() {
  const ref = useRef(null);
  return (
    <>
      <button onClick={() => ref.current.play()}>
        Прайграць
      </button>
      <button onClick={() => ref.current.pause()}>
        Прыпыніць
      </button>
      <br />
      <MyVideoPlayer
        ref={ref}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
        type="video/mp4"
        width="250"
      />
    </>
  );
}
```

```js src/MyVideoPlayer.js
import { forwardRef } from 'react';

const VideoPlayer = forwardRef(function VideoPlayer({ src, type, width }, ref) {
  return (
    <video width={width} ref={ref}>
      <source
        src={src}
        type={type}
      />
    </video>
  );
});

export default VideoPlayer;
```

```css
button { margin-bottom: 10px; margin-right: 10px; }
```

</Sandpack>

<Solution />

</Recipes>

---

### Перанакіроўванне рэфа цераз некалькі кампанентаў {/*forwarding-a-ref-through-multiple-components*/}

Замест таго, каб перанакіроўваць `ref` у вузел DOM, вы можаце перанакіраваць яго ва ўласны кампанент, напрыклад `MyInput`:

```js {1,5}
const FormField = forwardRef(function FormField(props, ref) {
  // ...
  return (
    <>
      <MyInput ref={ref} />
      ...
    </>
  );
});
```

Калі кампанент `MyInput` перанакіроўвае рэф у ягоны `<input>`, рэф ад `FormField` таксама будзе змяшчаць той `<input>`:

```js {2,5,10}
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <FormField label="Увядзіце вашае імя:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Рэдагаваць
      </button>
    </form>
  );
}
```

Кампанент `Form` вызначае рэф і перадае яго ў `FormField`. Кампанент `FormField` перанакіроўвае рэф у кампанент `MyInput`, які ў сваю чаргу перанакіроўвае рэф у вузел DOM `<input>`. Такім чынам `Form` атрымлівае доступ да дадзенага вузла DOM.


<Sandpack>

```js
import { useRef } from 'react';
import FormField from './FormField.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <FormField label="Увядзіце вашае імя:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Рэдагаваць
      </button>
    </form>
  );
}
```

```js src/FormField.js
import { forwardRef, useState } from 'react';
import MyInput from './MyInput.js';

const FormField = forwardRef(function FormField({ label, isRequired }, ref) {
  const [value, setValue] = useState('');
  return (
    <>
      <MyInput
        ref={ref}
        label={label}
        value={value}
        onChange={e => setValue(e.target.value)} 
      />
      {(isRequired && value === '') &&
        <i>Абавязкова</i>
      }
    </>
  );
});

export default FormField;
```


```js src/MyInput.js
import { forwardRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

export default MyInput;
```

```css
input, button {
  margin: 5px;
}
```

</Sandpack>

---

### Раскрыццё загаднага кіравання замест вузла DOM {/*exposing-an-imperative-handle-instead-of-a-dom-node*/}

Замест таго, каб раскрываць вузел DOM цалкам, вы можаце раскрыць уласны аб’ект з абмежаваным наборам метадаў, які называюць *загадным кіраваннем*. Каб гэта зрабіць, спатрэбіцца вызначыць асобны рэф, у якім трымаць вузел DOM:

```js {2,6}
const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  // ...

  return <input {...props} ref={inputRef} />;
});
```

Перадайце атрыманы `ref` у [`useImperativeHandle`](/reference/react/useImperativeHandle) і вызначце значэнні, якія вы хочаце раскрыць у `ref`:

```js {6-15}
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

Калі які-небудзь кампанент перадасць рэф у `MyInput`, ён атрымае толькі ваш аб’ект `{ focus, scrollIntoView }` замест вузла DOM. Гэта дазваляе абмяжоўваць інфармацыю, якую вы хочаце раскрыць пра ваш вузел DOM, да мінімуму.

<Sandpack>

```js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
    // Гэты код не спрацуе, бо вузел DOM не раскрыты:
    // ref.current.style.opacity = 0.5;
  }

  return (
    <form>
      <MyInput placeholder="Увядзіце вашае імя" ref={ref} />
      <button type="button" onClick={handleClick}>
        Рэдагаваць
      </button>
    </form>
  );
}
```

```js src/MyInput.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});

export default MyInput;
```

```css
input {
  margin: 5px;
}
```

</Sandpack>

[Даведайцеся больш пра выкарыстанне загаднага кіравання.](/reference/react/useImperativeHandle)

<Pitfall>

**Не выкарыстоўвайце рэфы болей, чым трэба.** Варта выкарыстоўваць рэфы толькі для загаднага кіравання, якое нельга зрабіць праз пропсы: напрыклад, каб прагартаць да вузла, сфакусіравацца на ім, пачаць анімацыю, вылучыць тэкст і г.д.

**Калі штосьці можна выразіць пропсамі, не варта выкарыстоўваць для гэтага рэфы.** Напрыклад, замест раскрыцця падобнага загаднага кіравання: `{ open, close }` з кампанента `Modal`, лепей прымаць `isOpen` як пропс, напрыклад: `<Modal isOpen={isOpen} />`. [Эфекты](/learn/synchronizing-with-effects) могуць дапамагчы раскрыць загаднае кіраванне ў выглядзе пропсаў.

</Pitfall>

---

## Дыягностыка непаладак {/*troubleshooting*/}

### Мой кампанент абгорнуты ў `forwardRef`, але `ref` заўсёды `null` {/*my-component-is-wrapped-in-forwardref-but-the-ref-to-it-is-always-null*/}

Звычайна гэта азначае, што вы забылі скарыстаць `ref`, які вы атрымалі.

Напрыклад, вось гэты кампанент нічога не робіць з атрыманым `ref`:

```js {1}
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input />
    </label>
  );
});
```

Каб гэта вырашыць, перадайце `ref` далей вузлу DOM ці іншаму кампаненту, што прымае рэф:

```js {1,5}
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input ref={ref} />
    </label>
  );
});
```

`ref` у `MyInput` таксама можа быць `null` калі сустракаецца ўмоўная логіка:

```js {1,5}
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      {showInput && <input ref={ref} />}
    </label>
  );
});
```

Калі значэнне `showInput` роўнае `false`, рэф не будзе перададзены ніводнаму вузлу, і рэф `MyInput` застанецца пустым. Падобнае лёгка прапусціць, калі ўмова схаваная ўнутры іншага кампанента, як у гэтым кампаненце `Panel` у прыкладзе:

```js {5,7}
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      <Panel isExpanded={showInput}>
        <input ref={ref} />
      </Panel>
    </label>
  );
});
```

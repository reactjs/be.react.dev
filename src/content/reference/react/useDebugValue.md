---
title: useDebugValue
---

<Intro>

`useDebugValue` — хук React, які дазваляе дадаць пазнаку да ўласнага хука ў [React DevTools.](/learn/react-developer-tools)

```js
useDebugValue(value, format?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `useDebugValue(value, format?)` {/*usedebugvalue*/}

Выклічце `useDebugValue` на верхнім узроўні вашага [ўласнага хука](/learn/reusing-logic-with-custom-hooks) каб паказаць прыдатнае да чытання значэнне:

```js
import { useDebugValue } from 'react';

function useOnlineStatus() {
  // ...
  useDebugValue(isOnline ? 'Online' : 'Offline');
  // ...
}
```

[Болей прыкладаў глядзіце ніжэй.](#usage)

#### Параметры {/*parameters*/}

* `value` — значэнне, якое вы хочаце паказваць у React DevTools. Яно можа мець любы тып.
* **(неабавязковае)** `format` — функцыя фармаціравання. Пры даследаванні элемента, React DevTools выкліча функцыю фармаціравання, перадаўшы ў яе `value` у якасці аргумента, і пакажа адфармаціраванае значэнне, якое верне функцыя (яна прымае значэнне любога тыпу). Калі не задаваць функцыю фармаціравання, будзе паказана арыгінальнае значэнне `value`.

#### Значэнні, якія вяртаюцца {/*returns*/}

`useDebugValue` нічога не вяртае.

## Выкарыстанне {/*usage*/}

### Дадаванне пазнакі да ўласнага хука {/*adding-a-label-to-a-custom-hook*/}

Выклічце `useDebugValue` на верхнім узроўні вашага [ўласнага хука](/learn/reusing-logic-with-custom-hooks) каб паказаць прыдатнае да чытання <CodeStep step={1}>значэнне для адладкі</CodeStep> для [React DevTools](/learn/react-developer-tools).

```js [[1, 5, "isOnline ? 'Online' : 'Offline'"]]
import { useDebugValue } from 'react';

function useOnlineStatus() {
  // ...
  useDebugValue(isOnline ? 'Online' : 'Offline');
  // ...
}
```

Гэта задасць кампаненту, які выклікае `useOnlineStatus`, пазнаку `OnlineStatus: "Online"` пры даследаванні яго:

![Здымак экрана React DevTools, у якім паказваецца значэнне для адладкі](/images/docs/react-devtools-usedebugvalue.png)

Без выкліку `useDebugValue` будзе паказана толькі асноўнае значэнне (у дадзеным прыкладзе — `true`).

<Sandpack>

```js
import { useOnlineStatus } from './useOnlineStatus.js';

function StatusBar() {
  const isOnline = useOnlineStatus();
  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}

export default function App() {
  return <StatusBar />;
}
```

```js useOnlineStatus.js active
import { useSyncExternalStore, useDebugValue } from 'react';

export function useOnlineStatus() {
  const isOnline = useSyncExternalStore(subscribe, () => navigator.onLine, () => true);
  useDebugValue(isOnline ? 'Online' : 'Offline');
  return isOnline;
}

function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);
  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  };
}
```

</Sandpack>

<Note>

Не дадавайце значэнні для адладкі кожнаму хуку. Іх карысна выкарыстоўваць у хуках, што з'яўляюцца часткай агульнай бібліятэкі і ў хуках са складанай унутранай структурай даных, якую можа быць складана даследаваць.

</Note>

---

### Адтэрміноўка фармаціравання значэння для адладкі {/*deferring-formatting-of-a-debug-value*/}

Вы заўсёды можаце перадаць функцыю фармаціравання ў якасці другога аргумента `useDebugValue`:

```js [[1, 1, "date", 18], [2, 1, "date.toDateString()"]]
useDebugValue(date, date => date.toDateString());
```

Вашая функцыя фармаціравання атрымае <CodeStep step={1}>значэнне для адладкі</CodeStep> ў якасці параметра і мае вярнуць <CodeStep step={2}>фармаціраванае значэнне</CodeStep>. Калі вы дасладуеце свой кампанент, React DevTools выклікае функцыю і паказвае вынік выканання.

Гэта дазваляе пазбегнуць выканання патэнцыйна складанай логікі фармаціравання да таго часу, як кампанент будзе даследавацца. Напрыклад, калі `date` мае тып Date, гэта пазбягае выканання `toDateString()` падчас кожнага перарэндэрынгу.

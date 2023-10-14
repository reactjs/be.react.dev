---
title: useDebugValue
---

<Intro>

`useDebugValue` — Хук React, які дазваляе дадаць цэтлік да ўласнага хука ў [React DevTools.](/learn/react-developer-tools)

```js
useDebugValue(value, format?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `useDebugValue(value, format?)` {/*usedebugvalue*/}

Выклічце `useDebugValue` на верхнім узроўні вашага [ўласнага хука](/learn/reusing-logic-with-custom-hooks) каб адабразіць прыдатнае да чытання значэнне:

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
* **(неабавязковае)** `format` — функцыя фарматавання. Пры даследванні элемента, React DevTools выкліча функцыю фарматавання, перадаўшы ў яе `value` у якасці аргумента, і пакажа адфарматаванае значэнне, якое верне функцыя (яна прымае значэнне любога тыпу). Калі не задаваць функцыю фарматавання, будзе паказане арыгінальнае значэнне `value`.

#### Вяртае {/*returns*/}

`useDebugValue` не вяртае нічога.

## Выкарыстанне {/*usage*/}

### Дадаванне цэтліка да ўласнага хука {/*adding-a-label-to-a-custom-hook*/}

Выклічце `useDebugValue` на верхнім узроўні вашага [ўласнагахука](/learn/reusing-logic-with-custom-hooks) каб адабразіць прыдатнае да чытання <CodeStep step={1}>значэнне для адладкі</CodeStep> для [React DevTools](/learn/react-developer-tools).

```js [[1, 5, "isOnline ? 'Online' : 'Offline'"]]
import { useDebugValue } from 'react';

function useOnlineStatus() {
  // ...
  useDebugValue(isOnline ? 'Online' : 'Offline');
  // ...
}
```

Гэта задасць кампаненту, які выклікае `useOnlineStatus` цэтлік `OnlineStatus: "Online"` пры даследванні яго:

![Скрыншот React DevTools, у якім паказваецца значэнне для адладкі](/images/docs/react-devtools-usedebugvalue.png)

Без выкліку `useDebugValue` толькі асноўнае значэнне будзе паказанае (у дадзеным прыкладзе, гэта было б `true`).

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

Не дадавайце значэнні для адладкі кожнаму хуку. Гэта найбольш патрэбна для тых хукаў, якія з’яўляюцца часткай агульнай бібліятэкі і маюць складаную ўнутраную структуру даных, якую можа быць складана даследваць.

</Note>

---

### Адтэрміноўка фарматавання значэння для адладкі {/*deferring-formatting-of-a-debug-value*/}

Вы заўсёды можаце перадаць функцыю фарматавання ў якасці другога аргумента `useDebugValue`:

```js [[1, 1, "date", 18], [2, 1, "date.toDateString()"]]
useDebugValue(date, date => date.toDateString());
```

Вашая функцыя фарматавання атрымае <CodeStep step={1}>значэнне для адладкі</CodeStep> ў якасці параметра і мае вярнуць <CodeStep step={2}>фарматаванае значэнне</CodeStep>. Калі вы дасладуеце свой кампанент, React DevTools выклікае функцыю і паказвае вынік выканання.

Гэта дазваляе пазбегнуць выканання патэнцыйна складанай логікі да таго часу, як кампанент будзе даследвацца. Напрыклад, калі `date` — экзэмпляр Date, гэта пазбягае выканання `toDateString()` падчас кожнага перарэндэрынгу.

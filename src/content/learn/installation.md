---
title: Усталяванне
---

<Intro>

React быў распрацаваны для паступовага ўкаранення. Вы можаце выкарыстоўваць столькі React, колькі вам трэба. Калі вы хочаце паспрабаваць React, дадаць трохі інтэрактыўнасці HTML старонцы або стварыць складаную праграму на React, гэты раздзел дапаможа вам прыступіць да працы.

</Intro>

## Паспрабуйце React {/*try-react*/}

Вам не трэба нічога ўсталёўваць, каб паспрабаваць React. Паспрабуйце рэдагаваць код у пясочніцы!

<Sandpack>

```js
function Greeting({ name }) {
  return <h1>Вітаю, {name}</h1>;
}

export default function App() {
  return <Greeting name="свет" />
}
```

</Sandpack>

Вы можаце рэдагаваць код прама тут, або адкрыць у новай укладцы, націснуўшы кнопку «Fork» у правым верхнім куце.

Большасць старонак дакументацыі React утрымліваюць пясочніцы падобныя гэтай. Па-за дакументацыяй React існуюць і іншыя анлайн-пясочніцы, якія падтрымліваюць React: напрыклад, [CodeSandbox](https://codesandbox.io/s/new), [StackBlitz](https://stackblitz.com/fork/react), або [CodePen.](https://codepen.io/pen?template=QWYVwWN)

Каб паспрабаваць React лакальна на вашым камп’ютары, [спампуйце гэтую HTML старонку.](https://gist.githubusercontent.com/gaearon/0275b1e1518599bbeafcde4722e79ed1/raw/db72dcbf3384ee1708c4a07d3be79860db04bff0/example.html) Адкрыйце яе ў сваім тэкставым рэдактары і браўзеры!

## Стварыць новы React праект {/*creating-a-react-app*/}

Калі вы хочаце стварыць React праграму, [стварыце новы React праект](/learn/creating-a-react-app) з дапамогай рэкамендаваных фрэймворкаў.

## Стварыць React праект з нуля {/*build-a-react-app-from-scratch*/}

Калі ніводзін з фрэймворкаў не падыходзіць для вашага праекта або вы хочаце стварыць новы, або вы хочацце вывучыць асновы React, то вы можаце [стварыць React праект з нуля](/learn/build-a-react-app-from-scratch).

## Дадаць React у існуючы праект {/*add-react-to-an-existing-project*/}

Калі вы хочаце выкарыстоўваць React у існуючых праграмах або вэб-сайтах, [дадайце React у існуючы праект.](/learn/add-react-to-an-existing-project)


<Note>

#### Ці варта мне выкарыстоўваць Create React App? {/*should-i-use-create-react-app*/}

Не. Create React App цяпер лічыцца састарэлым. Больш інфармацыі даступна на старонцы [Канец Create React App](/blog/2025/02/14/sunsetting-create-react-app).

</Note>

## Наступныя крокі {/*next-steps*/}

Азнаёмцеся са старонкай «[Хуткі старт](/learn)», каб пазнаёміцца з найбольш важнымі канцэпцыямі React, якімі вы будзеце карыстацца кожны дзень.

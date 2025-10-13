title: Агляд даведкі па React
---

<Intro>
  
У гэтым раздзеле змяшчаецца падрабязная даведачная дакументацыя па працы з React. 
Каб пазнаёміцца з React, наведайце раздзел [Навучанне](/learn).

</Intro>

Даведачная дакументацыя па React разбіта на функцыянальныя падраздзелы:

## React {/*react*/}

Праграмныя функцыі React:

* [Хукі](/reference/react/hooks) - Выкарыстоўвайце розныя функцыі React прама з вашых кампанентаў.
* [Кампаненты](/reference/react/components) - Апісвае ўбудаваныя кампаненты, якія можна выкарыстоўваць у вашым JSX.
* [API](/reference/react/apis) - Карысныя API для аб'яўлення кампанентаў.
* [Дырэктывы](/reference/react/directives) - Інструкцыі для зборшчыкаў сумяшчальных з сервернымі кампанентамі React.

## React DOM {/*react-dom*/}

React-dom змяшчае функцыі, якія падтрымліваюцца толькі для вэб-праграм (якія працуюць у DOM асяроддзі браўзера). Гэты раздзел разбіты на наступныя часткі:

* [Хукі](/reference/react-dom/hooks) - Хукі для вэб-праграм, якія працуюць у DOM асяроддзі браўзера.
* [Кампаненты](/reference/react-dom/components) - React падтрымлівае ўсе кампаненты HTML і SVG, убудаваныя ў браўзер.
* [API](/reference/react-dom) - Пакет `react-dom` змяшчае метады, якія падтрымліваюцца толькі ў вэб-праграмах.
* [Кліенцкія API](/reference/react-dom/client) - API з пакета `react-dom/client` дазваляюць рэндэрыць кампаненты React на кліенце (у браўзеры).
* [Серверныя API](/reference/react-dom/server) - API з пакета `react-dom/server` дазваляюць рэндэрыць кампаненты React у HTML на серверы.

<<<<<<< HEAD
## Правілы React {/*rules-of-react*/}
=======
## React Compiler {/*react-compiler*/}

The React Compiler is a build-time optimization tool that automatically memoizes your React components and values:

* [Configuration](/reference/react-compiler/configuration) - Configuration options for React Compiler.
* [Directives](/reference/react-compiler/directives) - Function-level directives to control compilation.
* [Compiling Libraries](/reference/react-compiler/compiling-libraries) - Guide for shipping pre-compiled library code.

## ESLint Plugin React Hooks {/*eslint-plugin-react-hooks*/}

The [ESLint plugin for React Hooks](/reference/eslint-plugin-react-hooks) helps enforce the Rules of React:

* [Lints](/reference/eslint-plugin-react-hooks) - Detailed documentation for each lint with examples.

## Rules of React {/*rules-of-react*/}
>>>>>>> 0d05d9b6ef0f115ec0b96a2726ab0699a9ebafe1

У React ёсць ідыёмы (або правілы) для выражэння патэрнаў такім чынам, каб іх было лёгка зразумець і атрымаць якасныя праграмы:

* [Кампаненты і хукі павінны быць чыстымі](/reference/rules/components-and-hooks-must-be-pure) — Чысціня спрашчае разуменне і адладку вашага кода і дазваляе React аўтаматычна і правільна аптымізаваць вашыя кампаненты і хукі.
* [React выклікае кампаненты і хукі](/reference/rules/react-calls-components-and-hooks) – React адказвае за рэндэрынг кампанентаў і хукаў, калі гэта неабходна для аптымізацыі карыстальніцкага вопыту.
* [Правілы хукаў](/reference/rules/rules-of-hooks) – Хукі вызначаюцца з дапамогай JavaScript функцый, але яны ўяўляюць сабой адмысловы тып UI логікі прыдатнай да паўторнага выкарыстання з абмежаваннямі на тое, дзе яны могуць быць выкліканыя.

## Устарэлыя API {/*legacy-apis*/}

* [Устарэлыя API](/reference/react/legacy) - API экспартаваныя з пакета React, але не рэкамендуюцца для выкарыстання ў нядаўна напісаным кодзе.

title: Агляд даведкі па React
---

<Intro>
  
У гэтым раздзеле змяшчаецца падрабязная даведачная дакументацыя па працы з React. 
Каб пазнаёміцца з React, наведайце раздзел [Навучанне](/learn).

</Intro>

Даведачная дакументацыя па React разбіта на функцыянальныя падраздзелы:

## React {/*react*/}

Праграмныя функцыі React:

<<<<<<< HEAD
* [Хукі](/reference/react/hooks) - Выкарыстоўвайце розныя функцыі React прама з вашых кампанентаў.
* [Кампаненты](/reference/react/components) - Апісвае ўбудаваныя кампаненты, якія можна выкарыстоўваць у вашым JSX.
* [API](/reference/react/apis) - карысныя API для аб'яўлення кампанентаў.
* [Дырэктывы](/reference/react/directives) - Пазначце інструкцыі для зборшчыкаў сумяшчальных з сервернымі кампанентамі React.
=======
* [Hooks](/reference/react/hooks) - Use different React features from your components.
* [Components](/reference/react/components) - Documents built-in components that you can use in your JSX.
* [APIs](/reference/react/apis) - APIs that are useful for defining components.
* [Directives](/reference/rsc/directives) - Provide instructions to bundlers compatible with React Server Components.
>>>>>>> 9e1f5cd590fd066e72dda9022237bee30b499951

## React DOM {/*react-dom*/}

React-dom змяшчае функцыі, якія падтрымліваюцца толькі для вэб-праграм (якія працуюць у DOM асяроддзі браўзера). Гэты раздзел разбіты на наступныя часткі:

* [Хукі](/reference/react-dom/hooks) - Хукі для вэб-праграм, якія працуюць у DOM асяроддзі браўзера.
* [Кампаненты](/reference/react-dom/components) - React падтрымлівае ўсе кампаненты HTML і SVG, убудаваныя ў браўзер.
* [API](/reference/react-dom) - Пакет `react-dom` змяшчае метады, якія падтрымліваюцца толькі ў вэб-праграмах.
* [Кліенцкія API](/reference/react-dom/client) - API з пакета `react-dom/client` дазваляюць рэндэрыць кампаненты React на кліенце (у браўзеры).
* [Серверныя API](/reference/react-dom/server) - API з пакета `react-dom/server` дазваляюць рэндэрыць кампаненты React у HTML на серверы.

## Правілы React {/*rules-of-react*/}

У React ёсць ідыёмы (або правілы) для выражэння патэрнаў такім чынам, каб іх было лёгка зразумець і атрымаць якасныя праграмы:

* [Кампаненты і хукі павінны быць чыстымі](/reference/rules/components-and-hooks-must-be-pure) — Чысціня спрашчае разуменне і адладку вашага кода і дазваляе React аўтаматычна і правільна аптымізаваць вашыя кампаненты і хукі.
* [React выклікае кампаненты і хукі](/reference/rules/react-calls-components-and-hooks) – React адказвае за рэндэрынг кампанентаў і хукаў, калі гэта неабходна для аптымізацыі карыстальніцкага вопыту.
* [Правілы хукаў](/reference/rules/rules-of-hooks) – Хукі вызначаюцца з дапамогай JavaScript функцый, але яны ўяўляюць сабой адмысловы тып UI логікі прыдатнай да паўторнага выкарыстання з абмежаваннямі на тое, дзе яны могуць быць выкліканыя.

## Устарэлыя API {/*legacy-apis*/}

* [Устарэлыя API](/reference/react/legacy) - API экспартаваныя з пакета React, але не рэкамендуюцца для выкарыстання ў нядаўна напісаным кодзе.

---
title: Пачаць новы React праект
---

<Intro>

<<<<<<< HEAD
Калі вы хочаце стварыць новую праграму або новы вэб-сайт цалкам на React, мы рэкамендуем выбраць адзін з фрэймворкаў на базе React, папулярных у супольнасці. Фрэймворкі забяспечваюць функцыі, якія патрэбны большасці праграм і сайтаў, уключаючы маршрутызацыю, выбарку даных і генерацыю HTML.
=======
If you want to build a new app or a new website fully with React, we recommend picking one of the React-powered frameworks popular in the community.
>>>>>>> 2372ecf920ac4cda7c900f9ac7f9c0cd4284f281

</Intro>


<<<<<<< HEAD
**Вам трэба ўсталяваць [Node.js](https://nodejs.org/en/) для лакальнага разгортвання.** Вы можаце *таксама* выкарыстоўваць Node.js у вытворчасці, але гэта неабавязкова. Многія React фрэймворкі падтрымліваюць экспарт у статычны HTML/CSS/JS.
=======
You can use React without a framework, however we’ve found that most apps and sites eventually build solutions to common problems such as code-splitting, routing, data fetching, and generating HTML. These problems are common to all UI libraries, not just React.
>>>>>>> 2372ecf920ac4cda7c900f9ac7f9c0cd4284f281

By starting with a framework, you can get started with React quickly, and avoid essentially building your own framework later.

<DeepDive>

#### Can I use React without a framework? {/*can-i-use-react-without-a-framework*/}

You can definitely use React without a framework--that's how you'd [use React for a part of your page.](/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page) **However, if you're building a new app or a site fully with React, we recommend using a framework.**

Here's why.

Even if you don't need routing or data fetching at first, you'll likely want to add some libraries for them. As your JavaScript bundle grows with every new feature, you might have to figure out how to split code for every route individually. As your data fetching needs get more complex, you are likely to encounter server-client network waterfalls that make your app feel very slow. As your audience includes more users with poor network conditions and low-end devices, you might need to generate HTML from your components to display content early--either on the server, or during the build time. Changing your setup to run some of your code on the server or during the build can be very tricky.

**These problems are not React-specific. This is why Svelte has SvelteKit, Vue has Nuxt, and so on.** To solve these problems on your own, you'll need to integrate your bundler with your router and with your data fetching library. It's not hard to get an initial setup working, but there are a lot of subtleties involved in making an app that loads quickly even as it grows over time. You'll want to send down the minimal amount of app code but do so in a single client–server roundtrip, in parallel with any data required for the page. You'll likely want the page to be interactive before your JavaScript code even runs, to support progressive enhancement. You may want to generate a folder of fully static HTML files for your marketing pages that can be hosted anywhere and still work with JavaScript disabled. Building these capabilities yourself takes real work.

**React frameworks on this page solve problems like these by default, with no extra work from your side.** They let you start very lean and then scale your app with your needs. Each React framework has a community, so finding answers to questions and upgrading tooling is easier. Frameworks also give structure to your code, helping you and others retain context and skills between different projects. Conversely, with a custom setup it's easier to get stuck on unsupported dependency versions, and you'll essentially end up creating your own framework—albeit one with no community or upgrade path (and if it's anything like the ones we've made in the past, more haphazardly designed).

If your app has unusual constraints not served well by these frameworks, or you prefer to solve these problems yourself, you can roll your own custom setup with React. Grab `react` and `react-dom` from npm, set up your custom build process with a bundler like [Vite](https://vitejs.dev/) or [Parcel](https://parceljs.org/), and add other tools as you need them for routing, static generation or server-side rendering, and more.

</DeepDive>

## React фрэймворкі, гатовыя для выкарыстання ў працоўным асяроддзі {/*production-grade-react-frameworks*/}

These frameworks support all the features you need to deploy and scale your app in production and are working towards supporting our [full-stack architecture vision](#which-features-make-up-the-react-teams-full-stack-architecture-vision). All of the frameworks we recommend are open source with active communities for support, and can be deployed to your own server or a hosting provider. If you’re a framework author interested in being included on this list, [please let us know](https://github.com/reactjs/react.dev/issues/new?assignees=&labels=type%3A+framework&projects=&template=3-framework.yml&title=%5BFramework%5D%3A+).

<<<<<<< HEAD
**[Next.js](https://nextjs.org/) — гэта ўніверсальны фулстэк React фрэймворк.** З яго дапамогай вы можаце ствараць сайты любога памеру ад простага статычнага блога да складанага дынамічнага сайта. Каб стварыць новы Next.js праект, запусціце ў вашым тэрмінале:
=======
### Next.js {/*nextjs-pages-router*/}

**[Next.js' Pages Router](https://nextjs.org/) is a full-stack React framework.** It's versatile and lets you create React apps of any size--from a mostly static blog to a complex dynamic application. To create a new Next.js project, run in your terminal:
>>>>>>> 2372ecf920ac4cda7c900f9ac7f9c0cd4284f281

<TerminalBlock>
npx create-next-app@latest
</TerminalBlock>

Для знаёмства з Next.js вы можаце азнаёміцца з іх [курсам па Next.js.](https://nextjs.org/learn)

Next.js падтрымліваецца [Vercel](https://vercel.com/). Вы можаце [разгарнуць Next.js](https://nextjs.org/docs/app/building-your-application/deploying) на любым бессерверным хостынгу, Node.js хостынгу або на вашым уласным серверы. Next.js таксама падтрымлівае [экспарт у статычныя файлы](https://nextjs.org/docs/pages/building-your-application/deploying/static-exports), якія не патрабуюць хостынгу.

### Remix {/*remix*/}

**[Remix](https://remix.run/) — гэта фулстэк React фрэймворк з укладзенай маршрутызацыяй.** Ён дазваляе разбіваць вашу праграму на ўкладзеныя часткі, якія могуць загружаць даныя паралельна і абнаўляць у адказ на дзеянні карыстальніка. Каб стварыць новы Remix праект, запусціце:

<TerminalBlock>
npx create-remix
</TerminalBlock>

Для знаёмства з Remix вы можаце азнаёміцца з іх інструкцыямі па [стварэнні блога](https://remix.run/docs/en/main/tutorials/blog) (кароткая) і [праграмы](https://remix.run/docs/en/main/tutorials/jokes) (доўгая).

Remix падтрымліваецца [Shopify](https://www.shopify.com/). Калі вы ствараеце Remix праект, вам трэба [выбраць шаблон для разгортвання праграмы](https://remix.run/docs/en/main/guides/deployment). Вы можаце разгарнуць Remix праграму на любым бессерверным хостынгу або Node.js хостынгу, выкарыстоўваючы або напісаўшы [адаптар](https://remix.run/docs/en/main/other-api/adapter).

### Gatsby {/*gatsby*/}

**[Gatsby](https://www.gatsbyjs.com/) — гэта React фрэймворк для стварэння хуткіх сайтаў з падтрымкай CMS.** Багатая экасістэма плагінаў і слой даных GraphQL спрашчаюць інтэграцыю змесціва, API і сэрвісаў на адным вэб-сайце. Каб стварыць новы Gatsby праект, запусціце:

<TerminalBlock>
npx create-gatsby
</TerminalBlock>

Для хуткага знаёмства з Gatsby, азнаёмцеся з яго [інструкцыяй](https://www.gatsbyjs.com/docs/tutorial/)

Gatsby падтрымліваецца [Netlify](https://www.netlify.com/). Вы можаце [разгарнуць цалкам статычны Gatsby сайт](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting) на любым статычным хостынгу. Калі вы жадаеце выкарыстоўваць толькі серверныя магчымасці Gatsby, пераканайцеся, што ваш хостынг іх падтрымлівае.

### Expo (для натыўных праграм) {/*expo*/}

**[Expo](https://expo.dev/) — гэта React фрэймворк, які дазваляе ствараць універсальныя праграмы для Android, iOS і вэб-праграмы з натыўным карыстальніцкім інтэрфейсам.** Ён пастаўляецца разам з SDK для [React Native](https://reactnative.dev/), што палягчае распрацоўку натыўных частак. Каб стварыць новы Expo праект, запусціце:

<TerminalBlock>
npx create-expo-app
</TerminalBlock>

Для знаёмства з Expo вы можаце азнаёміцца з іх [інструкцыя Expo](https://docs.expo.dev/tutorial/introduction/).

Expo падтрымліваецца [Expo (кампанія)](https://expo.dev/about). Вы можаце бясплатна ствараць праграмы з дапамогай Expo і дадаваць іх у крамы Google і Apple без абмежаванняў. Дадаткова Expo прапануе платныя воблачныя паслугі.

<<<<<<< HEAD
<DeepDive>

#### Ці можна выкарыстоўваць React без фрэймворка? {/*can-i-use-react-without-a-framework*/}

React дакладна можна выкарыстоўваць без фрэймворка. Напрыклад, вы можаце [выкарыстоўваць React толькі для пэўнай часткі старонкі.](/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page) **Аднак, калі вы ствараеце новую праграму або сайт цалкам з React, мы рэкамендуем выкарыстоўваць фрэймворк.**

Вось чаму:

Нават калі спачатку вам не патрэбна маршрутызацыя або атрыманне даных, вы, верагодна, захочаце пазней дадаць некаторыя бібліятэкі для гэтага. С кожнай новай функцыяй памер вашага JavaScript кода будзе расці і вам давядзецца задумацца аб тым, як падзяляць код для розных маршрутаў. Па меры таго, як ваша праграма будзе атрымліваць усё больш даных, вы можаце сутыкнуцца з каскаднымі запытамі, якія запаволяць вашу праграму. Сярод вашых карыстальнікаў з'явяцца тыя, хто карыстаецца нізкахуткасным інтэрнэтам або старымі прыладамі, і вы захочаце генерыраваць HTML на серверы або падчас зборкі. Змяніць налады вялікага праекту так, каб запускаць код на серверы або падчас зборкі, можа аказацца складанай задачай.

**Гэтыя праблемы не з'яўляюцца спецыфічнымі для React. Вось чаму ў Svelte ёсць SvelteKit, у Vue ёсць Nuxt і гэтак далей.** Каб іх вырашыць, вам давядзецца інтэграваць ваш зборшчык з абранымі бібліятэкамі для маршрутызацыі і атрымання даных. Зрабіць першасную наладу і прымусіць усё гэта працаваць разам можа аказацца не так складана, але існуе шмат падводных камянёў аб якіх вам прыйдзецца даведацца, каб падтрымліваць прадукцыйнасць праграмы па меры яе росту. Вы захочаце адпраўляць як мага менш кода, але зрабіць гэта за адзін раўнд запытаў паміж кліентам і серверам, пры гэтым паралельна атрымліваючы неабходныя для старонкі даныя. Верагодна, вы захочаце, каб са старонка можна было працаваць яшчэ да запуску JavaScript кода, каб падтрымліваць прагрэсіўнае паляпшэнне. Магчыма, вам спатрэбіцца дадаць папку статычных HTML файлаў для маркетынгавых старонак, якія могуць працаваць з адключаным на старонцы Javascript. Самастойнае стварэнне і падтрымка ўсіх гэтых магчымасцей патрабуе вельмі вялікай і сур'ёзнай працы.

**React фреймворкі на гэтай старонцы вырашаюць усе гэтыя праблемы і не патрабуюць ад вас дадатковых намаганняў.** Вы можаце пачаць з малога і дадаваць неабходную функцыянальнасць па меры неабходнасці. Кожны React фрэймворк мае суполку, таму знаходзіць адказы на пытанні і абнаўляць інструменты з іх дапамогай прасцей. Акрамя таго, фрэймворкі дапамагаюць структураваць ваш код і робяць яго зразумелым для іншых распрацоўшчыкаў. Правільна і адваротнае, зрабіўшы ўласнае рашэнне, ёсць рызыка захраснуць на версіі залежнасці, якая ўжо не падтрымліваецца і ў выніку стварыць свой уласны фрэймворк без суполкі і развіцця (і, хутчэй за ўсё, ён акажацца спраектаваны горш за ўжо існуючыя рашэнні ад каманд, якія прысвяцілі гэтым праблемам вялікую колькасць часу).

Калі мы вас яшчэ не пераканалі або фрэймворкі не рашаюць тую праблему, якая паўстала перад вашай праграмай і вы хочаце стварыць свой уласны фрэймворк - дзейнічайце! Усталюйце `react` і `react-dom` з npm, наладзьце свой уласны працэс зборкі з дапамогай зборшчыка, напрыклад [Vite](https://vitejs.dev/) або [Parcel](https://parceljs.org/), і па меры неабходнасці дадайце ўсе неабходныя інструменты для маршрутызацыі, статычнай генерацыі кода, сервернага рэндэрынгу і гэтак далей.
</DeepDive>

## Перадавыя React фрэймворкі {/*bleeding-edge-react-frameworks*/}
=======
## Bleeding-edge React frameworks {/*bleeding-edge-react-frameworks*/}
>>>>>>> 2372ecf920ac4cda7c900f9ac7f9c0cd4284f281

Па меры таго як мы развівалі React, мы зразумелі, што лепшая інтэграцыя з фрэймворкамі (асабліва ў пытаннях маршрутызацыі, зборкі і серверных тэхналогій) прынясе найбольшую карысць нашым карыстальнікам. Каманда Next.js пагадзілася ўзаемадзейнічаць з намі ў пошуку, распрацоўцы, інтэграцыі і тэсціраванні перадавых падыходаў, якія не залежаць ад канкрэтнага фрэймворка, напрыклад [серверныя кампаненты React.](/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023#react-server-components)

Мы працуем над тым, каб новыя магчымасці сталі прыдатнымі для працоўнага асяроддзя як мага хутчэй, і дамаўляемся з распрацоўшчыкамі зборшчыкаў і фрэймворкаў аб іх інтэграцыі. Мы разлічваем, праз год ці два ўсе пералічаныя фрэймворкі будуць поўнасцю падтрымліваць гэтыя магчымасці. (Калі вы распрацоўшчык фрэймворка і хочаце дапамагчы нам у эксперыментах, калі ласка, дайце нам ведаць!)

### Next.js (App Router) {/*nextjs-app-router*/}

**[Next.js App Router](https://nextjs.org/docs) — гэта абноўлены API Next.js, які адпавядае таму, як каманда React бачыць архітэктуру фулстэк-праграм сёння.** Ён дазваляе загружаць даныя ў асінхронных кампанентах на серверы або нават падчас зборкі.

Next.js падтрымліваецца [Vercel](https://vercel.com/). Вы можаце [разгарнуць Next.js праграму](https://nextjs.org/docs/app/building-your-application/deploying) на любым Node.js або бессерверным хостынгу, або на вашым уласным серверы. Next.js таксама падтрымлівае [статычны экспарт](https://nextjs.org/docs/app/building-your-application/deploying/static-exports), які не патрабуе сервера.

<DeepDive>

#### Якія функцыі складаюць бачанне поўнай архітэктуры React каманды? {/*which-features-make-up-the-react-teams-full-stack-architecture-vision*/}

Зборшчык Next.js App Router цалкам рэалізуе афіцыйную [спецыфікацыю серверных кампанентаў React](https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md). Гэта дазваляе змяшаць кампаненты часу зборкі, серверныя і інтэрактыўныя кампаненты ў адным дрэве React.

Напрыклад, вы можаце напісаць серверны кампанент React як `async` функцыю, якая чытае з базы даных або з файла. Затым вы можаце перадаваць даныя з яго ў інтэрактыўныя кампаненты:

```js
// Гэты кампанент працуе *толькі* на серверы (або падчас зборкі).
async function Talks({ confId }) {
  // 1. Вы знаходзіцеся на серверы, таму можаце напрамую звярнуцца да вашай базы даных без запытаў да API.
  const talks = await db.Talks.findAll({ confId });

  // 2. Дадайце любую колькасць логікі рэндэрынгу. Гэта не павялічыць памер вашага JavaScript пакета.
  const videos = talks.map(talk => talk.video);

  // 3. Перадайце даныя кампанентам, якія будуць працаваць у браўзеры.
  return <SearchableVideoList videos={videos} />;
}
```
Next.js App Router таксама падтрымлівае [атрыманне даных з затрымкай (Suspense)](/blog/2022/03/29/react-v18#suspense-in-data-frameworks). Так вы можаце задаць стан розных частак вашай праграмы пры загрузцы (напрыклад, паказаць заглушкі) непасрэдна ў вашым дрэве React:

```js
<Suspense fallback={<TalksLoading />}>
  <Talks confId={conf.id} />
</Suspense>
```

Серверныя кампаненты і затрымка (Suspense) — гэта хутчэй магчымасці React, чым Next.js. Аднак іх прыняцце на ўзроўні фрэймворка патрабуе згоды на гэта ад каманды распрацоўкі і нетрывіяльнай працы па рэалізацыі. На дадзены момант Next.js App Router з'яўляецца найбольш поўнай рэалізацыяй гэтых магчымасцей. Каманда React працуе разам з распрацоўшчыкамі зборшчыкаў, каб палегчыць рэалізацыю гэтых функцый у фрэймворках наступнага пакалення.
</DeepDive>

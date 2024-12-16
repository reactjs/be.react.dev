---
title: React Developer Tools
---

<Intro>

Для інспектавання [кампанентаў](/learn/your-first-component) React, змянення [пропсаў](/learn/passing-props-to-a-component) і [стану](/learn/state-a-components-memory), а таксама вызначэння праблем прадукцыйнасці раім да выкарыстання React Developer Tools — інструменты для распрацоўшчыкаў React.

</Intro>

<YouWillLearn>

* Як усталяваць React Developer Tools

</YouWillLearn>

## Пашырэнне для браўзера {/*browser-extension*/}

Самы лёгкі спосаб адладжвання вэб-сайтаў, пабудаваных на React, — усталяваць браўзернае пашырэнне React Developer Tools. Яно даступнае для шэрагу папулярных браўзераў:

* [Усталяваць у **Chrome**](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
* [Усталяваць у **Firefox**](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/)
* [Усталяваць у **Edge**](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil)

Цяпер, калі вы адкрыеце сайт, **які пабудаваны на React**, вы пабачыце панэлі _Components_ і _Profiler_.

![React Developer Tools extension](/images/docs/react-devtools-extension.png)

### Safari і іншыя браўзеры {/*safari-and-other-browsers*/}
Для іншых браўзераў (напрыклад, Safari) усталюйце npm пакет [`react-devtools`](https://www.npmjs.com/package/react-devtools):
```bash
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

Затым адкрыйце інструменты для распрацоўшчыка праз тэрмінал:
```bash
react-devtools
```

Цяпер падключыце свой вэб-сайт, дадаўшы дадзены тэг `<script>` унутр тэга `<head>` вашага вэб-сайта:
```html {3}
<html>
  <head>
    <script src="http://localhost:8097"></script>
```

Цяпер перазагрузіце ваш вэб-сайт у браўзеры, каб пабачыць яго ў інструментах для распрацоўшчыка.

![React Developer Tools standalone](/images/docs/react-devtools-standalone.png)

<<<<<<< HEAD
## Для мабільных праграм (React Native) {/*mobile-react-native*/}
React Developer Tools таксама можна выкарыстоўваць і для прагляду праграм, пабудаваных з дапамогай [React Native](https://reactnative.dev/).

Прасцей за ўсё карыстацца React Developer Tools будзе, усталяваўшы глабальна:
```bash
# Yarn
yarn global add react-devtools
=======
## Mobile (React Native) {/*mobile-react-native*/}

To inspect apps built with [React Native](https://reactnative.dev/), you can use [React Native DevTools](https://reactnative.dev/docs/debugging/react-native-devtools), the built-in debugger that deeply integrates React Developer Tools. All features work identically to the browser extension, including native element highlighting and selection.
>>>>>>> 3b02f828ff2a4f9d2846f077e442b8a405e2eb04

[Learn more about debugging in React Native.](https://reactnative.dev/docs/debugging)

<<<<<<< HEAD
Затым адкрыўшы праз тэрмінал:
```bash
react-devtools
```

React Developer Tools мае падключыцца да ўжо запушчанай React Native праграмы.

> Паспрабуйце перазагрузіць праграму, калі інструменты для распрацоўшчыка не падключаюцца да яе цягам некалькіх секунд.

[Даведацца больш пра адладжванне React Native.](https://reactnative.dev/docs/debugging)
=======
> For versions of React Native earlier than 0.76, please use the standalone build of React DevTools by following the [Safari and other browsers](#safari-and-other-browsers) guide above.
>>>>>>> 3b02f828ff2a4f9d2846f077e442b8a405e2eb04

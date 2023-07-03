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

## Для мабільных праграм (React Native) {/*mobile-react-native*/}
React Developer Tools таксама можна выкарыстоўваць і для прагляду праграм, пабудаваных з дапамогай [React Native](https://reactnative.dev/).

Прасцей за ўсё карыстацца React Developer Tools будзе, усталяваўшы глабальна:
```bash
# Yarn
yarn global add react-devtools

# Npm
npm install -g react-devtools
```

Затым адкрыўшы праз тэрмінал:
```bash
react-devtools
```

React Developer Tools мае падключыцца да ўжо запушчанай React Native праграмы.

> Паспрабуйце перазагрузіць праграму, калі інструменты для распрацоўшчыка не падключаюцца да яе цягам некалькіх секунд.

[Даведацца больш пра адладжванне React Native.](https://reactnative.dev/docs/debugging)

---
title: Перадача пропсаў у кампанент
---

<Intro>

Для камунікацыі між сабой кампаненты React выкарыстоўваць *пропсы*. Кожны бацькоўскі кампанент можа перадаваць некаторую інфармацыю даччыным, задаючы ім пропсы. Пропсы падобныя на атрыбуты ў HTML, але ў іх вы можаце перадаваць любыя JavaScript значэнні, уключаючы аб’екты, масівы, функцыі.

</Intro>

<YouWillLearn>

* Як перадаваць пропсы ў кампанент
* Як атрымліваць пропсы ў кампаненце
* Як задаваць прадвызначаныя значэнні для пропсаў
* Як перадаць JSX у кампанент
* Як пропсы змяняюцца з часам

</YouWillLearn>

## Знаёмыя пропсы {/*familiar-props*/}

Пропсы — гэта інфармацыя, што вы перадаяце ў JSX тэг. Напрыклад: `className`, `src`, `alt`, `width`, і `height` — некаторыя пропсы, якія можна перадаць у `<img>`:

<Sandpack>

```js
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Лінь Ланьін"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

```css
body { min-height: 120px; }
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Пропсы, якія можна перадаць у тэг `<img>` прадвызначаныя (ReactDOM адпавядае [стандарту HTML](https://www.w3.org/TR/html52/semantics-embedded-content.html#the-img-element)). Але ва *ўласныя кампаненты*, такія як `<Avatar>`, вы можаце перадаваць любыя пропсы, каб іх дапасоўваць. Вось як гэта зрабіць!

## Перадача пропсаў у кампаненты {/*passing-props-to-a-component*/}

У гэтым кодзе кампанент `Profile` не перадае аніводнага пропса свайму даччынаму кампаненту `Avatar`:

```js
export default function Profile() {
  return (
    <Avatar />
  );
}
```

Вы можаце дадаць новыя пропсы для `Avatar` у два этапы.

### Крок 1: Перадаць пропсы даччынаму кампаненту {/*step-1-pass-props-to-the-child-component*/}

Па-першае, перадайце пропсы ў `Avatar`. Напрыклад, давайце перададзім два пропсы: `person` (аб’ект) і `size` (лічба):

```js
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Лінь Ланьін', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

<Note>

Калі падвойныя фігурныя дужкі пасля `person=` вас блытаюць, напамінаем, што [гэта ўсяго толькі аб’екты](/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx) ўнутры фігурных дужак JSX.

</Note>

Цяпер вы можаце прачытаць гэтыя пропсы ў кампаненце `Avatar`.

### Крок 2: прачытайце пропсы ў даччыным кампаненце {/*step-2-read-props-inside-the-child-component*/}

Вы можаце прачытаць гэтыя пропсы, пералічыўшы іх назвы `person, size` цераз коску ўнутры `({` і `})` адразу пасля `function Avatar`. Гэта дазволіць вам выкарыстоўваць іх у Avatar, нібы яны пераменныя.

```js
function Avatar({ person, size }) {
  // person і size цяпер даступныя тут
}
```

Нарэшце, дадайце некаторую логіку да `Avatar`, скарыстаўшы пропсы `person` і `size` для рэндэрынгу.

Цяпер з дапамогай пропсаў вы можаце змяняць канфігурацыю `Avatar`, каб рэндэрыць яго па-рознаму. Паспрабуйце пагуляцца са значэннямі!

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{ 
          name: 'Кацуко Сарухасі', 
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Аклілу Лема', 
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{ 
          name: 'Лiнь Ланьін',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
body { min-height: 120px; }
.avatar { margin: 10px; border-radius: 50%; }
```

</Sandpack>

Пропсы дазваляюць вам успрымаць бацькоўскі і даччыны кампаненты незалежнымі адзін ад аднаго. Напрыклад, вы можаце змяніць пропсы `person` ці `size` унутры кампанента `Profile` нават не задумваючыся аб тым, як кампанент `Avatar` іх выкарыстоўвае. Аналагічна, вы можаце змяняць тое, як `Avatar` апрацоўвае гэтыя пропсы, не гледзячы на логіку кампанента `Profile`.

Вы можаце разглядаць пропсы як «рычажкі», якімі вы можаце рэгуляваць свой кампанент. Яны выконваюць тую ролю, што і аргументы ў функцыі. Да таго ж, пропсы — _адзіны_ аргумент, які перадаецца ў кампанент! Функцыянальны кампанент React прымае толькі адзін кампанент, а іменна аб’ект `props`:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

Звычайна вам не будзе патрэбны сам аб’ект `props`, прасцей будзе разабраць на асобныя пропсы.

<Pitfall>

**Не забывайце пра пару фігурных дужак `{` і `}`** унутры дужак `(` і `)`, калі вызначаеце пропсы:

```js
function Avatar({ person, size }) {
  // ...
}
```

дадзены сінтаксіс называецца [«дэструктурызацыяй»](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) — гэта як чытаць уласцівасці з параметра функцыі:

```js
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
```

</Pitfall>

## Прадвызначаныя значэнні для пропсаў {/*specifying-a-default-value-for-a-prop*/}

Калі вы хочаце задаць прадвызначанае значэнне, якое будзе выкарыстоўвацца, калі пропс не вызначаны, вы можаце дадаць `=` і пасля яго прадвызначанае значэнне:

```js
function Avatar({ person, size = 100 }) {
  // ...
}
```

Цяпер, калі `<Avatar person={...} />` будзе адрэндэрына без пропса `size`, ягоным значэннем будзе `100`.

Прадвызначанае значэнне будзе выкарыстана толькі калі пропс `size` адсутнічае, ці калі зададзены як `size={undefined}`. Такія варыянты, як `size={null}` ці `size={0}` **не будуць** замененыя на прадвызначанае значэнне.

## Перадача пропсаў з выкарыстаннем сінтаксіса разгортвання {/*forwarding-props-with-the-jsx-spread-syntax*/}

Часам пропсы пачынаюць шмат паўтарацца:

```js
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

Няма нічога дрэннага ў тым, каб паўтараць імёны пропсаў — такі код будзе больш зразумелым. Але магчыма з часам вам захочацца зрабіць яго лаканічней. Некаторыя кампаненты перадаюць свае пропсы даччыным, напрыклад як кампанент `Profile` перадае іх кампаненту `Avatar`. Так як кампанент не выкарыстоўвае ўласныя пропсы сам, ёсць сэнс скарыстаць больш лаканічны сінтаксіс разгортвання:

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

Гэта перадасць усе пропсы кампанента `Profile` кампаненту `Avatar` без іх пераліку спісам.

**Карыстайцеся аператарам разгортвання разумна.** Калі вы пачынаеце выкарыстоўваць яго ў кожным кампаненце, вы штосьці робіце не так. Часта гэта прыкмета таго, што варта раздзяліць кампаненты і перадаць даччыныя ў выглядзе JSX. Далей разгледзім як гэта зрабіць!

## Перадача JSX у якасці даччыных элементаў {/*passing-jsx-as-children*/}

Звычайная практыка — укладаць у стандартны набор тэгаў іншыя тэгі:

```js
<div>
  <img />
</div>
```

Часам вам можа спатрэбіцца зрабіць тое ж самае і з уласнымі кампанентамі:

```js
<Card>
  <Avatar />
</Card>
```

Калі вы ўкладаеце штосьці ў JSX тэг, бацькоўскі кампанент атрымае кантэнт у якасці пропса пад назвай `children`. Напрыклад, кампанент `Card` з прыкладу ніжэй атрымае пропс `children`, у якім будзе `<Avatar />`, і адрэндэрыць яго ўнутры div:

<Sandpack>

```js src/App.js
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Кацуко Сарухасі',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```

```js src/Avatar.js
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.card {
  width: fit-content;
  margin: 5px;
  padding: 5px;
  font-size: 20px;
  text-align: center;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.avatar {
  margin: 20px;
  border-radius: 50%;
}
```

</Sandpack>

Паспрабуйце замяніць `<Avatar>` унутры `<Card>` на які-небудзь тэкст каб паглядзець як кампанент `Card` можа працаваць з розным укладзеным кантэнтам. Яму не трэба «ведаць», што будзе адрэндэрына ўнутры яго. Падобны гібкі шаблон вы яшчэ шмат дзе пабачыце.

Паспрабуйце разгледзіць кампанент з пропсам `children` як «дзірку», якую бацькоўскі кампанент можа запоўніць разметкай JSX. Вам часта давядзецца выкарыстоўваць пропс `children` для візуальных абгортак: панэлей, сетак і г.д.

<Illustration src="/images/docs/illustrations/i_children-prop.png" alt='Плітка, падобная на пазл, са слотам «children» у які падыходзяць кавалкі «Text» і «Avatar»' />

## Як пропсы змяняюцца з часам {/*how-props-change-over-time*/}

Кампанент `Clock` ніжэй атрымлівае два пропсы ад бацькоўскага кампанента: `color` і `time` (бацькоўскі кампанент не разглядаецца, бо ён выкарыстоўвае [стан](/learn/state-a-components-memory), у падрабязнасці чаго мы яшчэ не паглыналіся).

Паспрабуйце змяніць колер у прыкладзе ніжэй:

<Sandpack>

```js src/Clock.js active
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

```js src/App.js hidden
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  const [color, setColor] = useState('lightcoral');
  return (
    <div>
      <p>
        Абярыце колер:{' '}
        <select value={color} onChange={e => setColor(e.target.value)}>
          <option value="lightcoral">светла-каралавы</option>
          <option value="midnightblue">паўночна-сіні</option>
          <option value="rebeccapurple">пурпур</option>
        </select>
      </p>
      <Clock color={color} time={time.toLocaleTimeString()} />
    </div>
  );
}
```

</Sandpack>

Дадзены прыклад адлюстроўвае, што **пропсы, якія кампанент атрымлівае, могуць змяняцца з часам**. Пропсы не заўсёды статычныя! Тут, напрыклад, пропс `time` змяняецца кожную секунду, а пропс `color` змяняецца падчас выбару іншага колеру. Пропсы змяшчаюць даныя кампанента ў канкрэтны момант, а не толькі ў момант першага рэндэру.

Не гледзячы на гэта, пропсы [нязменныя](https://en.wikipedia.org/wiki/Immutable_object) — гэты тэрмін азначае аб’ект, які не можа змяняцца пасля стварэння. Калі кампаненту трэба змяніць пропс (напрыклад, у адказ на ўзаемадзеянне з боку карыстальніка ці новыя даныя), яму давядзецца «папрасіць» бацькоўскі кампанент перадаць новы _іншы пропс_ — новаствораны аб’ект! Старыя пропсы будуць адкінутыя, і ў рэшце рэшт рухавік JavaScript выдаліць іх з памяці.

**Не спрабуйце «змяняць пропсы». **Калі вы хочаце адрэагаваць на ўведзеныя карыстальнікам даныя (напрыклад, змену колеру), вам спатрэбіцца «задаць стан». Падрабязней пра гэта вы можаце даведацца на старонцы «[Стан: Памяць кампанента.](/learn/state-a-components-memory)»

<Recap>

* Каб перадаць пропсы, дадайце іх у JSX як бы вы дадалі атрыбуты ў HTML.
* Каб прачытаць пропсы, скарыстайце дэструктурызацыйны сінтаксіс: `function Avatar({ person, size })`.
* Вы можаце задаць прадвызначанае значэнне для пропса: `size = 100`, яно будзе скарыстана, калі пропс адсутнічае ці ягонае значэнне `undefined`.
* Вы можаце перадаць усе пропсы даччынаму элементу, скарыстаўшы сінтаксіс разгортвання: `<Avatar {...props} />`. Але не выкарыстоўвайце яго зашмат!
* Укладзены JSX, такі як `<Card><Avatar /></Card>`, з’явіцца ў кампаненце `Card` у якасці пропсы `children`.
* Пропсы нязменны і адлюстроўваюць толькі цяперашні стан: пры кожны рэндэры кампанент атрымлівае новую версію пропсаў.
* Вы не можаце змяняць пропсы. Калі вам патрэбная інтэрактыўнасць, вам спатрэбіцца задаць стан.

</Recap>



<Challenges>

#### Вынесіце кампанент {/*extract-a-component*/}

Дадзены кампанент `Gallery` змяшчае вельмі падобную разметку для двух профіляў. Вынесіце яе ў кампанент `Profile`, каб паменшыць колькасць кода, які паўтараецца. Вам давядзецца выбраць, якія пропсы вам спатрэбіцца перадаваць.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

export default function Gallery() {
  return (
    <div>
      <h1>Выбітныя навукоўцы</h1>
      <section className="profile">
        <h2>Марыя Складоўская-Кюры</h2>
        <img
          className="avatar"
          src={getImageUrl('szV5sdG')}
          alt="Марыя Складоўская-Кюры"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Сфера дзейнасці: </b> 
            фізіка і хімія
          </li>
          <li>
            <b>Нагароды: 4 </b> 
            (Нобелеўская прэмія па фізіцы, Нобелеўская прэмія па хіміі, медаль Дэві, медаль Матэуччы)
          </li>
          <li>
            <b>Адкрыццё: </b>
            Палоній (хімічны элемент)
          </li>
        </ul>
      </section>
      <section className="profile">
        <h2>Кацуко Сарухасі</h2>
        <img
          className="avatar"
          src={getImageUrl('YfeOqp2')}
          alt="Кацуко Сарухасі"
          width={70}
          height={70}
        />
        <ul>
          <li>
            <b>Сфера дзейнасці: </b>
            геахімія
          </li>
          <li>
            <b>Нагароды: 2 </b> 
            (прыз Міякэ па геахіміі, прыз Танака)
          </li>
          <li>
            <b>Адкрыццё: </b>
            метад вымярэння вуглякіслага газу ў марской вадзе
          </li>
        </ul>
      </section>
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://i.imgur.com/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

<Hint>

Пачніце з таго, каб вынесці разметку для аднаго з навукоўцаў. Потым знайдзіце часткі, якія не супадаюць з другім прыкладам, і зрабіце іх пераменнымі з дапамогай пропсаў.

</Hint>

<Solution>

У дадзеным прыкладзе кампанент `Profile` прымае шэраг пропсаў: `imageId` (радок), `name` (радок), `profession` (радок), `awards` (масіў радкоў), `discovery` (радок) і `imageSize` (нумар).

Заўважце, што пропс `imageSize` мае прадвызначанае значэнне, менавіта таму мы не перадаём яго з бацькоўскага кампанента.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({
  imageId,
  name,
  profession,
  awards,
  discovery,
  imageSize = 70
}) {
  return (
    <section className="profile">
      <h2>{name}</h2>
      <img
        className="avatar"
        src={getImageUrl(imageId)}
        alt={name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li><b>Сфера дзейнасці:</b> {profession}</li>
        <li>
          <b>Узнагароды: {awards.length} </b>
          ({awards.join(', ')})
        </li>
        <li>
          <b>Адкрыццё: </b>
          {discovery}
        </li>
      </ul>
    </section>
  );
}

export default function Gallery() {
  return (
    <div>
      <h1>Выбітныя навукоўцы</h1>
      <Profile
        imageId="szV5sdG"
        name="Марыя Складоўская-Кюры"
        profession="фізіка і хімія"
        discovery="Палоній (элемент)"
        awards={[
          'Нобелеўская прэмія па фізіцы', 
          'Нобелеўская прэмія па хіміі', 
          'медаль Дэві',
          'медаль Матэуччы'
        ]}
      />
      <Profile
        imageId='YfeOqp2'
        name='Кацуко Сарухасі'
        profession='геахімія'
        discovery="метад вымярэння вуглякіслага газу ў марской вадзе"
        awards={[
          'прыз Міякэ па геахіміі',
          'прыз Танака'
        ]}
      />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(imageId, size = 's') {
  return (
    'https://i.imgur.com/' +
    imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

Заўважце, што няма патрэбы ў тым, каб вылучаць `awardCount` у асобны пропс, бо `awards` — масіў. Можна выкарыстоўваць `awards.length` для колькасці ўзнагарод. Памятайце, што ў пропсы могуць быць перададзеныя любыя значэнні, у тым ліку і масівы!

Іншым рашэннем, больш падобным на папярэднія прыклады, будзе згрупаваць усе даныя пра асобу ў аб’ект і перадаць яго ў якасці аднаго пропса:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Profile({ person, imageSize = 70 }) {
  const imageSrc = getImageUrl(person)

  return (
    <section className="profile">
      <h2>{person.name}</h2>
      <img
        className="avatar"
        src={imageSrc}
        alt={person.name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li>
          <b>Profession:</b> {person.profession}
        </li>
        <li>
          <b>Awards: {person.awards.length} </b>
          ({person.awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {person.discovery}
        </li>
      </ul>
    </section>
  )
}

export default function Gallery() {
  return (
    <div>
      <h1>Notable Scientists</h1>
      <Profile person={{
        imageId: 'szV5sdG',
        name: 'Марыя Складоўская-Кюры',
        profession: 'фізіка і хімія',
        discovery: 'Палоній (элемент)',
        awards: [
          'Нобелеўская прэмія па фізіцы', 
          'Нобелеўская прэмія па хіміі', 
          'медаль Дэві',
          'медаль Матэуччы'
        ],
      }} />
      <Profile person={{
        imageId: 'YfeOqp2',
        name: 'Кацуко Сарухасі',
        profession: 'геахімія',
        discovery: 'метад вымярэння вуглякіслага газу ў марской вадзе',
        awards: [
          'прыз Міякэ па геахіміі',
          'прыз Танака'
        ],
      }} />
    </div>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 5px; border-radius: 50%; min-height: 70px; }
.profile {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
h1, h2 { margin: 5px; }
h1 { margin-bottom: 10px; }
ul { padding: 0px 10px 0px 20px; }
li { margin: 5px; }
```

</Sandpack>

Не гледзячы на тое, што сінтаксіс выглядае крыху інакш, бо вы апісваеце параметры JavaScript аб’екта, а не перадаяце асобныя пропсы ў якасці JSX атрыбутаў, абодва прыклады даюць патрэбны вынік, таму вы можаце выкарыстоўваць любы з гэтых падыходаў.

</Solution>

#### Рэгуляванне памеру відарыса пропсам {/*adjust-the-image-size-based-on-a-prop*/}

У дадзеным выпадку `Avatar` атрымлівае памер у лічбах праз пропс `size`, што задае шырыню і вышыню для элемента `<img>`. У дадзеным прыкладзе пропс `size` мае значэнне `40`. Але калі вы адкрыеце відарыс ў новай вокладцы, вы пабачыце, што памер самога відарыса значна большы (`160` пікселяў). Сапраўдны памер відарыса вызначаецца памерам мініяцюры, якую вы запытваеце.

Змяніце кампанент `Avatar` так, каб запытваць відарыс найбольш падыходзячага памеру, базуючыся на пропсе `size`. Канкрэтна, калі `size` меней за `90`, перадайце `'s'` («small») замест `'b'` («big») у функцыю `getImageUrl`. Пераканайцеся, што вашыя змены працуюць, паспрабаваўшы адрэндэрыць аватары з рознымі значэннямі пропса `size` і адкрыць відарыс у новай укладцы.

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person, 'b')}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <Avatar
      size={40}
      person={{ 
        name: 'Грэгорыа І. Зара', 
        imageId: '7vQD0fP'
      }}
    />
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

<Solution>

Вось адно з магчымых рашэнняў:

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{ 
          name: 'Грэгорыа І. Зара', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{ 
          name: 'Грэгорыа І. Зара', 
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Таксама вы можаце паказваць больш рэзкі відарыс, базуючыся на DPI прылады, узяўшы ва ўлік [`window.devicePixelRatio`](https://developer.mozilla.org/en-US/docs/Web/API/Window/devicePixelRatio):

<Sandpack>

```js src/App.js
import { getImageUrl } from './utils.js';

const ratio = window.devicePixelRatio;

function Avatar({ person, size }) {
  let thumbnailSize = 's';
  if (size * ratio > 90) {
    thumbnailSize = 'b';
  }
  return (
    <img
      className="avatar"
      src={getImageUrl(person, thumbnailSize)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <>
      <Avatar
        size={40}
        person={{ 
          name: 'Грэгорыа І. Зара', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={70}
        person={{ 
          name: 'Грэгорыа І. Зара', 
          imageId: '7vQD0fP'
        }}
      />
      <Avatar
        size={120}
        person={{ 
          name: 'Грэгорыа І. Зара', 
          imageId: '7vQD0fP'
        }}
      />
    </>
  );
}
```

```js src/utils.js
export function getImageUrl(person, size) {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

```css
.avatar { margin: 20px; border-radius: 50%; }
```

</Sandpack>

Пропсы дазваляюць інкапсуляваць логіку як у прыкладзе з кампанентам `Avatar` (і змяняць пазней пры патрэбе), каб кожны, хто выкарыстоўвае кампанент `<Avatar>`, маглі гэта рабіць не задумваючыся аб тым, якім чынам відарыс будзе запытаны і ў якім памеры.

</Solution>

#### Перадача JSX праз пропс `children` {/*passing-jsx-in-a-children-prop*/}

Вынесіце кампанент `Card` з разметкі ніжэй і скарыстайце пропс `children`, каб перадаваць у яго розную JSX разметку:

<Sandpack>

```js
export default function Profile() {
  return (
    <div>
      <div className="card">
        <div className="card-content">
          <h1>Фота</h1>
          <img
            className="avatar"
            src="https://i.imgur.com/OKS67lhm.jpg"
            alt="Аклілу Лема"
            width={70}
            height={70}
          />
        </div>
      </div>
      <div className="card">
        <div className="card-content">
          <h1>Пра навукоўца</h1>
          <p>Аклілу Лема быў выбітным Эфіопскім навукоўцам, хто адкрыў натуральны спосаб лячэння шыстасамозу.</p>
        </div>
      </div>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

<Hint>

Любая JSX разметка ўнутры тэга кампанента будзе перададзеная ў якасці пропса `children` у гэты кампанент.

</Hint>

<Solution>

Вось як вы можаце выкарыстоўваць кампанент `Card` у абодвух выпадках:

<Sandpack>

```js
function Card({ children }) {
  return (
    <div className="card">
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card>
        <h1>Photo</h1>
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Аклілу Лема"
          width={100}
          height={100}
        />
      </Card>
      <Card>
        <h1>About</h1>
        <p>Аклілу Лема быў выбітным Эфіопскім навукоўцам, хто адкрыў натуральны спосаб лячэння шыстасамозу.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

Таксама вы можаце зрабіць `title` асобным пропсам, калі хочаце, каб кожны `Card` меў загаловак:

<Sandpack>

```js
function Card({ children, title }) {
  return (
    <div className="card">
      <div className="card-content">
        <h1>{title}</h1>
        {children}
      </div>
    </div>
  );
}

export default function Profile() {
  return (
    <div>
      <Card title="Photo">
        <img
          className="avatar"
          src="https://i.imgur.com/OKS67lhm.jpg"
          alt="Аклілу Лема"
          width={100}
          height={100}
        />
      </Card>
      <Card title="About">
        <p>Аклілу Лема быў выбітным Эфіопскім навукоўцам, хто адкрыў натуральны спосаб лячэння шыстасамозу.</p>
      </Card>
    </div>
  );
}
```

```css
.card {
  width: fit-content;
  margin: 20px;
  padding: 20px;
  border: 1px solid #aaa;
  border-radius: 20px;
  background: #fff;
}
.card-content {
  text-align: center;
}
.avatar {
  margin: 10px;
  border-radius: 50%;
}
h1 {
  margin: 5px;
  padding: 0;
  font-size: 24px;
}
```

</Sandpack>

</Solution>

</Challenges>

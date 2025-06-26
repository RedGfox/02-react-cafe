<!-- Файл: src/components/App/App.tsx

import { useState } from 'react';
useState — це хук з React, який дозволяє створювати локальний стан у
функціональному компоненті. React API.

import css from './App.module.css';
Імпортуєш стилі з CSS-модуля. css.app буде класом для div нижче.

Шлях вказує на файл App.module.css в цій самій папці. import type { Votes,
VoteType } from '../../types/votes';
Імпортуються типи з файлу votes.ts: Votes — це інтерфейс типу об'єкта { good:
number, neutral: number, bad: number}. VoteType — це тип 'good' | 'neutral' |
'bad'.

Ти або копіював ці типи сам, або створив у src/types/votes.ts.

import CafeInfo from '../CafeInfo/CafeInfo'; importVoteOptions from
'../VoteOptions/VoteOptions'; import VoteStats from'../VoteStats/VoteStats';
import Notification from'../Notification/Notification';

Це імпорти твоїх власних компонентів, які зберігаються у відповідних папках.

Ці назви ти придумав сам:

CafeInfo VoteOptions VoteStats Notification

export default function App() { Ти створюєш головний компонент App. І він
експортується за замовчуванням (default), щоб його можна було використовувати,
наприклад, у main.tsx.

const [votes, setVotes] = useState<Votes> ({ good:0, neutral: 0, bad: 0, }); Це
стан, у якому зберігається кількість голосів:

votes — поточні значення. setVotes — функція, щоб оновити стан. Ти використав
тип Votes, і початкове значення — всі нулі.

const handleVote = (type: VoteType) => { setVotes(prev => ({ ...prev, [type]:
prev[type] + 1 })); };
Це функція, яка: Приймає тип голосу: 'good', 'neutral' або 'bad'. Оновлює стан:
збільшує відповідне значення на 1. prev — це попередній стан, ...prev — копія
всього об’єкта.

const resetVotes = () => { setVotes({ good: 0, neutral: 0, bad: 0 }); }; Скидає
всі голоси до нуля.

Використовується, коли користувач натискає кнопку "Reset".

const totalVotes = votes.good + votes.neutral + votes.bad; Обчислює загальну
кількість голосів.

Локальна змінна (не стан), бо не потрібно зберігати в React — вона рахується
автоматично при кожному рендері.

const positiveRate = totalVotes ? Math.round((votes.good / totalVotes) \* 100) :
0; Рахує відсоток позитивних голосів (good):

Якщо голоси є — порахувати відсоток. Якщо голосів нема — показати 0. Math.round
округлює результат до цілого числа.

JSX — візуальна частина:

return (

<div className={css.app}> Обгортає весь інтерфейс у div, стилізований через
App.module.css.

      <CafeInfo />

Компонент, що показує заголовок і опис кафе. Він нічого не приймає — просто
статичний текст.

      <VoteOptions
        onVote={handleVote}
        onReset={resetVotes}
        canReset={totalVotes > 0}
      />

Це компонент з кнопками "Good", "Neutral", "Bad", "Reset". Ти передаєш йому:

onVote — функція, яку він викличе при натисканні голосу.

onReset — функція для скидання.

canReset — логічне значення: чи є що скидати.

      {totalVotes > 0 ? (
        <VoteStats
          votes={votes}
          totalVotes={totalVotes}
          positiveRate={positiveRate}
        />
      ) : (
        <Notification />
      )}

Тут відбувається умовний рендеринг: Якщо є голоси, рендериться VoteStats. Якщо
нема — показується Notification. Таким чином, сторінка або показує статистику,
або повідомлення "No feedback yet".

Файл: src/components/CafeInfo/CafeInfo.tsx

import css from "./CafeInfo.module.css"; Цей рядок імпортує CSS-модуль:

Файл CafeInfo.module.css лежить у тій самій папці, що й CafeInfo.tsx.

Він містить стилі, які застосовуються до HTML-елементів цього компонента.

Класами будеш користуватися через об’єкт css, наприклад: css.container.

Це стандартний спосіб застосовувати стилі в React із CSS-модулями.

export default function CafeInfo() { Оголошується функціональний компонент з
назвою CafeInfo.

Назва CafeInfo — вигадана (авторська), але має сенс:

це інформація про кав’ярню (café information).

export default означає, що цей компонент можна імпортувати в інших файлах як:

import CafeInfo from '../CafeInfo/CafeInfo';

return ( <div className={css.container}> <h1 className={css.title}>Sip Happens
Café</h1> <p className={css.description}> Please rate our service by selecting
one of the options below. </p> </div> ); Що рендериться на сторінці:

<div className={css.container}>

Обгортка для всього контенту цього блоку.

Стилізується класом container з CafeInfo.module.css.

<h1 className={css.title}>

Назва кав’ярні: "Sip Happens Café".

Вигаданий текст — ти можеш замінити на будь-який інший.

Стилізується класом title.

<p className={css.description}>

Короткий опис:

“Please rate our service by selecting one of the options below.”

Теж вигаданий текст — можна замінити.

Стилізується класом description.

Як компонент використовується У файлі App.tsx:

<CafeInfo />
Компонент вставляється всередину сторінки — і показує заголовок + опис.

Файл: src/components/Notification/Notification.tsx

import css from './Notification.module.css'; Тут імпортується CSS-модуль з файла
Notification.module.css, який лежить у тій самій папці. Усі класи в цьому
CSS-файлі підключаються як властивості об'єкта css.

Наприклад:

.message { color: gray; } У JSX:

className={css.message} Це гарантує, що клас буде унікальний (ізольований від
інших).

export default function Notification() { Оголошується React-компонент
Notification. export default означає, що його можна імпортувати в інших файлах
без фігурних дужок:

import Notification from '../Notification/Notification'; Назва Notification —
вигадана, але логічна:

Вона означає "повідомлення", бо цей компонент показує повідомлення про
відсутність фідбеку.

return <p className={css.message}>No feedback yet</p>; Компонент повертає
простий HTML-елемент <p> з текстом:

"No feedback yet" — "Ще немає відгуків".

className={css.message} — це назва CSS-класу з CSS-модуля, стилізує цей абзац.

Файл: src/components/VoteOptions/VoteOptions.tsx Імпорт стилів

import css from './VoteOptions.module.css'; Це імпортується CSS-модуль, де css —
це об’єкт із назвами класів. Наприклад, якщо в CSS є:

.button { background: blue; } то у JSX ти можеш написати:

className={css.button} Імпорт типу

import type { VoteType } from '../../types/votes'; Імпортується тип VoteType. Це
TypeScript-перелік можливих варіантів голосування:

export type VoteType = 'good' | 'neutral' | 'bad'; Цей тип використовується для
перевірки правильного значення в onVote(type).

Інтерфейс пропсів

interface VoteOptionsProps { onVote: (type: VoteType) => void; onReset: () =>
void; canReset: boolean; } Це опис вхідних параметрів (props):

Назва Тип Що робить onVote функція викликається при кліку на одну з кнопок Good
/ Neutral / Bad onReset функція викликається при кліку на Reset canReset boolean
якщо true — показується кнопка Reset

Назви onVote, onReset, canReset — вигадані, ти міг назвати їх як завгодно,
головне — узгоджено.

Компонент

export default function VoteOptions({ onVote, onReset, canReset, }:
VoteOptionsProps) { Функція-компонент отримує деструктуризовані пропси з
батьківського компонента (тобто з App.tsx).

JSX-розмітка

return (

  <div className={css.container}>
Контейнер всіх кнопок — стилізований класом container.

Кнопки голосування

<button className={css.button} onClick={() => onVote('good')}> Good </button> Є
три кнопки:

onClick={() => onVote('good')} — викликає функцію onVote з аргументом 'good'

'good', 'neutral', 'bad' — це значення типу VoteType

onVote передається з App.tsx, де воно оновлює стан голосів.

Умовний рендеринг кнопки Reset

{canReset && ( <button className={`${css.button} ${css.reset}`}
onClick={onReset}> Reset </button> )} Кнопка Reset з'явиться лише якщо canReset
=== true.

Її класи: button (загальний стиль) + reset (додатковий стиль).

Клік по ній викликає onReset() — функція, яка скидає голоси (в App.tsx).

Звʼязок із App.tsx У файлі App.tsx компонент викликається так:

<VoteOptions
  onVote={handleVote}
  onReset={resetVotes}
  canReset={totalVotes > 0} /> Пропс у VoteOptions Звідки береться в App.tsx
onVote handleVote — додає голос onReset resetVotes — обнуляє голоси canReset
totalVotes > 0 — булеве значення

Файл: src/components/VoteStats/VoteStats.tsx Імпорт CSS-модуля

import css from './VoteStats.module.css'; Цей рядок імпортує стилі з CSS-файлу.
Наприклад, якщо в VoteStats.module.css є клас:

.stat { font-weight: bold; } то className={css.stat} буде застосовувати цей
стиль.

Назва css — довільна, але загальноприйнята.

Імпорт типу Votes

import type { Votes } from '../../types/votes'; Імпортується тип об’єкта, який
має вигляд:

export interface Votes { good: number; neutral: number; bad: number; } Цей тип
описує, скільки голосів за кожну категорію.

Інтерфейс пропсів

interface VoteStatsProper { votes: Votes; totalVotes: number; positiveRate:
number; } Це опис вхідних параметрів (props) для компонента VoteStats.

Назва Тип Що означає votes Votes об’єкт з голосами good, neutral, bad totalVotes
number загальна кількість усіх голосів positiveRate number відсоток позитивних
голосів (good / total)

Назва VoteStatsProper трохи дивна, краще: VoteStatsProps — це загальноприйнята
назва.

Компонент VoteStats

export default function VoteStats({ votes, totalVotes, positiveRate, }:
VoteStatsProper) { Компонент приймає три пропси:

votes — об’єкт { good, neutral, bad }

totalVotes — число

positiveRate — число (у відсотках)

Ці значення передаються з компонента App.tsx, де вони обчислюються з useState.

Розмітка JSX

<div className={css.container}>
css.container — зовнішній контейнер для всієї статистики.

Далі — кожен рядок виводить окрему частину статистики:

<p className={css.stat}>
  Good: <strong>{votes.good}</strong>
</p>
Виводить кількість голосів за Good.

votes.good — значення з об’єкта votes, який передано пропсом.

Інші аналогічно:

Що показується Звідки береться votes.good з об’єкта голосів votes votes.neutral
аналогічно votes.bad аналогічно totalVotes окрема змінна в App.tsx, передається
пропсом positiveRate обчислюється у App.tsx, передається пропсом

Як це працює разом із App.tsx У файлі App.tsx є таке:

{totalVotes > 0 ? ( <VoteStats
    votes={votes}
    totalVotes={totalVotes}
    positiveRate={positiveRate}
  /> ) : ( <Notification /> )} Пропс в VoteStats Значення з App.tsx votes зі
стану: useState({ good, ... }) totalVotes votes.good + votes.neutral + bad
positiveRate розрахунок через формулу

export type VoteType = 'good' | 'neutral' | 'bad'; Це тип-лімітований рядковий
літерал.

Що це означає? VoteType — це тип, який дозволяє використовувати лише одне з
трьох значень:

'good'

'neutral'

'bad'

Наприклад, якщо ти напишеш:

const option: VoteType = 'great'; // ❌ помилка Але так буде правильно:

const option: VoteType = 'neutral'; // ✅ 2. export interface Votes { ... } Це
інтерфейс, що описує об’єкт голосів. Він визначає структуру об’єкта, який
зберігає кількість голосів для кожної категорії.

export interface Votes { good: number; neutral: number; bad: number; } Як
виглядає об'єкт типу Votes?

const votes: Votes = { good: 3, neutral: 1, bad: 2, }; Цей об’єкт каже: було
віддано 3 "good", 1 "neutral", 2 "bad".

Де це використовується? У App.tsx для useState:

const [votes, setVotes] = useState<Votes>({ good: 0, neutral: 0, bad: 0, }); Тип
Votes вказує, як саме виглядає об’єкт у стані.

Тип VoteType використовується в:

const handleVote = (type: VoteType) => { ... } щоб контролювати, які значення
передаються — лише 'good', 'neutral' або 'bad'.

import { StrictMode } from 'react'; StrictMode — це компонент з React, який не
відображає нічого в DOM, але:

активує додаткові перевірки (в development-режимі),

допомагає виявити потенційні помилки в компонентах,

нагадує про застарілі методи.

Він потрібен тільки під час розробки, у продакшн його можна не використовувати.

import { createRoot } from 'react-dom/client'; createRoot — новий метод із React
18, який використовується замість старого ReactDOM.render.

import 'modern-normalize/modern-normalize.css'; Це CSS-ресет з бібліотеки
modern-normalize. Він:

скидає стилі браузера,

робить усі елементи відображеними однаково в різних браузерах.

Це добре, щоб дизайн виглядав стабільно в Chrome, Firefox, Edge тощо.

import App from './Components/App/App'; Імпортується головний компонент
застосунку — App. Саме він буде рендеритися першим.

createRoot(document.getElementById('root') as HTMLDivElement).render(
<StrictMode> <App /> </StrictMode> ); Тут відбувається кілька речей:

document.getElementById('root') — отримує елемент із index.html, куди
вставлятиметься React.

Обов’язково в HTML має бути:

<div id="root"></div>
.render(...) — всередину цього блоку вставляється:

<StrictMode>
  <App />
</StrictMode>
📦 Це означає: рендеримо App всередині StrictMode в root-елемент HTML-документу. -->

---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides, markdown enabled
title: 《React 思維進化》 ch2-8~2-9
info: |
  ## 《React 思維進化》 讀書會導讀：ch2-8~2-9
  - speaker：[Monica](https://github.com/linyawun)
  - 《React 思維進化》 讀書會：[Zet-React-Book](https://github.com/Tech-Book-Community/Zet-React-Book/tree/main/%E7%AC%AC%E4%BA%8C%E7%B5%84)
author: Monica
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
lineNumbers: true

drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
fonts:
  # basically the text
  sans: Robot Noto Sans
  # use with `font-serif` css class from UnoCSS
  serif: Robot Noto Serif
  # for code blocks, inline code, etc.
  mono: Fira Code
download: 'https://github.com/linyawun/React-Book-slide/raw/main/react-book-2-8-2-9.pdf'
exportFilename: 《React 思維進化》 ch2-8~2-9
---

# 《React 思維進化》 ch2-8~2-9

## 畫面更新的發動機：state & 畫面更新的流程機制：reconciliation

<div class='mt-6'>
<p>Speaker：Monica</p>
<p> 2024.04.03 @Tech-Book-Community</p>
</div>

<!-- <div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div> -->

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

<style>
  h2{
    @apply text-light-700;
  }
  .slidev-layout p{
    margin-top: 0px;
    margin-bottom: 0.5rem;
    opacity: 0.6;
  }
</style>

---

```yaml
transition: slide-left
```

# 什麼是 state?

- 前端常遇到使用者與網頁互動，進而使網頁產生變化的情境
  - 需記錄這些「可更新的資料」以維持應用運作，在資料更新時連動更新畫面，此類資料稱為應用程式的「state(狀態資料)」
- 單向資料流：原始資料更新時，畫面才會更新，原始資料是畫面結果的起點
  - React 的 state 機制扮演「可更新的原始資料（也常稱作狀態）」角色，作為單向資料流起
    <img src="/image/one-way data flow(state).png" class="h-15 mt-4" />

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<!--
Here is another comment.
-->

---

```yaml
transition: slide-up
level: 2
```

# state 與 component

- React 採一律重繪策略，但只重繪**跟被更新資料有關的畫面區塊**

<br>

#### 怎麼知道哪些畫面跟被更新的資料有關?

> 以 component 作為 state 運作的載體及一律重繪的界線

- 運作的載體
  - state 需依附於 component 才能記憶、維持狀態資料，生命週期隨 component 存亡
  - 可將 state 視為「component 內的資料記憶體」
- 一律重繪的界線
  - state 更新並啟動重繪時，只重繪該 component（包含其子孫 component）以內的畫面區塊

---

```yaml
transition: fade-out
```

# useState 初探

- `useState` hook 可定義、更新狀態資料，並觸發 component re-render，進而更新瀏覽器畫面
  - 只能在 component function 內呼叫
  - 可想成是一種「在 component 內註冊並存取狀態資料」的工具

<div  class='note-block'>
💡 Hooks：React 提供的 API，只能在 function component 內的頂層作用域才能呼叫的特殊函式，可將 React 核心特性或功能注入到 component 中
</div>

---

```yaml
transition: fade
```

# useState 使用方式

```js
import { useState } from 'react';
export default function App(props) {
  const [state, setState] = useState(initialState);
  //...
}
```

- 參數：state 初始值，可以是任意型別的值
- 回傳值：一個陣列，陣列包含兩個項目
  - 第一個項目：「該次 render 的當前 state 值」
  - 第二個項目：「用來更新 state 值的 `setState` 方法」，是一個 JavaScript 函式
    - 呼叫 `setState` 時傳入新 state 值作為參數，以取代舊 state 值，並觸發 component re-render
- 開發慣例
  - 以陣列解構取得 state 值和 `setState` 方法
  - 根據商業邏輯自訂變數名稱，如：`const [count, setCount] = useState(0);`

---

```yaml
transition: fade-out
```

# useState 應用範例

```jsx {all}{maxHeight:'400px'}
import { useState } from 'react';
export default function Counter() {
  //呼叫 useState 定義一個 state，來記憶計數器的值，且初始值為0
  const [count, setCount] = useState(0);

  const handleDecrementButtonClick = () => {
    //以參數指定新的 state 值為目前 count-1
    setCount(count - 1);
  };

  const handleIncrementButtonClick = () => {
    //以參數指定新的 state 值為目前 count+1
    setCount(count + 1);
  };

  return (
    <div>
      <button onClick={handleDecrementButtonClick}>-</button>
      {/* count 是 useState 取出的 state 值，count 一開始會是我們給的初始值 0 */}
      <span>{count}</span>
      <button onClick={handleIncrementButtonClick}>+</button>
    </div>
  );
}
```

---

```yaml
transition: fade
```

# onClick 事件綁定

- 在 React，可透過 React element 來間接管理和綁定事件到實際 DOM element 上

<br>

#### 如何綁定事件?

- 在對應實際 DOM element 類型的 React element 上新增 `onClick` prop，並傳遞事件處理函式給該 prop

```jsx
function MyButton() {
  const handleClick = () => {
    console.log('click!');
  };
  return <button onClick={handleClick}>click me</button>;
}
```

---

```yaml
transition: slide-up
level: 2
```

# 補充：只有對應實際 DOM element 的 React element 才會內建事件綁定的 prop

<br class='hidden'>
React element 可分為 3 種類型：

- 可對應實際 DOM element，如：`<h1>`、`<p>`、`<button>`
  - 只有此類才會內建事件綁定的 `prop`
  - 例如： ` <button onClick={handleClick}>`，React 會自動綁定事件到該 button，類似幫你做 `button.addEventListener("click", handleClick);`
- 對應自定義的 component function，如：`<MyComponent >`
  - 傳遞事件綁定的 props 沒有任何效果，只是剛好自定義 prop 也叫 `onClick`
- Fragment 類型，也就是 `<>` 或 `<Fragment>`
  - 傳遞 `key` 以外的其他 props 沒有意義

---

```yaml
transition: fade
```

# setState 方法

- 點擊 + 按鈕，呼叫 `setState` 觸發狀態資料更新，並連動畫面更新
- `setState` 使用方式
  - 參數：要更新的新值，可以是任何型別的值
    - 如果傳入函式，此函式會被視為 updater function，updater function 會拿到一個 pending state 作為參數，並回傳要更新的新值（[官網範例](https://react.dev/reference/react/useState#updating-state-based-on-the-previous-state)）
  - 回傳值：無
- `setState` 觸發 component 的 re-render 時，會重新執行 component function，產生新版本的 React element
  - 再次執行 `useState`，得到的回傳值 state 就是新 state 值（上次 `setState` 傳入的新值）

<div  class='note-block'>
💡 呼叫 <code>setState</code> 後，React 不會立即觸發 re-render，而是等正在執行的事件內所有程式結束後，才開始執行 re-render，因此會聽到「<code>setState</code> 是非同步的」這種說法
</div>

<!--
呼叫 <code>setState</code> 後，React 不會立即觸發 re-render，而是等正在執行的事件內所有程式結束後，才開始執行 re-render，因此會聽到「<code>setState</code> 是非同步的」這種說法。
因為 React 採用 batching 的機制來將更新排入佇列，之後才安排一次執行所有更新。
-->

---

```yaml
transition: fade-out
```

# 使用 setState 方法

  <img src="/image/setState-first-render.png" class="h-105" />

---

```yaml
transition: fade
```

# 使用 setState 方法

  <img src="/image/setState-re-render.png" class="h-105" />

---

```yaml
transition: slide-up
```

# state 的補充觀念

### Hooks 的限制

- 只能在 component function 內被呼叫，hooks 需依賴 component 才能運作
- 只能在 component function 的頂層作用域被呼叫，不能在條件式、迴圈或 callback 函式中呼叫

```jsx {all}{maxHeight:'200px'}
function MyComponent() {
  //✅ 合法的 hooks 呼叫: 在 component function 的頂層作用域呼叫
  useState();

  if(...){
    //⛔️ 非法的 hooks 呼叫，沒有在 component function 的頂層作用域呼叫
    useState();
  }
  for(...){
    //⛔️ 非法的 hooks 呼叫，沒有在 component function 的頂層作用域呼叫
    useState();
  }
}
useState(); // ⛔️ 非法的 hooks 呼叫，沒有在 component function 內呼叫
```

<br>

#### 為何 hooks 有這些限制?

- 為了確保 hooks 機制正確運作，沒遵守可能導致資料丟失問題

---

```yaml
transition: fade
```

# state 的補充觀念

### 為什麼 `useState` 的回傳值是一個陣列

- `useState` 回傳值：`[該次 render 的當前狀態值, 更新狀態值的 setState 方法]`
  - 回傳值是陣列有助於：呼叫 `useState` 後，更方便的將回傳值[解構賦值](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)給自定義變數
- 如果回傳的是其他資料型別?
  - `useState` 會回傳兩個值，需要用陣列或物件這種集合的方式來回傳
  - 如果回傳的是物件?
    - 每次都要解構回傳值、並為物件屬性賦予別名
    - 語法上不簡潔

<div class='ml-10'>

```js
//⛔️ 假想回傳物件的情況，非真實 useState 用法
const { state, setState } = useState();

//物件解構時為屬性賦予別名
const { state: count, setState: setCount } = useState(0);
const { state: isOpen, setState: setIsOpen } = useState(false);
```

</div>

---

```yaml
transition: fade-out
```

# state 的補充觀念

### `setState` 方法是更新 state 值並觸發 re-render 的唯一合法手段

- 如何更新 state 資料的值?
  - ✅ **`setState` 是唯一的更新方式**
  - state 是單向資料流起點，資料變更後，才會驅動 React re-render，再更新對應的畫面區塊
- 如果不透過 `setState` 方法，自己修改 state 值會怎樣?
  - ⛔️ React 不知道你修改 state 的值，因此不會觸發 React 的 re-render，也無法讓對應的畫面更新
    - 導致資料與畫面不同步，單向資料流可靠性被破壞
  - [書中範例：直接修改 state 資料無法觸發 re-render](https://codesandbox.io/p/sandbox/qr-code-2-8-3-zhi-jie-xiu-gai-state-zi-liao-wu-fa-chu-fa-re-render-yu-hua-mian-geng-xin-nhdg2g?file=%2Fsrc%2FApp.jsx)

---

```yaml
transition: slide-up
```

# state 的補充觀念

### React 如何辨認同一個 component 中的多個 state

- 可在 component function 內多次呼叫 `useState` 來定義不同的 state，且每個 state 值互不影響：
  ```js
  const [count, setCount] = useState(0);
  const [name, setName] = useState('Chair');
  ```
- 沒有給 id 或 key 這類的值，React 怎麼知道哪次的 `useState` 要回傳哪個 state 的資料?
  - component 的**所有 hooks 在每次 render 都會依賴固定的呼叫順序**，以區別彼此
    - 對應 hooks 的限制：hooks 只能在 component function 頂層作用域被呼叫
    - 在頂層作用域呼叫 hooks，才能保證每次 render 時，hooks 都會被呼叫、且執行順序固定不變
- 多次呼叫 hooks 時，React 記的是「第一個呼叫的 hook」、「第二個呼叫的 hook」這種順序，以順序區別

---

```yaml
transition: fade
```

# state 的補充觀念

### 同一個 component 的同一個 state，在該 component 的不同實例間的狀態資料獨立

- component 是一種藍圖，可透過藍圖產出實例，產出的實例互不影響
  - state 是依附在 component 上的資料，透過 component 藍圖產出的實例所擁有的 state 也互不影響
  - [React component state demo](https://codesandbox.io/p/sandbox/react-state-demo-ymg38c?file=%2Fsrc%2FApp.js%3A11%2C11)

<div class='ml-10'>

```jsx
import Counter from './Counter';
export default function App() {
  //這三個 Counter component 實例的 counter state 資料是獨立、互不影響的
  //第一個 Counter component 的 count 的值變動，不會影響另一個 Counter component 的 count 值
  return (
    <div className='App'>
      <Counter />
      <Counter />
      <Counter />
    </div>
  );
}
```

</div>

---

```yaml
transition: slide-left
```

# 補充：props 與 state 的差異

<img src="/image/props-and-state.jpg"  />

---

```yaml
transition: slide-up
```

# Render phase 與 Commit phase

<br class='hidden'>
React 的畫面處理機制可分為兩階段：

- <mdi-numeric-1-circle class='text-cyan-600'/> 產生一份描述最新畫面結構的 React element
  - 對應 component 的處理機制，稱為「render phase」
  - 在 render phase，component 會渲染並產生 React element
- <mdi-numeric-2-circle class='text-cyan-600'/> 將 React element 轉換為畫面上實際的 DOM element
  - 對應 component 的處理機制，稱為「commit phase」
  - 在 commit phase，component 會將 React element 提交並處理到瀏覽器的實際 DOM element

<style>
  ul li {
    list-style-type: none;
    margin-left: 0px;
    ul li{
      list-style-type: circle;
      margin-left: 40px;
    }
  }
</style>

---

```yaml
transition: fade
```

# Render phase 與 Commit phase

### 產生初始畫面時：首次 render component

<br>

#### Render phase

- 執行 component function，以 props 與 state 資料產生初始畫面的 React element
- 將產出的 React element 交給 commit phase 處理

<br>

#### Commit phase

- 將 component 在 render phase 回傳的 React element 全部轉換、建立成實際 DOM element
  - （因為第一次 render 時，瀏覽器畫面還沒有此 component 對應的實際 DOM element）
- 透過瀏覽器 API `appendChild()` 放到實際畫面上

> - 「component 首次 render 並 commit 到實際 DOM」的過程也稱為「mount」
> - mount 完成的狀態稱為「mounted」，代表 component render 已完成，且已「掛載」到瀏覽器畫面
>   - mounted 後，才能在瀏覽器結構中找到 component 對應的那些 DOM element

---

```yaml
transition: fade-out
```

# Render phase 與 Commit phase

### 更新畫面時：re-render component

<br>

#### Render phase

- 再次執行 component function，以新 props 與 state 產生新 React element，以對應新版本畫面
- 比較新版本 React element 和上一次 render phase 產生的舊版本 React element，找出差異處
- 將差異處交給 commit phase 繼續處理

<br>

#### Commit phase

- 只操作、更新新舊 React element 的差異處對應的實際 DOM element，其餘 DOM element 不動

> 更新畫面的情況是 React 畫面管理機制的精髓，通常把 React 更新畫面的流程稱為「reconciliation」

---

```yaml
transition: slide-up
```

# Reconciliation 流程

<br class='hidden'>

🌰 以一個 Counter component 作為接下來說明的範例：

```jsx
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0); // state 值初始值為 0
  const handleDecrementButtonClick = () => {
    setCount(count - 1); //以參數指定新的 state 的值為目前 state 值 -1
  };
  const handleIncrementButtonClick = () => {
    setCount(count + 1); //以參數指定新的 state 的值為目前 state 值 +1
  };
  return (
    <div>
      <button onClick={handleDecrementButtonClick}>-</button>
      <span>{count}</span>
      <button onClick={handleIncrementButtonClick}>+</button>
    </div>
  );
}
```

---

```yaml
transition: fade
```

# Reconciliation 流程

### 首次 render

- Render phase：state 預設是 `0`，會產生如下的 React element
  ```js
  <div>
    <button onClick={handleDecrementButtonClick}>-</button>
    <span>0</span>
    <button onClick={handleIncrementButtonClick}>+</button>
  </div>
  ```
- Commit phase：將以上的 React element 完整轉換為實際 DOM element，渲染到瀏覽器畫面上

<br>

畫面更新則進入 reconciliation 過程

---

```yaml
transition: slide-up
```

# Reconciliation 流程

### 畫面更新：reconciliation

<br>

#### 步驟一：呼叫 `setState` 方法更新 state 資料，並發起 re-render

- 呼叫 `setState` 要更新資料時，React 會先以 [`Object.is()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/is) 比較要更新的 state 值和舊的值是否相同：
  - 如果相同，判定資料沒有更新，也不需驅動畫面更新，直接中斷後續，不會觸發 re-render
  - 如果不同，判定資料需驅動畫面更新，執行 component function 的 re-render
- 🌰 以上述 Counter component 為例
  - 點擊 increment button 後，會執行 `setCount(0+1)` （因為當前 state 值為 `0`）
  - 舊 state 值是 `0`，新 state 值是 `1`，React 會以 `Object.is(0,1)` 比較新舊值是否相同
    - → 比較結果為 false，觸發 component function re-render

---

```yaml
transition: slide-up
```

# Reconciliation 流程

### 畫面更新：reconciliation

<br>

#### 步驟二：更新 state 資料並 re-render component function

- 根據傳給 `setState` 的新 state 值來更新資料，以新 props 與 state 再次執行 component function
- 🌰 以上述 Counter component 為例
  - 以 `setCount(1)` 傳入的新值 `1` 來更新 state 的值，再次執行 Counter 的 component function
  - re-render 呼叫 `useState` 回傳的 count 就是新值 `1`
  - 得到新版的 React element
    ```js
    <div>
      <button onClick={handleDecrementButtonClick}>-</button>
      <span>1</span>
      <button onClick={handleIncrementButtonClick}>+</button>
    </div>
    ```

---

```yaml
transition: slide-up
```

# Reconciliation 流程

### 畫面更新：reconciliation

<br>

#### 步驟三：比較新舊版本的 React element ，並更新差異處對應的實際 DOM element

- 以 diffing 演算法比較 re-render 的新版 React element 和上一次 render 的舊版 React element，找出兩者差異處
  - 差異處對應的實際 DOM element 是真正要操作的畫面區塊
- 找出差異後，在 commit phase，React 會操作需要被更新的 DOM element，完成畫面更新
  - 其他 DOM element 不動，降低操作 DOM 產生的效能消耗
- 🌰 以上述 Counter component 為例
  - 比較 Counter component 兩次 render 的新舊 React element，找出差異處是 `<span>` 內的文字
  - 在 commit phase，只操作 `<span>` 內的文字

---

```yaml
transition: slide-up
```

# Reconciliation 流程

### 整體流程示意圖

<br>

<img src='/image/reconciliation-flow.png' />

---

```yaml
transition: fade
```

# setState 觸發的 re-render 會觸發子 component 的 re-render

- 當 `setState` 觸發 re-render，重新執行 component function 時，如果該 component 內有子 component，也會觸發子 component 的 re-render
  - [React child component re-render demo](https://codesandbox.io/p/sandbox/child-component-re-render-demo-xsdn8g?file=%2Fsrc%2FApp.jsx%3A8%2C35)

<br>

#### component 在兩種情況下會被觸發 re-render

- <mdi-numeric-1-circle class='text-cyan-600'/> 作為有 state 且呼叫 `setState` 的 component
  - component 本身有定義 state，該 state 對應的 `setState` 方法被呼叫時（且要更新的 state 值和既有的值不同）
- <mdi-numeric-2-circle class='text-cyan-600'/> 作為子 component，被父代以上的 component re-render 影響
  - component 沒有因為自己的 `setState` 方法被呼叫而 re-render，而是 component 父代以上的 component 發生 re-render，因而觸發子 component 的 re-render

<style>
  ul li {
    list-style-type: none;
    margin-left: 0px;
    ul li{
      list-style-type: circle;
      margin-left: 40px;
    }
  }
</style>

---

```yaml
layout: center
class: text-center
transition: fade
```

# Thanks for Listening!

Medium：[ \[React\] 認識狀態管理機制 state 與畫面更新機制 reconciliation ](https://medium.com/@linyawun031/react-%E8%AA%8D%E8%AD%98%E7%8B%80%E6%85%8B%E7%AE%A1%E7%90%86%E6%A9%9F%E5%88%B6-state-%E8%88%87%E7%95%AB%E9%9D%A2%E6%9B%B4%E6%96%B0%E6%A9%9F%E5%88%B6-reconciliation-5233ff86bdcc)

---

```yaml
transition: slide-left
```

# 觀念自我檢測

- React state 的本質是什麼? state 在 React 的畫面管理機制扮演什麼角色?
- State 與 component 的關係是什麼?
- 為什麼 `useState` 的回傳值是一個陣列?
- React 是如何辨認並區分同一個 component 中的多個 state 的?
- 同一個 component 在多個地方被呼叫，它們之間的 state 資料會互通嗎?為什麼?
- 什麼是 render phase 以及 commit phase?
- 解釋 React 更新畫面的 reconciliation 流程
- 一個 component 有哪些可能會被觸發 re-render 的情形?

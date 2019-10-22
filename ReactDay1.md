# 1. Hello World

React 最簡單的範例看起來像是：

```react
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

在頁面上顯示「Hello, world!」。

> Codepen Example：https://zh-hant.reactjs.org/redirect-to-codepen/hello-world

>  re-introduction to JavaScript (JS tutorial)：https://tinyurl.com/npqrde8

> 本篇指南偶爾會使用較新的 JavaScript 語法，可以參考：https://tinyurl.com/y27naaop



# 2. 介紹 JSX

考慮下面這個變數宣告：

```react
const element = <h1>你好，世界！</h1>;	
```

這個有趣的標籤語法不是一個字串也不是 HTML。

這個語法叫做 JSX，是一個 JavaScript 的語法擴充，可能為讓你想到一些樣板語言，但不一樣的地方是 JSX 允許你使用 JavaScript 所有的功能。



### 為什麼要用 JSX?

React 擁抱了 render 邏輯從根本上就得跟其他 UI 邏輯綁在一起的事實：事件要怎麼處理？隨著時間經過 state 會如何變化？以及要怎麼將資料準備好用於顯示？與其刻意的將*技術*拆開，把標籤語法跟邏輯拆放於不同檔案之中，React 的方法是將其拆分為很多同時包含 UI 與邏輯的 component，而彼此之間很少互相依賴。

> 沒被說服接受將標籤語法寫在 JS 裡頭，可以參考：https://tinyurl.com/le224w9



### 在 JSX 中嵌入 Expression

在下面這個範例中，我們宣告一個名為 `name` 的變數，並在 JSX 中透過將其名稱包在大括號中使用：

```react
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);	
```

你可以在 JSX 的大括號中寫入任何合法的 JavaScript expression。舉例來說，`2 + 2`、`user.firstName` 以及 `formatName(user)` 都是合法的 JavaScript expression。
> JavaScript expression：https://tinyurl.com/y6chkvmp



接下來的範例中，我們將嵌入呼叫 JavaScript function (`formatName(user)`) 的回傳值到一個 `<h1>` element 中。

```react
function formatName(user) {
  return user.firstName+ ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);	
```

> Codepen Example：https://zh-hant.reactjs.org/redirect-to-codepen/introducing-jsx



### JSX 本身也是 Expression

在編譯之後，JSX expressions 就變成了一般的 JavaScript function 呼叫並回傳 JavaScript 物件。這表示你也可以在 `if` 跟 `for` 迴圈中使用 JSX，將其指定到一個變數，使用 JSX 作為參數並由 function 中回傳。

```react
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```



### 在 JSX 中指定屬性

你可以使用引號將字串設定為屬性：

```react
const element = <div tabIndex="0"></div>;
```

你也可以在屬性中使用大括號來嵌入一個 JavaScript expression：

```react
const element = <img src={user.avatarUrl}></img>;
```

不要在嵌入 JavaScript expression 作為屬性的時候同時使用引號或是大括號。你應該要在使用字串屬性的時候使用引號，使用 expressions 的時候使用大括號，但不要同時使用。

>**注意：**

> > 由於 JSX 比較接近 JavaScript 而不是 HTML，React DOM 使用 `camelCase` 來命名屬性而不是使用慣有的 HTML 屬性名稱。
> >
> > 舉例來說：在 JSX 之中，`class` 變成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) 而 `tabindex` 變成了 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。



### 在 JSX 中指定 Children

就像是在 XML 之中，如果一個標籤是空白的，你可以用 `/>` 立刻關閉這個標籤：

```react
const element = <img src={user.avatarUrl} />;	
```

JSX 標籤也可以包含 children：

```react
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```



### JSX 防範注入攻擊

在 JSX 之中可以安全的直接嵌入使用者輸入：

```react
const title = response.potentiallyMaliciousInput;
// 這是安全的：
const element = <h1>{title}</h1>;
```

React DOM 預設會在 render 之前 [escape](http://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) 所有嵌入在 JSX 中的變數。這保證你永遠不會不小心注入任何不是直接寫在你的應用程式中的東西。所有變數都會在 render 之前轉為字串，這可以避免 [XSS（跨網站指令碼）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻擊。



### JSX 表示物件

Babel 將 JSX 編譯為呼叫 `React.createElement()` 的程式。

下面這兩個例子完全相同：

```react
const element = (
  <h1 className="greeting">
    Hello, World!
  </h1>
);
```

```react
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, World!'
);
```

`React.createElement()` 會進行一些檢查以幫助你寫出沒有 bug 的程式，但基本上它會產生類似下面的物件：

```react
// 注意：這是簡化過的結構
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

這種物件被稱呼為「React element」。你可以想像他們描述的是你想要在螢幕上看到的東西，React 會讀取這些物件並用這些描述來產生 DOM 並保持他們在最新狀態。

我們會在下一個章節探討如何把 React element render 到 DOM 之中。

>**提示：**

> > 我們推薦你在編輯器中使用 [「Babel」語法](http://babeljs.io/docs/editors)，這樣可以確保 ES6 跟 JSX 都能夠正確的被語法突顯。



# 3.Render Element

建立 React 應用程式最小的單位是 element。一個 element 描述你想要在螢幕上所看到的：

```react
const element = <h1>Hello, world</h1>;
```

與瀏覽器的 DOM element 不同，React element 是單純的 object，而且很容易被建立。React DOM 負責更新 DOM 來符合 React element。



## Render Element 到 DOM 內

假設你的 HTML 檔案內有一個 `<div>`：

```react
<div id="root"></div>
```

我們稱為這是一個「root」DOM node，因為所有在內的 element 都會透過 React DOM 做管理。

使用 React 建立應用程式時，通常會有一個單一的 root DOM node。如果你想要整合 React 到現有的應用程式時，你可以根據你的需求獨立出多個 root DOM node。

如果要 render 一個 React element 到 root DOM node，傳入兩者到 `ReactDOM.render()`：

```react
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

> Codepen Example：https://tinyurl.com/y5gauzwm



## 更新被 Render 的 Element

React element 是 [immutable](https://en.wikipedia.org/wiki/Immutable_object) 的。一旦你建立一個 element，你不能改變它的 children 或是 attribute。Element 就像是電影中的一個幀：它代表特定時間點的 UI。

憑藉我們迄今為止對 React 的認識，更新 UI 唯一的方式是建立一個新的 element，並且將它傳入到 `ReactDOM.render()`。

思考以下這個 ticking clock 的範例：

```react
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

它從 [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback 每秒呼叫 `ReactDOM.render()`。

> Codepen Example：https://tinyurl.com/y63r77jw

> **注意：**
>
> > 在實踐中，大部分 React 應用程式只呼叫 `ReactDOM.render()` 一次。在下一個章節中，我們將會學習如何將這些程式碼封裝到 [stateful component](https://zh-hant.reactjs.org/docs/state-and-lifecycle.html)。



## React 只更新必要的 Element

React DOM 會將 element 和它的 children 與先前的狀態做比較，並且只更新必要的 DOM 達到理想的狀態。

你可以透過瀏覽器工具來檢測做驗證：

> Codepen Example：https://tinyurl.com/y63r77jw

即使我們在每秒建立一個 element 描述整個 UI tree，只有內容更改的 text node 才會被 React DOM 更新。

根據我們的經驗，應該思考 UI 在任何時候應該如何呈現，而不是隨著時間的推移去消除錯誤。



# 4.Components 與 Props

Component 使你可以將 UI 拆分成獨立且可複用的程式碼，並且專注於各別程式碼的思考。本章節旨在介紹 component 的相關概念，你也可以在此參閱[詳細的 API 文件](https://zh-hant.reactjs.org/docs/react-component.html)。

概念上來說，component 就像是 JavaScript 的 function，它接收任意的參數（稱之為「props」）並且回傳描述畫面的 React element。



## Function Component 與 Class Component

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

此 function 是一個符合規範的 React component，因為它接受一個「props」（指屬性 properties）物件並回傳一個 React element。我們稱之為 function component，因為它本身就是一個 JavaScript function。

同樣的，你也可以使用 [ES6 Class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 來定義 component：

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上述兩種 component 在 React 中是同等的，我們將會在 [下一個章節](https://zh-hant.reactjs.org/docs/(/docs/state-and-lifecycle.html)) 探討 class 所擁有的額外特性



## Render 一個 Component

在此之前，我們只見過這種相當於 DOM 標籤的 React element：

```react
const element = <div />;
```

不過，React element 也可以是使用者自定義的 component：

```react
const element = <Welcome name="Sara" />;
```

當 React element 為使用者定義的 component 時，它會將 JSX 所接收的屬性作為一個物件傳遞給 component，這一個物件被稱為「props」。

舉例來說，這段程式碼會在頁面上 render 出「Hello, Sara」：

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
>Codepen Example：https://tinyurl.com/y2cfglnc



讓我們來複習一下這個例子發生了什麼事：

1. 我們對 `<Welcome name="Sara" />` 這個 element 呼叫了 `ReactDOM.render()`。
2. React 以 `{name: 'Sara'}` 作為 props 傳入 `Welcome` component 並呼叫。
3. `Welcome` component 回傳了 `<h1>Hello, Sara</h1>` 這個 element 作為返回值。
4. React DOM 有效的將 DOM 更新為 `<h1>Hello, Sara</h1>`。



>**注意：** Component 的字首須為大寫字母

> > React 會將小寫字母開頭的組件視為原始 DOM 標籤，舉例來說，`<div />` 就會被視為是 HTML 的 div 標籤，但是 `<Welcome />` 則是一個 component，而且需要在作用域中使用 `Welcome`。
> >
> > 想要了解更多關於此慣例的原因，請參閱 [JSX In Depth](https://zh-hant.reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized) 章節。



## 組合 Component

Component 可以在輸出中引用其他 component。我們可以在任何層次中抽象化相同的 component，按鈕、表單、對話框、甚至是整個畫面，在 React 應用程式中都將以 component 的方式呈現。

舉例來說，我們可以建立一個 render 多次 `Welcome` 的 `App` component：

```react
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

> Codepen Example：**https://tinyurl.com/y2shtu5m**

通常來說，每個 React 應用程式都有一個最高層級的 `App` component。然而，如果你將 React 結合至現存的應用程式中，你可能需要使用像 `Button` 這樣的小型 component，並由下往上，逐步應用到畫面的最高層級。



## 抽離 Component

別害怕將 component 拆分成更小的 component。舉例來說，我們看這個 `Comment` 的 component：

```react
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> Codepen Example：https://tinyurl.com/y4lhlvzl

它接受 `author` (一個物件)、`text` (一個字串)、還有 `date` (一個日期) 作為它的 props。它的作用是在一個社交網站上 render 一則評論。

這個 component 可能因為太多的巢狀關係而難以更動，而且也難以複用獨立的部分。讓我們把一些 component 從中分離吧。

首先，我們將 `Avatar` 分離出來：

```react
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

這個 `Avatar` 並不需知道它會被 render 在 `Comment` 中。這是為什麼我們給他一個更為一般的名字：`user` 而不是 `author`。

我們建議從 component 的角度為 props 命名，而不是它的使用情境。

現在我們可以稍微簡化 `Comment`：

```react
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

接下來，我們將 `UserInfo` component 也抽離出來，它會在使用者名稱旁邊 render `Avatar` component：

```react
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

讓我們將 `Comment` 更加簡化：

```react
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> Codepen Example：**https://tinyurl.com/y6goacbm**

在一開始，將 component 抽離出來可能是一件繁重的工作，但是在較大的應用程式中，建構可複用的 component 是非常值得。以經驗來說，如果一個 UI 中有一部分會被重複使用很多次（`Button`、`Panel`、`Avatar`），或者它足夠複雜到自成一個 component（`App`、`FeedStory`、`Comment`），那它就適合被抽出作為一個可複用的 component。



## Props 是唯讀的

不管你使用 function 或是 class 來宣告 component，都絕不能修改自己的 props。

例如這個 sum function：

```react
function sum(a, b) {
  return a + b;
}
```

像這樣的 function 是 [Pure function](https://en.wikipedia.org/wiki/Pure_function) 的，因為他們並沒有改變輸入，而且相同的輸入總是回傳一樣的結果。

相反地，這個 function 並非 Pure function，因為它更改了它的參數：

```react
function withdraw(account, amount) {
  account.total -= amount;
}
```

React 是很彈性的，但有一條嚴格的規定：

**所有的 React component 都必須像 Pure function 一般保護他的 props**

當然，應用程式的 UI 是動態的，而且總是隨著時間改變。在[下個章節](https://zh-hant.reactjs.org/docs/state-and-lifecycle.html)，我們會介紹一個新的概念「state」。State 可以在不違反上述規則的前提下，讓 React component 隨使用者操作、網路回應、或是其他方式改變輸出內容。
## 拆分组件 

现在，你有一个大型的 App 组件。它在不停地扩展，最终可能会变得混乱。你可以开始将它拆分成若干个更小的组件。

让我们开始使用一个用于搜索的输入组件和一个用于展示的列表组件。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search />
        <Table />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

你可以给组件传递属性并在组件中使用它们。至于 App 组件，它需要传递由本地状态 (state) 托管的属性和它自己的类方法。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

现在你可以接着 App 组件定义这些组件。这些组件仍然是 ES6 类组件，它们会渲染和之前相同的元素。

第一个是 Search 组件。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...
}

# leanpub-start-insert
class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

第二个是 Table 组件。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

现在你有了三个 ES6 类组件。你可能已经注意到，`props` 对象可以通过这个类实例的 `this` 来访问。props 是 properties 的简写，当你在 App 组件里面使用它时，它有你传递给这些组件的所有值。这样，组件可以沿着组件树向下传递属性。

从 App 组件中提取这些组件之后，你就可以在别的地方去重用它们了。因为组件是通过 props 对象来获取它们的值，所以当你在别的地方重用它时，你可以每一次都传递不同的 props，这些组件就变得可复用了。

### Exercises: 练习：

* 从已经完成的 Search 和 Table 组件中找出可以进一步提取的组件
  * 但是不要现在就去做，否则在接下来的几个章节你会遇到冲突。  
  
## Composable Components 可组合组件

在 props 对象中还有一个小小的属性可供使用: `children` 属性。通过它你可以将元素从上层传递到你的组件中，这些元素对你的组件来说是未知的，但是却为组件相互组合提供了可能性。让我们来看一看，当你只将一个文本 (字符串) 作为子元素传递到 Search 组件中会怎样。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

现在 Search 组件可以从 props 对象中解构出 children 属性。然后它就可以指定这个 children 应该显示在哪里。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-insert
    return (
      <form>
# leanpub-start-insert
        {children} <input
# leanpub-end-insert
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

现在，你应该可以在输入框旁边看到这个 "Search" 文本了。当你在别的地方使用 Search 组件时，如果你喜欢，你可以选择一个不同的文本。总之，它不仅可以把文本作为子元素传递，还可以将一个元素或者元素树 (它还可以再次封装成组件) 作为子元素传递。children 属性让组件相互交织在一起成为可能。

### Exercises: 练习：

* 阅读更多关于 [React 组件模型](https://facebook.github.io/react/docs/composition-vs-inheritance.html) 的内容

## Reusable Components 可复用组件

可复用和可组合组件让你能够给出合理的组件分层，它们是 React 视图层的基础。前面几章提到了可重用性的术语。现在你可以复用 Search 和 Table 组件了。甚至 App 组件都是可复用的了，因为你可以在别的地方重新实例化它。

让我们再来定义一个可复用组件 Button，最终会被更频繁地复用。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

声明这样一个组件可能看起来有点多余。你将会用 `Button` 组件来替代 `button` 元素。它只省去了 `type="button"`。当你想使用 Button 组件的时候，你还得去定义除了 type 之外的所有属性。但这里你必须要考虑到长期投资。想象在你的应用中有若干个 button，但是你想改变它们的一个属性、样式或者行为。如果没有这个组件的话，你就必须重构每个 button。相反，Button 组件拥有单一可信数据源。一个 Button 组件可以立即重构所有 button。一个 Button 组件统治所有的 button。

既然你已经有了 button 元素，你可以用 Button 组件代替。它省略了 type 属性，因为 Button 组件已经指定了。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
# leanpub-start-insert
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Button 组件期望在 props 里面有一个 `className` 属性. `className` 属性是 React 基于 HTML 属性 `class` 的另一个衍生物。但是当使用 Button 组件时，我们并没有传递任何 `className` 属性，所以在 Button 组件的代码中，我们更应该明确地标明 `className` 是可选的。

因此，你可以使用默认参数，它是一个 JavaScript ES6 的特性。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
# leanpub-start-insert
      className = '',
# leanpub-end-insert
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

这样当使用 Button 组件时，若没有指定 `className` 属性，它的值就是一个空字符串，而非 `undefined`。

### Exercises: 练习：

* 阅读更多关于 [ES6 默认参数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters) 的内容
## Component Declarations 组件声明

现在你已经有四个 ES6 类组件了，但是你可以做得更好。让我来介绍一下函数式无状态组件 (functional stateless components)，作为除了 ES6 类组件的另一个选择。在重构你的组件之前，让我来介绍一下 React 不同的组件类型。

* **函数式无状态组件:** 这类组件就是函数，它们接收一个输入并返回一个输出。输入是 props，输出就是一个普通的 JSX 组件实例。到这里，它和 ES6 类组件非常的相似。然而，函数式无状态组件是函数 (函数式的)，并且它们没有本地状态 (无状态的)。你不能通过 `this.state` 或者 `this.setState()` 来访问或者更新状态，因为这里没有 `this` 对象。此外，它也没有生命周期方法。虽然你还没有学过生命周期方法，但是你已经用到了其中两个：`constructor()` and `render()`。constructor 在一个组件的生命周期中只执行一次，而 `render()` 方法会在最开始执行一次，并且每次组件更新时都会执行。当你阅读到后面关于生命周期方法的章节时，要记得函数式无状态组件是没有生命周期方法的。

* **ES6 类组件:** 在你的四个组件中，你已经使用过这类组件了。在类的定义中，它们继承自 React 组件。`extend` 会注册所有的生命周期方法，只要在 React component API 中，都可以在你的组件中使用。通过这种方式你可以使用 `render()` 类方法。此外，通过使用 `this.state` 和 `this.setState()`，你可以在 ES6 类组件中储存和操控 state。

* **React.createClass:** 这类组件声明曾经在老版本的 React 中使用，仍然存在于很多 ES5 React 应用中。但是为了支持 JavaScript ES6，[Facebook 声明它已经不推荐使用了](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html)。他们还[在 React 15.5 中加入了不推荐使用的警告](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html)。你不会在本书使用它。

因此这里基本只剩下两种组件声明了。但是什么时候更适合使用函数式无状态组件而非 ES6 类组件？一个经验法则就是当你不需要本地状态或者组件生命周期方法时，你就应该使用函数式无状态组件。最开始一般使用函数式无状态组件来实现你的组件，一旦你需要访问 state 或者生命周期方法时，你就必须要将它重构成一个 ES6 类组件。在我们的应用中，为了学习 React，我们采取了相反的方式。

让我们回到你的应用中。App 组件使用内部状态，这就是为什么它必须作为 ES6 类组件存在的原因。但是你的其他三个 ES6 类组件都是无状态的，它们不需要使用 `this.state` 或者 `this.setState()`，甚至都不需要使用生命周期函数。让我们一起把 Search 组件重构成一个函数式无状态组件。Table 和 Button 组件的重构会留做你的练习。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
# leanpub-end-insert
~~~~~~~~

基本上就是这样了。props 可以在[函数签名](https://developer.mozilla.org/zh-CN/docs/Glossary/Signature/Function)（译者注：这里应指函数入参）中访问，返回值是 JSX。你已经知道 ES6 解构了，所以在函数式无状态组件中，你可以优化之前的写法。最佳实践就是在函数签名中通过解构 props 来使用它。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search({ value, onChange, children }) {
# leanpub-end-insert
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

但是它还可以变得更好。你已经知道，ES6 箭头函数允许让你保持你的函数简洁。你可以移除函数的块声明（译者注：即花括号`{}`）。在简化的函数体中，表达式会自动作为返回值，因此你可以将 return 语句移除。因为你的函数式无状态组件是一个函数，你同样可以用这种方式来简化它。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
# leanpub-end-insert
~~~~~~~~

最后一步对于强制只用 props 作为输入和 JSX 作为输出非常有用。这之间没有任何别的东西。但是你仍然可以在 ES6 箭头函数块声明中*做一些事情*。

{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

但是你现在并不需要这样做，这也是为什么你可以让之前的版本没有块声明。当使用块声明时，人们往往容易在这个函数里面做过多的事情。通过移除块声明，你可以专注在函数的输入和输出上。

现在你已经有一个轻量的函数式无状态组件了。一旦你需要访问它的内部组件状态或者生命周期方法，你最好将它重构成一个 ES6 类组件。另外，你也已经看到，JavaScript ES6 是如何被用到 React 组件中并让它们变得更加的简洁和优雅。

### Exercises: 练习：

* 将 Table 和 Button 组件重构成函数式无状态组件
* 阅读更多关于 [ES6 类组件和函数式无状态组件](https://facebook.github.io/react/docs/components-and-props.html) 的内容

## Styling Components 给组件声明样式

让我们给你的应用和组件添加一些基本的样式。你可以复用 *src/App.css* 和 *src/index.css* 文件。因为你是用 *create-react-app* 来创建的，所以这些文件应该已经在你的项目中了。它们应该也被引入到你的 *src/App.js* 和 *src/index.js* 文件中了。我准备了一些 CSS，你可以直接复制粘贴到这些文件中，你也可以随意使用你自己的样式。

首先，给你的整个应用声明样式。

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~

其次，在 App 文件中给你的组件声明样式。

{title="src/App.css",lang="css"}
~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

现在你可以在一些组件中使用这些样式。但是别忘了使用 React 的 `className`， 而不是 HTML 的 `class` 属性。

首先，将它应用到你的 App ES6 类组件中。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
# leanpub-start-insert
      <div className="page">
        <div className="interactions">
# leanpub-end-insert
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
# leanpub-start-insert
        </div>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-start-insert
      </div>
# leanpub-end-insert
    );
  }
}
~~~~~~~~

其次，将它应用到你的 Table 函数式无状态组件中。

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
# leanpub-start-insert
  <div className="table">
# leanpub-end-insert
    {list.filter(isSearched(pattern)).map(item =>
# leanpub-start-insert
      <div key={item.objectID} className="table-row">
# leanpub-end-insert
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
# leanpub-start-insert
            className="button-inline"
# leanpub-end-insert
          >
            Dismiss
          </Button>
        </span>
# leanpub-start-insert
      </div>
# leanpub-end-insert
    )}
# leanpub-start-insert
  </div>
# leanpub-end-insert
~~~~~~~~

现在你已经给你的应用和组件添加了基本的 CSS 样式，看起来应该非常不错。如你所知，JSX 混合了 HTML 和 JavaScript。现在有人呼吁将 CSS 也加入进去，这就叫作内联样式 (inline style)。你可以定义 JavaScript 对象，并传给一个元素的 style 属性。

让我们通过使用内联样式来使 Table 的列宽自适应。

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    {list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
# leanpub-start-insert
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
# leanpub-end-insert
      </div>
    )}
  </div>
~~~~~~~~

现在样式已经内联了。你可以在你的元素之外定义一个 style 对象，这样可以让它变得更整洁。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

随后你可以将它们用于你的 columns ：`<span style={smallColumn}>`。

总而言之，关于 React 中的样式，你会找到不同的意见和解决方案。现在你已经用过纯 CSS 和 内联样式了。这足以开始。

在这里我不想下定论，但是想给你一些更多的选择。你可以自行阅读并应用它们。但是如果你刚开始使用 React，目前我会推荐你坚持纯 CSS 和内联样式。

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)
* [CSS 模块化](https://github.com/css-modules/css-modules)

{pagebreak}

你已经学习了编写一个 React 应用所需要的基础知识了！让我们来回顾一下前面几个章节:

* React
  * 使用 `this.state` 和 `setState()` 来管理你的内部组件状态
  * 将函数或者类方法传递到你的元素处理器
  * 在 React 中使用表单或者事件来添加交互 
  * 在 React 中单向数据流是一个非常重要的概念
  * 拥抱 controlled components
  * 通过 children 和可复用组件来组合组件
  * ES6 类组件和函数式无状态组件的使用方法和实现
  * 给你的组件声明样式的方法
* ES6
  * 绑定到一个类的函数叫作类方法
  * 解构对象和数组
  * 默认参数
* General
  * 高阶函数

该休息一下了，吸收这些知识然后转化成你自己的东西。你可以用你已有的代码来做个实验。另外，你可以进一步阅读[官方文档](https://facebook.github.io/react/docs/installation.html)

你可以在[官方代码仓库](https://github.com/rwieruch/hackernews-client/tree/4.2)找到源码。
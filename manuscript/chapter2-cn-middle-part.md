## 事件处理

本章节会让你对元素的事件处理有更深入的了解，在你的应用程序中，你将使用下面的按钮来从列表中忽略一项内容

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

这已经是一个复杂的例子了，因为你必须传递一个参数到类的方法，因此你需要将它封装到另一个（箭头）函数中，基本上，由于要传递给事件处理器使用，因此它必须是一个函数。下面的代码不会工作，因为类方法会在浏览器中打开程序时立即执行。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

当使用 `onClick={doSomething()}` 时，`doSomething()` 函数会在浏览器打开程序时立即执行，由于监听表达式是函数执行的返回值而不再是函数，所以点击按钮时不会有任何事发生。但当使用 `onClick={doSomething}` 时，因为 `doSomething` 是一个函数，所以它会在点击按钮时执行。同样的规则也适用于在程序中使用的 `onDismiss()` 类方法。

然而，使用 `onClick={this.onDismiss}` 并不够，因为这个类方法需要接收 item.objectID 属性来识别那个将要被忽略的项，这就是为什么它需要被封装到另一个函数中来传递这个属性。这个概念在 JavaScript 中被称为高阶函数，稍后会做简要解释。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

一个解决方案事在外部定义一个包装函数，并且只将定义的函数传递给处理程序。因为需要访问特定的列表项，所以它必须位于 map 函数块的内部。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

毕竟，传给元素事件处理器的内容必须是函数。作为一个示例，请尝试以下代码：

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
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
~~~~~~~~

它会在浏览器加载该程序时执行，但点击按钮时并不会。而下面的代码只会在点击按钮时执行。它是一个在触发事件时才会执行的函数。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

为了保持简洁，你可以把它转成一个 JavaScript ES6 的箭头函数，和我们在 `onDismiss()` 类方法时做的一样。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

经常会有 React 初学者在事件处理中使用函数遇到困难，这就是为什么我要在这里更详细的解释它。最后，你应该使用下面的代码来拥有一个可以访问 `item` 对象的 `objectID` 属性简洁的内联 JavaScript ES6 箭头函数。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

另一个经常会被提到的性能相关话题是在事件处理程序中使用箭头函数的影响。例如，`onClick` 事件处理中的 `onDismiss()` 方法被封装在另一个箭头函数中以便能传递项标识。因此每次 `render()` 执行时，事件处理程序就会实例化一个高阶箭头函数，它*可能*会对你的程序性能产生影响，但在大多数情况下你都不会注意到这个问题。假设你有一个包含1000个项目的巨大数据表，每一行或者列在事件处理程序中都有这样一个箭头函数，这个时候就需要考虑性能影响，因此你可以实现一个专用的按钮组件来在构造函数中绑定方法，但这是一个不成熟的优化。所以现在，专注到学习 React 会更有价值。

### 练习：

>尝试在按钮的 `onClick` 处理程序中使用函数的不同方法

## 和表单交互

让我们在程序中加入表单来体验 React 和表单事件的交互，我们将在程序中加入搜索功能，列表会根据输入框的内容对标题进行过滤。

第一步，你需要在 JSX 中定义一个带有输入框的表单。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

在下面的场景中，将会使用在输入框中的内容作为搜索字段来临时过滤列表。为了能根据输入框的值过滤列表，你需要将输入框的值储存在你的本地状态中，但是如何访问这个值呢？你可以使用 React 的 **合成事件** 来访问事件返回值。

让我们为输入框定义一个 `onChange` 处理程序。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

这个函数被绑定到组件上，因此再次成为一个类方法，你必定义方法并 bind 它。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

在元素中使用监听时，你可以在回调函数的签名中访问到 React 的合成事件。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

event 对象的 target 属性中带有输入框的值，因此你可以使用 `this.setState()` 来更新本地的搜索词的状态了。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

此外，你应该记住在构造函数中为 `searchTerm` 定义初始状态，输入框在开始时应该是空的，因此初始值应该是空字符串。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

现在你将会把输入框每次变化的输入值储存到组件的内部状态中。

关于一个在 React 组件中更新状态的简要说明，在使用 `this.setState()` 更新 `searchTerm` 时应该把这个列表也传递进去来保留它才是公平的，但事实并非如此，React 的 `this.setState()` 是一个浅合并，在更新一个唯一的属性时，他会保留状态对象中的其他属性，因此即使你已经在列表状态中排除了一个项，在更新 `searchTerm` 属性时也会保持不变。

让我们回到你的程序中，现在列表还没有根据储存在本地状态中的输入字段进行过滤。基本上，你已经具有了根据 `searchTerm` 临时过滤列表的所有东西。那么怎么暂时的过滤它呢？你可以在 `render()` 方法中，在 map 映射列表之前，插入一个过滤的方法。这个过滤方法将只会匹配标题属性中有 `searchTerm` 内容的列表项。你已经使用过了 JavaScript 内置的 filter 功能，让我们再用一次，你可以在 map 函数之前加入 filter 函数，因为 filter 函数返回一个新的数组，所以 map 函数可以这样方便的使用。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

让我们用一种不同的方式来处理过滤函数，我们想在 ES6 组件类之外定义一个传递给过滤函数的函数参数，在这里我们不能访问到组件内的状态，所以无法访问 `searchTerm` 属性来作为筛选条件求值，我们需要传递 `searchTerm` 到过滤函数并返回一个新函数来根据条件求值，这叫做高阶函数。

一般来说，我不会提到高阶函数，但在 React 中了解高阶函数是有意义的，因为在 React 中有一个高阶组件的概念，你将在这本书的后面了解到这个概念。但现在，让我们关注到过滤器的功能。

首先，你需要在 App 组件外定义一个高阶函数。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

该函数接受 `searchTerm` 并返回另一个函数，因为所有的 filter 函数都接受一个函数作为它的输入，返回的函数可以访问列表项目对象，因为它是传给 filter 函数的函数。此外，返回的函数将会根据函数中定义的条件对列表进行过滤。让我们来定义条件。

{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

条件是列表中项目的标题属性和输入的 `searchTerm` 参数相匹配，你可以使用 JavaScript 内置的 `includes` 功能来实现这一点。只有满足匹配时才会返回 true 并将项目保留在列表中。当不匹配时，项目会从列表中移除。但需要注意的是，你需要把输入内容和待匹配的内容都转换成小写，否则，搜索词 'redux' 和列表中标题叫 'Redux' 的项目无法匹配。由于我们使用的是一个不可变的列表，并使用 filter 函数返回一个新列表，所以本地状态中的原始列表根本就没有被修改过。

还有一点需要注意，我们使用了 Javascript 内置的 includes 功能，它已经是一个 ES6 的特性了。这在 ES5 中该如何实现呢？你将使用 `indexOf()` 函数来获取列表中项的索引，当项目在列表中时，`indexOf()` 将会返回它的索引。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

另一个优雅的重构可以用 ES6 箭头函数完成，它可以让函数更加整洁:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

人们会争论哪个函数更易读，就我个人而言，我更习惯第二个。React 的生态使用了大量的函数式编程概念。通常情况下，你会使用一个函数返回另一个函数（高阶函数）。在 JavaScript ES6 中，可以使用箭头函数更简洁的表达这些。

最后，你需要使用定义的 `isSearched()` 函数来过滤你的列表，你从本地状态中传递 `searchTerm` 属性返回一个根据条件过滤列表的输入过滤函数。之后它会映射过滤后的列表用于显示每个列表项的元素。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

搜索功能现在应该起作用了，在浏览器中自己尝试一下。

### 练习：

* 阅读更多 [React 事件](https://facebook.github.io/react/docs/handling-events.html) 相关内容
* 阅读更多 [高阶函数](https://en.wikipedia.org/wiki/Higher-order_function) 相关内容

## ES6 解构

在 JavaScript ES6 中有一种更方便的方法来访问对象和数组的属性，叫做解构。比较下面 JavaScript ES5 和 ES6 的代码片段。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

在 JavaScript ES5 中每次访问对象的属性都需要额外添加一行代码，但在 JavaScript ES6 中可以在一行中进行。可读性最好的方法是在将对象解构成多个属性时使用多行。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

对于数组一样可以使用解构，同样，多行代码会使你的代码保持可读性。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

也许你已经注意到，程序组件内的状态对象也可以使用同样的方式解构，你可以让 map 和 filter 部分的代码更简短。

{title="src/App.js",lang=javascript}
~~~~~~~~
  render() {
# leanpub-start-insert
    const { searchTerm, list } = this.state;
# leanpub-end-insert
    return (
      <div className="App">
        ...
# leanpub-start-insert
        {list.filter(isSearched(searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
~~~~~~~~

你可以使用 ES5 或者 ES6 的方式来做：

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

由于这本书大部分时候都使用了 JavaScript ES6，所以你应该坚持使用它。

### 练习：

* 阅读更多[ES6 解构](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)的相关内容

## 受控组件

你已经了解了 React 中的单向数据流，同样的规则适用于更新本地状态 `searchTerm` 来过滤列表的输入框。当状态变化时，`render()` 方法将再次运行，并使用最新状态中的`searchTerm` 值来作为过滤条件。

但是我们是否忘记了输入元素的一些东西？一个 HTML 输入标签带有一个 `value` 属性，这个属性通常有一个值作为输入框的显示，在本例中，它是 `searchTerm` 属性。然而，看起来我们在 React 好像并不需要它。

这是错误的，表单元素比如 `<input>`, `<textarea>` 和 `<select>` 会以原生 HTML 的形式保存他们自己的状态。一旦有人从外部做了一些修改，它们就会修改内部的值，在 React 中这被称为**不受控组件**，因为它们自己处理状态。在 React 中，你应该确保这些元素变为**受控组件**

你应该怎么做呢？你只需要设置输入框的值属性，这个值已经在 `searchTerm` 状态属性中保存了，那么为什么不从这里访问呢？

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
# leanpub-start-insert
            value={searchTerm}
# leanpub-end-insert
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

就是这样。现在输入框的单项数据流循环是自包含的，组件内部状态是输入框的唯一数据来源。

整个内部状态管理和单向数据流可能对你来说比较新，但你一旦习惯了它，你就会自然而然的在 React 中实现它。一般来说，React 带来一种新的模式，将单向数据流引入到单页面应用的生态中，到目前为止，它已经被几个框架和库所采用。

### 练习

* 阅读更多[React 表单](https://facebook.github.io/react/docs/forms.html)相关内容
# Getting Real with an API 使用真实的API

Now it's time to get real with an API, because it can get boring to deal with sample data.

现在是时候使用真实的API了，因为老是处理样本数据会变得很无聊。

If you are not familiar with APIs, I encourage you [to read my journey where I got to know APIs](https://www.robinwieruch.de/what-is-an-api-javascript/).

如果你对API不熟悉，我建议你[去读读我的博客，里面有关于我是怎样了解API的](https://www.robinwieruch.de/what-is-an-api-javascript/)。

Do you know the [Hacker News](https://news.ycombinator.com/) platform? It's a great news aggregator about tech topics. In this book, you will use the Hacker News API to fetch trending stories from the platform. There is a [basic](https://github.com/HackerNews/API) and [search](https://hn.algolia.com/api) API to get data from the platform. The latter one makes sense in the case of this application in order to search stories on Hacker News. You can visit the API specification to get an understanding of the data structure.

你知道 [Hacker News](https://news.ycombinator.com/) 这个平台吗？他是一个很棒的技术话题整合平台。在本书中，你将使用它的API来获取时事报道。这里有一个 [basic](https://github.com/HackerNews/API) API 和一个 [search](https://hn.algolia.com/api) API 用来从平台获取数据。后者让我们的应用通过 Hacker News 搜索报道成为可能。同时你可以通过访问API的说明来理解它的数据结构。

## Lifecycle Methods 生命周期方法

You will need to know about React lifecycle methods before you can start to fetch data in your components by using an API. These methods are a hook into the lifecycle of a React component. They can be used in ES6 class components, but not in functional stateless components.

在你开始在组件中通过 API 来获取数据之前，你需要知道React的生命周期方法。这些方法是嵌入 React 组件中的一组挂钩。它们可以在 ES6 类组件中使用，但是不能在无状态组件中使用。

Do you remember when a previous chapter taught you about JavaScript ES6 classes and how they are used in React? Apart from the `render()` method, there are several methods that can be overridden in a React ES6 class component. All of these are the lifecycle methods. Let's dive into them:

你还记得前一章中讲过的 JavaScript ES6 类并且他们是怎么在 React 中使用的吗？除了 `render()` 方法外，还有几种方法可以在React ES6 类组件中被复写。所有的这些方法都是生命周期方法。现在让我们来深入了解他们：

You already know two lifecycle methods that can be used in an ES6 class component: `constructor()` and `render()`.

通过之前的学习，你已经知道两种能够用在ES6类组件中的生命周期方法：`constructor()` 和 `render()`。

The constructor is only called when an instance of the component is created and inserted in the DOM. The component gets instantiated. That process is called mounting of the component.

constructor（构造函数）只有在一个组件实例化并插入到DOM中的时候才会被调用。组件被实例化的过程被称作组件的挂载。

The `render()` method is called during the mount process too, but also when the component updates. Each time when the state or the props of a component change, the `render()` method of the component is called.

`render()` 方法也会在组件挂载的过程中被调用，同时当组件更新的时候也会被调用。每当组件的状态（state）或者属性（props）改变时，组件的 `render()` 方法都会被调用。


Now you know more about the two lifecycle methods and when they are called. You already used them as well. But there are more of them.

现在对于这两个生命周期方法你知道了更多而且也知道他们什么时候会被调用了。你也已经在前面的学习中使用过他们。但是 React 里还有更多的生命周期方法。

The mounting of a component has two more lifecycle methods: `componentWillMount()` and `componentDidMount()`. The constructor is called first, `componentWillMount()` gets called before the `render()` method and `componentDidMount()` is called after the `render()` method.

一个组件的挂载中还另外两个生命周期方法被调用：`componentWillMount()` 和 `componentDidMount()`。在这四个方法中，首先被调用的是构造函数（constructor）其次是 `componentWillMount()`，再者是 `render()` ，最后是 `componentDidMount()`。

Overall the mounting process has 4 lifecycle methods. They are invoked in the following order:

总而言之在挂载过程中，这四个方法的调用顺序是这样的：

* constructor()
* componentWillMount()
* render()
* componentDidMount()

But what about the update lifecycle of a component that happens when the state or the props change? Overall it has 5 lifecycle methods in the following order:

但是当组件的状态或者属性改变的时候用来更新组件的生命周期是什么样的呢？总的来说 React 有5个生命周期方法来处理更新，他们以下面的顺序先后被调用：

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

Last but not least there is the unmounting lifecycle. It has only one lifecycle method: `componentWillUnmount()`.

最后但并非最不重要的是卸载生命周期。它只有一个生命周期方法：`componentWillUnmount()`。

After all, you don't need to know all of these lifecycle methods from the beginning. It can be intimidating yet you will not use all of them. Even in a larger React application you will only use a few of them apart from the `constructor()` and the `render()` method. Still, it is good to know that each lifecycle method can be used for specific use cases:

要知道，你不用一开始就知道所有的生命周期方法，那样可能会吓到你，然而你将不会全部使用它们。即使在一个很大的React应用当中你也只会用到一小部分除了 `constructor()` 和 `render()` 的生命周期函数。不过，了解每个生命周期方法在什么特定的时候被调用将对你有所帮助：

* **constructor(props)** - It is called when the component gets initialized. You can set an initial component state and bind class methods during that lifecycle method.

* **constructor(props)** - 它在组件初始化时被调用。在这个方法中，你可以设置初始化状态以及绑定类方法。

* **componentWillMount()** - It is called before the `render()` lifecycle method. That's why it could be used to set internal component state, because it will not trigger a second rendering of the component. Generally it is recommended to use the `constructor()` to set the initial state.

* **componentWillMount()** - 它在 `render()` 方法被调用之前调用。这就是为什么它可以被用作去设置组件内部的状态，因为它不会触发组件的再次渲染。一般来说推荐在 `constructor()` 中去设置初始化状态。

* **render()** - The lifecycle method is mandatory and returns the elements as an output of the component. The method should be pure and therefore shouldn't modify the component state. It gets an input as props and state and returns an element.

* **render()** - 这个生命周期方法是强制性的，作为组件的输出它返回（需要渲染的）元素。这个方法应该是一个纯函数因此不应该在这个方法中修改组件的状态。它把属性和状态作为输入并且返回（需要渲染的）元素

* **componentDidMount()** - It is called only once when the component mounted. That's the perfect time to do an asynchronous request to fetch data from an API. The fetched data would get stored in the internal component state to display it in the `render()` lifecycle method.

* **componentDidMount()** - 它只有当组件渲染过后才会被立刻调用。这是发起异步请求去从一个 API 获取数据的绝佳时期。获取到的数据将被保存在内部组件的状态中然后在 `render()` 生命周期方法中展示出来。

* **componentWillReceiveProps(nextProps)** - The lifecycle method is called during an update lifecycle. As input you get the next props. You can diff the next props with the previous props, by using `this.props`, to apply a different behavior based on the diff. Additionally, you can set state based on the next props.

* **componentWillReceiveProps(nextProps)** - 这个方法在一个更新生命周期（update lifecycle）中被调用。它将下一个属性作为输入。利用 `this.props`，你可以对比下一个属性和前一个属性，基于对比的结果去实现不同的行为。此外，你可以基于下一个属性来设置组件的状态

* **shouldComponentUpdate(nextProps, nextState)** - It is always called when the component updates due to state or props changes. You will use it in mature React applications for performance optimizations. Depending on a boolean that you return from this lifecycle method, the component and all its children will render or will not render on an update lifecycle. You can prevent the render lifecycle method of a component.

* **shouldComponentUpdate(nextProps, nextState)** - 它总是在组件因为状态或者属性改变而更新时被调用。你将在成熟的React应用中使用它来进行性能优化。在一个更新生命周期中，组件以及这个组件所有的子组件将根据这个方法返回的布尔值决定渲染或者不渲染。这样你可以阻止组件的渲染生命周期（render lifecycle）方法，避免不必要的渲染。

* **componentWillUpdate(nextProps, nextState)** - The lifecycle method is immediately invoked before the `render()` method. You already have the next props and next state at your disposal. You can use the method as last opportunity to perform preparations before the render method gets executed. Note that you cannot trigger `setState()` anymore. If you want to compute state based on the next props, you have to use `componentWillReceiveProps()`.

* **componentWillUpdate(nextProps, nextState)** - 这个方法在 `render()` 被调用之前立即被调用。你已经拥有下一个属性和状态，它们可以在这个方法中任由你处置。你可以利用这个方法在渲染方法被执行前进行最后的准备。注意在这个生命周期方法中你不能再触发 `setState()`。如果你想基于下一个属性计算状态，你必须利用 `componentWillReceiveProps()`。

* **componentDidUpdate(prevProps, prevState)** - The lifecycle method is immediately invoked after the `render()` method. You can use it as opportunity to perform DOM operations or to perform further asynchronous requests.

* **componentDidUpdate(prevProps, prevState)** - 这个方法在 `render()` 被调用之后立即被调用。你可以用它作为进行 DOM 的操作或者执行更多异步请求的机会。

* **componentWillUnmount()** - It is called before you destroy your component. You can use the lifecycle method to perform any clean up tasks.

* **componentWillUnmount()** - 它会在你销毁你的组件之前被调用。你可以利用这个生命周期方法去执行任何清理任务。

The `constructor()` and `render()` lifecycle methods are already used by you. These are the commonly used lifecycle methods for ES6 class components. Actually the `render()` method is required, otherwise you wouldn't return a component instance.

 `constructor()` 和 `render()` 生命周期方法已经被你使用过了。对于 ES6 类组件来说他们是常用的生命周期方法。实际上 `render()` 是必须有的，否则它将不会返回一个组件实例。

There is one more lifecycle method: `componentDidCatch(error, info)`. It was introduced in [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) and is used to catch errors in components. For instance, displaying the sample list in your application works just fine. But there could be a case when the list in the local state is set to `null` by accident (e.g. when fetching the list from an external API, but the request failed and you set the local state of the list to null). Afterward, it wouldn't be possible to filter and map the list anymore, because it is `null` and not an empty list. The component would be broken and the whole application would fail. Now, by using `componentDidCatch()`, you can catch the error, store it in your local state, and show an optional message to your application user that an error has happened.

还有另一个生命周期方法：`componentDidCatch(error, info)`。它在 [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) 中被引进并且用来捕获组件的错误。举例来说，在你的应用中显示样本列表工作的很好。但是这种情况可能会发生，列表本地的状态被意外地设置成 `null`（例如当从外部API获取列表时请求失败，然后你设置本地状态为空）。之后，它不可能再去过滤以及映射这个列表，因为它不是空列表而是 `null`。这样这个组件就会崩溃然后整个应用就会挂。现在，利用 `componentDidCatch()`，你可以捕获错误，将它存在本地的状态中，然后像用户显示一条可选信息，说明应用发生了错误。

### Exercises: 练习：

* read more about [lifecycle methods in React](https://facebook.github.io/react/docs/react-component.html)
* read more about [the state related to lifecycle methods in React](https://facebook.github.io/react/docs/state-and-lifecycle.html)
* read more about [error handling in components](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)
* 阅读更多 [React 生命周期函数](https://facebook.github.io/react/docs/react-component.html)
* 阅读更多 [React 中状态与生命周期函数的关系](https://facebook.github.io/react/docs/state-and-lifecycle.html)
* 阅读更多[组件错误处理](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Fetching Data 获取数据

Now you are prepared to fetch data from the Hacker News API. There was one lifecycle method mentioned that can be used to fetch data: `componentDidMount()`. You will use the native fetch API in JavaScript to perform the request.

现在你做好了从 Hacker News API 获取数据的准备。在前文曾提到过的 `componentDidMount()` 生命周期方法可以用来获取数据。你将使用 JavaScript 原生的 fetch API 来发起请求。

Before we can use it, let's set up the URL constants and default parameters to breakup the API request into chunks.

在我们用它之前，让我们设置好 URL 常量以及默认参数来将 API 请求分解成块。

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-end-insert

...
~~~~~~~~

In JavaScript ES6, you can use [template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals) to concatenate strings. You will use it to concatenate your URL for the API endpoint.

在JavaScript ES6中，你可以用[模板字符串（template strings）](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)去链接字符串。你将用它来拼凑访问API端点的URL。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

That will keep your URL composition flexible in the future.

那样就可以保证以后你 URL 组合的灵活性。

But let's get to the API request where you will use the url. The whole data fetch process will be presented at once, but each step will be explained afterward.

让我们开始使用 API 请求，在这个请求中将用到上述的网址。整个数据获取的过程在下面代码中一次给出，但是对于每一步之后会有详细的解释。

{title="src/App.js",lang=javascript}
~~~~~~~~
...

class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      result: null,
      searchTerm: DEFAULT_QUERY,
# leanpub-end-insert
    };

# leanpub-start-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  setSearchTopStories(result) {
    this.setState({ result });
  }

  fetchSearchTopStories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(e => e);
  }

  componentDidMount() {
    const { searchTerm } = this.state;
    this.fetchSearchTopStories(searchTerm);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

A lot of things happen in the code. I thought about breaking it into smaller pieces. Then again it would be difficult to grasp the relations of each piece to each other. Let me explain each step in detail.

这段代码做了很多事。我想把它分成更小的代码段。但是那样又会很难去理解每段代码之间的关系。接下来我就来详细解释代码中的每一步。

First, you can remove the sample list of items, because you return a real list from the Hacker News API. The sample data is not used anymore. The initial state of your component has an empty result and default search term now. The same default search term is used in the input field of the Search component and in your first request.

首先，你可以删除这些样本列表了，因为你将从 Hacker News API 得到一个真实的列表。这些样本数据已经没用了。现在组件将一个空的列表结果以及一个默认的搜索项作为初始状态。这个默认搜索项也同样用在 Search 组件的输入字段和第一个（API）请求中。

Second, you use the `componentDidMount()` lifecycle method to fetch the data after the component did mount. In the very first fetch, the default search term from the local state is used. It will fetch "redux" related stories, because that is the default parameter.

其次，在组件挂载之后，这里用了 `componentDidMount()` 生命周期方法去获取数据。在第一次获取数据时，用了在本地状态的默认搜索项。它将获取与 “redux” 相关的报道，因为它是默认的参数。

Third, the native fetch API is used. The JavaScript ES6 template strings allow it to compose the URL with the `searchTerm`. The URL is the argument for the native fetch API function. The response needs to get transformed to a JSON data structure, which is a mandatory step in a native fetch function when dealing with JSON data structures, and can finally be set as result in the internal component state. In addition, the catch block is used in case of an error. If an error happens during the request, the function will run into the catch block instead of the then block. In a later chapter of the book, you will include the error handling.

第三，这里使用了本地的提取 API 函数。JavaScript ES6 模板字符串允许组件利用 `searchTerm` 来组成 URL。该 URL 是本地提取 API 函数的参数。响应需要被转化成 JSON 的数据结构，这是在处理 JSON 数据结构时本地获取 API 函数中的强制步骤，最终可以在内部组件状态中设置为结果。此外，一个 catch 块用来以防出现错误。如果在发起请求时出现错误，这个函数会进入到 catch 中而不是 then 中。在本书之后的章节中，将涵盖错误处理。

Last but not least, don't forget to bind your new component methods in the constructor.

最后在并不是最不重要的，不要忘记在构造函数中绑定你的组件方法。

Now you can use the fetched data instead of the sample list of items. However, you have to be careful again. The result is not only a list of data. [It's a complex object with meta information and a list of hits which are in our case the stories](https://hn.algolia.com/api). You can output the internal state with `console.log(this.state);` in your `render()` method to visualize it

现在你用获取的数据去代替样本数据了。然而，你必须注意一点，这个结果不仅仅是一个数据的列表。它是一个复杂的对象，[它包含了元信息以及一系列点击，在我们这个应用中这些点击就是报道](https://hn.algolia.com/api)。你可以用 `console.log(this.state);` 将这些信息在 `render()` 方法中打印出来。

In the next step, you will use the result to render it. But we will prevent it from rendering anything, so we will return null, when there is no result in the first place. Once the request to the API succeeded, the result is saved to the state and the App component will re-render with the updated state.

在接下来的步骤中，你将把之前的得到的结果渲染出来。但是我们不会把所有得到的数据都渲染出来，所以在刚开始没有拿到结果时，我们会返回空。一旦我们发起的请求成功，结果会保存在状态里，然后 App 组件将用更新后的状态重新渲染界面。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;

    if (!result) { return null; }

# leanpub-end-insert
    return (
      <div className="page">
        ...
        <Table
# leanpub-start-insert
          list={result.hits}
# leanpub-end-insert
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Let's recap what happens during the component lifecycle. Your component gets initialized by the constructor. After that, it renders for the first time. But you prevent it from displaying anything, because the result in the local state is null. It is allowed to return null for a component in order to display nothing. Then the `componentDidMount()` lifecycle method runs. In that method you fetch the data from the Hacker News API asynchronously. Once the data arrives, it changes your internal component state in `setSearchTopStories()`. Afterward, the update lifecycle comes into play because the local state was updated. The component runs the `render()` method again, but this time with populated result in your internal component state. The component and thus the Table component with its content will be rendered.

让我们回顾一下在整个组将的生命周期中发生了什么。首先组件通过构造函数得到初始化，之后它将初始化的状态渲染出来。但是当结果为空时组件返回空，这阻止了组件的渲染。React允许组件通过返回 null 来不渲染任何东西。接着 `componentDidMount()` 生命周期函数执行。在这个方法中你从 Hacker News API 中异步地拿到了数据。一旦数据到达，组件就通过 `setSearchTopStories()` 函数改变组件内部的状态。之后，因为状态的更新，更新生命周期开始运行。组件再次执行 `render()` 方法，但这一次内部组件的状态中的结果已经被填充不再为 null。因此组件将渲染 Table 组件的内容。

You used the native fetch API that is supported by most browsers to perform an asynchronous request to an API. The *create-react-app* configuration makes sure that it is supported in every browser. There are third-party node packages that you can use to substitute the native fetch API: [superagent](https://github.com/visionmedia/superagent) and [axios](https://github.com/mzabriskie/axios).

你使用了大多数浏览器支持的本机提取 API 来执行对 API 的异步请求。*create-react-app* 中的配置保证了它被所有浏览器支持。你也可以使用第三方的库来代替本机提取 API：[superagent](https://github.com/visionmedia/superagent) 和 [axios](https://github.com/mzabriskie/axios).

Back to your application: The list of hits should be visible now. However, there are two regression bugs in the application now. First, the "Dismiss" button is broken. It doesn't know about the complex result object and still operates on the plain list from the local state when dismissing an item. Second, when the list is displayed but you try to search for something else, the list gets filtered on the client-side even though the initial search was made by searching for stories on the server-side. The perfect behvaior would be to fetch another result object from the API when using the Search component. Both regression bugs will be fixed in the following chapters.

回到你的应用中：现在应该可以看到列表了。然而，现在应用中存在两个回归错误。第一，"Dismiss" 按钮不工作。它不知道这个复杂的结果对象，并且在关闭项目时仍然从本地状态的简单列表中操作。第二，当这个列表显示出来之后，你尝试搜索其他的东西，即使初始的报道搜索在服务器端进行，该列表也会在客户端进行过滤。我们期待的行为是当我们使用 Search 组件时从 API 拿到另外的结果。两个回归错误都将在之后的章节中得到修复。

### Exercises: 练习

* read more about [ES6 template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* read more about [the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* read more about [data fetching in React](https://www.robinwieruch.de/react-fetching-data/)

* 阅读更多 [ES6 模板字符串](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* 阅读更多 [本地提取 API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* 阅读更多 [在 React 中获取数据](https://www.robinwieruch.de/react-fetching-data/)

## ES6 Spread Operators ES6 扩展操作符

The "Dismiss" button doesn't work because the `onDismiss()` method is not aware of the complex result object. It only knows about a plain list in the local state. But it isn't a plain list anymore. Let's change it to operate on the result object instead of the list itself.

“Dismiss” 按钮之所以不工作是因为 `onDismiss()` 方法不能识别复杂的结果对象。它只能识别一个平铺的本地状态列表。但是它已经不再是一个平铺的列表了。让我们去操作这个结果对象而不是直接使用它。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
# leanpub-start-insert
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

But what happens in `setState()` now? Unfortunately the result is a complex object. The list of hits is only one of multiple properties in the object. However, only the list gets updated, when an item gets removed in the result object, while the other properties stay the same.

那现在 `setState()` 中发生了什么呢？不幸的是这个结果是一个复杂的对象。hits 字段的列表只是这个对象中所有属性中的一个。然而，当某一项从结果对象中移除时，只有列表得到了更新其他的属性还是保持原样。

One approach could be to mutate the hits in the result object. I will demonstrate it, but we won't do it that way.

有一种解决方法是直接改变 result 对象中的 hits 字段。我将演示这个方法，但实际上我们不会这样去做。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// don`t do this
this.state.result.hits = updatedHits;
~~~~~~~~

React embraces immutable data structures. Thus you shouldn't mutate an object (or mutate the state directly). A better approach is to generate a new object based on the information you have. Thereby none of the objects get altered. You will keep the immutable data structures. You will always return a new object and never alter an object.

React 拥护不可变的数据结构。因此你不应该改变一个对象（或者直接改变状态）。更好的做法是基于现在拥有的资源创建一个新对象。这样没有任何对象被改变。你将保持不变的数据结构。你将永远只返回一个新对象并且不会改变任何对象。

Therefore you can use JavaScript ES6 `Object.assign()`. It takes as first argument a target object. All following arguments are source objects. These objects are merged into the target object. The target object can be an empty object. It embraces immutability, because no source object gets mutated. It would look similar to the following:

因此你可以用 JavaScript ES6 中的 `Object.assign()` 函数。它接收第一个参数作为目标对象。后面的所有参数作为源对象。所有的源对象被合并到目标对象中。目标对象可以是一个空对象。这种做法是拥抱不变性的，因为没有任何源对象被改变。类似下面的写法：

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Latter objects will override former merged objects when they share the same property names. Now let's do it in the `onDismiss()` method:

当遇到相同的属性时，排在后面的对象会复写先前合并的对象。现在让我们用它来改写 `onDismiss()` 方法：

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: Object.assign({}, this.state.result, { hits: updatedHits })
# leanpub-end-insert
  });
}
~~~~~~~~

That would already be the solution. But there is a simpler way in JavaScript ES6 and future JavaScript releases. May I introduce the spread operator to you? It only consists of three dots: `...` When it is used, every value from an array or object gets copied to another array or object.

那样写已经是一个解决方案了。但是在 JavaScript ES6 以及之后的 JavaScript 版本中有一个更简单的方法。现在我将向你介绍扩展操作符。它只由三个点组成：`...`。当使用它时，一个数组或者对象所有的值会被拷贝到另一个数组或对象。

Let's examine the ES6 **array** spread operator even though you don't need it yet.

让我们先来看一下 ES6 中**数组**的扩展运算符，即使你还不需要用它。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

The `allUsers` variable is a completely new array. The other variables `userList` and `additionalUser` stay the same. You can even merge two arrays that way into a new array.

`allUsers` 变量是一个全新的数组。变量 `userList` 和 `additionalUser` 还是和原来一样。这样做你甚至可以合并两个数组到一个新的数组中。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Now let's have a look at the object spread operator. It is not JavaScript ES6. It is a [proposal for a next JavaScript version](https://github.com/sebmarkbage/ecmascript-rest-spread) yet already used by the React community. That's why *create-react-app* incorporated the feature in the configuration.

现在让我们来看看对象的扩展运算符。它并不是 JavaScript ES6 中的。它是[针对下一个JavaScript版本的提案](https://github.com/sebmarkbage/ecmascript-rest-spread)，然而已经被 React 社区所使用。这就是为什么 *create-react-app* 在配置中加入了这个功能。

Basically it is the same as the JavaScript ES6 array spread operator but with objects. It copies each key value pair into a new object.

对象的扩展运算符和 JavaScript ES6 中数组的扩展运算符基本一样。它拷贝每一个键值对到一个新的对象。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Multiple objects can be spread like in the array spread example.

多个对象的扩展可以像之前数组的例子一样。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

After all, it can be used to replace `Object.assign()`.

最终，它可以用来代替 `Object.assign()`。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: { ...this.state.result, hits: updatedHits }
# leanpub-end-insert
  });
}
~~~~~~~~

Now the "Dismiss" button should work again, because the `onDismiss()` method is aware of the complex result object and how to update it after dismissing an item from the list.

现在 "Dismiss" 按钮应该可以再次工作了，因为 `onDismiss()` 方法已经知道这个复杂的 result 对象，并且知道当要忽略掉列表中的某一项时怎么去更新列表了。

### Exercises: 练习

* read more about the [ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* read more about the [ES6 array spread operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * the object spread operator is briefly mentioned

* 阅读更多 [ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* 阅读更多 [ES6 数组的扩展操作符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * 对象的扩展操作符在其中也被简单提到

## Conditional Rendering 条件渲染

The conditional rendering is introduced pretty early in React applications. But not in the case of the book, because there wasn't such an use case yet. The conditional rendering happens when you want to make a decision to render either one or another element. Sometimes it means to render an element or nothing. After all, a conditional rendering simplest usage can be expressed by an if-else statement in JSX.

条件渲染很早就被 React 应用引入。单本书中还没有实例，因为目前为止本书中还没有这样的需求。条件渲染发生在当你想要决定渲染一个元素或另一个元素时。有些时候也意味着渲染一个元素或者什么都不渲染。毕竟，一个条件渲染最简单的用法可用 JSX 语法中的 if-else 来实现。

The `result` object in the internal component state is `null` in the beginning. So far, the App component returned no elements when the `result` hasn't arrived from the API. That's already a conditional rendering, because you return earlier from the `render()` lifecycle method for a certain condition. The App component either renders nothing or its elements.

当 `result` 对象在组件内部的状态为 `null` 并且 `result` 还没有从 API 到达时，App 组件没有返回任何元素。这也是一个条件渲染，因为在 `render()` 生命周期方法中你根据一个特定的条件返回值。根据条件，App 组件什么都不渲染或者渲染它的元素。

But let's go one step further. It makes more sense to wrap the Table component, which is the only component that depends on the `result`, in an independent conditional rendering. Everything else should be displayed, even though there is no `result` yet. You can simply use a ternary operator in your JSX.

但是让我们更深入一步。因为只有 Table 组件的渲染依赖于 `result`，所以将它包裹在一个独立的条件渲染中比较合理。其它的所有组件应该被渲染，即使 `result` 为空。你可以简单地在 JSX 中使用三目运算符来达到这样的目的。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
        </div>
# leanpub-start-insert
        { result
          ? <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
          : null
        }
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

That's your second option to express a conditional rendering. A third option is the logical `&&` operator. In JavaScript a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false.

这是你的第二个选择来实现条件渲染。第三个选择是运用 `&&` 逻辑运算符。在JavaScript中 `true && 'Hello World'` 的值永远是 “Hello World”。`false && 'Hello World'` 的值永远是 false。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
~~~~~~~~

In React you can make use of that behavior. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores and skips the expression. It is applicable in the Table conditional rendering case, because it should return a Table or nothing.

在 React 中你可以利用这样的行为。如果条件判断为 true，`&&` 操作符后边的表达式的值将会被输出。如果条件判断为 false，React 将会忽略并跳过表达式。这个可以运用在 Table 组件的条件渲染中，因为它返回一个 Table 组件或者什么都不返回。

{title="src/App.js",lang=javascript}
~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

These were a few approaches to use conditional rendering in React. You can read about [more alternatives in an exhaustive list of examples for conditional rendering approaches](https://www.robinwieruch.de/conditional-rendering-react/). Moreover you will get to know their different use cases and when to apply them.

这里有一些运用条件渲染的方法。你可以阅读[更多的条件渲染选择：一个详尽的列子列表](https://www.robinwieruch.de/conditional-rendering-react/)这篇文章了解更多。读完它你将了解他们不同的用例已经什么时候该用他们。

After all, you should be able to see the fetched data in your application. Everything except the Table is displayed when the data fetching is pending. Once the request resolves the result and stores it into the local state, the Table is displayed because the `render()` method runs again and the condition in the conditional rendering resolves in favor of displaying the Table component.

现在，你应该能够在你的应用中看到获取的数据。但数据正在获取时你可以看到除了 Table 组件以外的所有东西。一旦请求完成数据存入本地状态，Table 组件将被渲染出来。因为 `render()` 方法再次运行并且条件渲染判断的结果为渲染 Table 组件。

### Exercises: 练习

* read more about [React conditional rendering](https://facebook.github.io/react/docs/conditional-rendering.html)
* read more about [different ways for conditional renderings](https://www.robinwieruch.de/conditional-rendering-react/)

* 阅读更多 [React条件渲染](https://facebook.github.io/react/docs/conditional-rendering.html)
* 阅读更多 [实现条件渲染的不同方法](https://www.robinwieruch.de/conditional-rendering-react/)

## Client- or Server-side Search 客货端或服务端搜索

When you use the Search component with its input field now, you will filter the list. That's happening on the client-side though. Now you are going to use the Hacker News API to search on the server-side. Otherwise you would deal only with the first API response which you got on `componentDidMount()` with the default search term parameter.

现在当你使用 Search 组件的输入栏时，你将过滤列表。虽然这在客户端发生。现在你将使用 Hacker News API 在服务器端进行搜索。否则你将只能处理第一次从 `componentDidMount()` 拿到的默认搜索 API 响应。

You can define an `onSearchSubmit()` method in your App component which fetches results from the Hacker News API when executing a search in the Search component. It will be the same fetch as in your `componentDidMount()` lifecycle method, but this time with a modified search term from the local state and not with the initial default search term.

你可以在你的 App 组件中定义一个 `onSearchSubmit()` 方法。当在 Search 组件中执行搜索时，这个方法用来从 Hacker News API 获取结果。这与 `componentDidMount()` 生命周期方法中的获取数据相同，但是这次搜索的的术语从本地状态中得到了改变，并且不用初始化设定的默认搜索术语。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-start-insert
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  onSearchSubmit() {
    const { searchTerm } = this.state;
    this.fetchSearchTopStories(searchTerm);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Now the Search component has to add an additional button. The button has to explicitly trigger the search request. Otherwise you would fetch data from the Hacker News API every time when your input field changes. But you want to do it explicitly in a on click handler.

现在 Search 组件不得不增加一个按钮。这个按钮必须明确地出发搜索请求。否则每次当你输入改变你的输入框中的值时，你就从 Hacker News API 获取数据。但是你想要的是用一个明确地点击事件来发起请求。

As alternative you could debounce (delay) the `onChange()` function and spare the button, but it would add more complexity at this time and maybe wouldn't be the desired effect. Let's keep it simple without a debounce for now.

作为另一个选择，你可以通过去抖（延迟）`onChange()` 函数来节省按钮，但是那样会增加复杂度并且可能不值得这么做。让我们现在就用简单的方式来做。

First, pass the `onSearchSubmit()` method to your Search component.

首先，传递 `onSearchSubmit()` 方法到你的 Search 组件。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
# leanpub-start-insert
            onSubmit={this.onSearchSubmit}
# leanpub-end-insert
          >
            Search
          </Search>
        </div>
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
~~~~~~~~

Second, introduce a button in your Search component. The button has the `type="submit"` and the form uses its `onSubmit()` attribute to pass the `onSubmit()` method. You can reuse the children property, but this time it will be used as the content of the button.

第二，在你的 Search 组件中引入按钮。这个按钮有 `type="submit"` 属性并且表单用它的 `onSubmit()` 属性去传递 `onSubmit()` 方法。你可以复用 children 属性，但在这里它将被用作按钮的显示内容。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) =>
  <form onSubmit={onSubmit}>
    <input
      type="text"
      value={value}
      onChange={onChange}
    />
    <button type="submit">
      {children}
    </button>
  </form>
# leanpub-end-insert
~~~~~~~~

In the Table, you can remove the filter functionality, because there will be no client-side filter (search) anymore. Don't forget to remove the `isSearched()` function as well. It will not be used anymore. The result comes directly from the Hacker News API now after you have clicked the "Search" button.

在 Table 组件中，你可以移除过滤功能，因为已经没有客户端过滤（搜索）了。同时别忘记移除 `isSearched()` 函数。它也不再使用了。现在，当你点击 “Search” 按钮时，结果直接从 Hacker News API 中得到。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        ...
        { result &&
          <Table
# leanpub-start-insert
            list={result.hits}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
      </div>
    );
  }
}

...

# leanpub-start-insert
const Table = ({ list, onDismiss }) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {list.map(item =>
# leanpub-end-insert
      ...
    )}
  </div>
~~~~~~~~

When you try to search now, you will notice that the browser reloads. That's a native browser behavior for a submit callback in a HTML form. In React you will often come across the `preventDefault()` event method to suppress the native browser behavior.

现在当你尝试去搜索时，你将注意到浏览器重新加载了。这是本地浏览器在提交一个 HTML 表单后的行为。在 React 中，你会经常遇到用 `preventDefault()` 事件方法来抑制本地浏览器行为。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
onSearchSubmit(event) {
# leanpub-end-insert
  const { searchTerm } = this.state;
  this.fetchSearchTopStories(searchTerm);
# leanpub-start-insert
  event.preventDefault();
# leanpub-end-insert
}
~~~~~~~~

Now you should be able to search different Hacker News stories. Perfect, you interact with a real world API. There should be no client-side search anymore.

现在你能搜索不同的报道了。非常好，你现在与一个真正 API 打交道了。不再需要客户端的搜索了。

### Exercises: 练习

* read more about [synthetic events in React](https://facebook.github.io/react/docs/events.html)
* experiment with the [Hacker News API](https://hn.algolia.com/api)

* 阅读更多 [React 中的合成事件](https://facebook.github.io/react/docs/events.html)
* 试一试 [Hacker News API](https://hn.algolia.com/api)

## Paginated Fetch 分页抓取

Did you have a closer look at the returned data structure yet? The [Hacker News API](https://hn.algolia.com/api) returns more than a list of hits. Precisely it returns a paginated list. The page property, which is `0` in the first response, can be used to fetch more paginated sublists as result. You only need to pass the next page with the same search term to the API.

你仔细看过返回回来的数据结构吗？[Hacker News API](https://hn.algolia.com/api) 不仅仅只返回 hits 的列表。确切的说它返回一个分页列表。页面属性（在第一个响应中为“0”）可用于获取更多分页的子列表。你只需要将具有相同搜索字词的下一页传递给 API。

Let's extend the composable API constants so that it can deal with paginated data.

让我们扩展一下可组合的 API 常量，以便处理分页数据。

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert
~~~~~~~~

Now you can use the new constant to add the page parameter to your API request.

现在你可以使用新常量将页面参数添加到你的 API 请求中。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

The `fetchSearchTopStories()` method will take the page as second argument. If you don't provide the second argument, it will fallback to the `0` page for the initial request. Thus the `componentDidMount()` and `onSearchSubmit()` methods fetch the first page on the first request. Every additional fetch should fetch the next page by providing the second argument.

`fetchSearchTopStories()` 函数接收页面作为第二个参数。如果你不提供第二个参数，它将使用‘0’作为初始参数发起请求。因此 `componentDidMount()` 和 `onSearchSubmit()` 方法在第一个请求获取第一页。每个追加的获取将根据提供的第二个参数抓取下一个页面。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
# leanpub-end-insert
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(e => e);
  }

  ...

}
~~~~~~~~

Now you can use the current page from the API response in `fetchSearchTopStories()`. You can use this method in a button to fetch more stories on a `onClick` button handler. Let's use the Button to fetch more paginated data from the Hacker News API. You only need to define the `onClick()` handler which takes the current search term and the next page (current page + 1).

现在你可以使用从调用 `fetchSearchTopStories()` 函数返回的响应中使用当前的页面。你可以在一个按钮中通过 `onClick` 事件处理，并使用这个方法来抓取更多的报道。让我用这个按钮从 Hacker News API 中获取更多的分页数据。你只需要定义 `onClick()` 事件处理器，这个处理器以当前的搜索关键字和下一页作为参数（当前页码+1）。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
# leanpub-start-insert
    const page = (result && result.page) || 0;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-start-insert
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopStories(searchTerm, page + 1)}>
            More
          </Button>
        </div>
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

In addition, in your `render()` method you should make sure to default to page 0 when there is no result yet. Remember that the `render()` method is called before the data is fetched asynchronously in the `componentDidMount()` lifecycle method.

此外，当还没有返回结果时，在你的 `render()` 方法中你应该保证你的默认分页是0。记住 `render()` 方法是在 `componentDidMount()` 生命周期方法中异步获取数据之前调用的。

There is one step missing. You fetch the next page of data, but it will override your previous page of data. It would be ideal to concatenate the old and new list of hits from the local state and new result object. Let's adjust the functionality to add the new data rather than to override it.

这里还遗漏了一步。你抓取下一个分页的数据，但新数据会覆盖你之前的分页数据。理想的情况下，新的列表和老的列表应该在本地状态和新 result 对象中连接起来。让我们来实现将新的数据添加到老的数据上而不是覆盖它。

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
# leanpub-start-insert
  const { hits, page } = result;

  const oldHits = page !== 0
    ? this.state.result.hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    result: { hits: updatedHits, page }
  });
# leanpub-end-insert
}
~~~~~~~~

A couple of things happen in the `setSearchTopStories()` method now. First, you get the hits and page from the result.

现在 `setSearchTopStories()` 方法中发生了一些事情。首先，你从 result 对象中拿到 hits 字段和 page 字段。

Second, you have to check if there are already old hits. When the page is 0, it is a new search request from `componentDidMount()` or `onSearchSubmit()`. The hits are empty. But when you click the "More" button to fetch paginated data the page isn't 0. It is the next page. The old hits are already stored in your state and thus can be used.

第二，你必须检查老的 hits 字段是否存在。当页码为0时，它是一个来自 `componentDidMount()` 或者 `onSearchSubmit()` 方法的新的搜索请求。hits 现在是空的。但是当你通过点击 “More” 按钮去抓取更过的分页数据时，页码不为0。此时它是下一个页码。老的 hits 已储存在状态中并且可比被使用。

Third, you don't want to override the old hits. You can merge old and new hits from the recent API request. The merge of both lists can be done with the JavaScript ES6 array spread operator.

第三，你不想覆盖老的 hits。你可以合并老的 hits 和从当前 API 返回的新的 hits。这个列表的合并可以通过 JavaScript ES6 数据扩展操作符完成。

Fourth, you set the merged hits and page in the local component state.

第四，你将合并后的 hits 和页码设置到本地组件的状态中。

You can make one last adjustment. When you try the "More" button it only fetches a few list items. The API URL can be extended to fetch more list items with each request. Again, you can add more composable path constants.

你可以做一个最后的调整。当你尝试点击 “More” 按钮时，它只抓取一个新的列表。在每个请求的中，你可以扩展 API URL 以获取更多列表项。再者，你可以添加更多的可组合路径常量。

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';
# leanpub-start-insert
const DEFAULT_HPP = '100';
# leanpub-end-insert

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
# leanpub-start-insert
const PARAM_HPP = 'hitsPerPage=';
# leanpub-end-insert
~~~~~~~~

Now you can use the constants to extend the API URL.

现在你可以使用常量来扩展 API URL。

{title="src/App.js",lang=javascript}
~~~~~~~~
fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-end-insert
    .then(response => response.json())
    .then(result => this.setSearchTopStories(result))
    .catch(e => e);
}
~~~~~~~~

Afterward, the request to the Hacker News API fetches more list items in one request than before. As you can see, a powerful such as the Hacker News API gives you plenty of ways to experiment with real world data. You should make use of it to make your endeavours when learning something new more exciting. That's [how I learned about the empowerment that APIs provide](https://www.robinwieruch.de/what-is-an-api-javascript/) when learning a new programming language or library.

之后，这个对 Hacker News API 的请求能比以前在一个请求中获取更多列表项。正如你所看到的，像 Hacker News API 这样的强大功能，为你提供了大量的真实数据来做练习。学习新的东西时，你应该利用它来做出更多努力。这就是在我学习一门新的编程语言或者库时[如何利用API提供的便利](https://www.robinwieruch.de/what-is-an-api-javascript/)。

### Exercises: 练习

* experiment with the [Hacker News API parameters](https://hn.algolia.com/api)

* 练习 [Hacker News API 变量](https://hn.algolia.com/api)

## Client Cache 用户缓存

Each search submit makes a request to the Hacker News API. You might search for "redux", followed by "react" and eventually "redux" again. In total it makes 3 requests. But you searched for "redux" twice and both times it took a whole asynchronous roundtrip to fetch the data. In a client-sided cache you would store each result. When a request to the API is made, it checks if a result is already there. If it is there, the cache is used. Otherwise an API request is made to fetch the data.

每一个提交都会生成一个对 Hacker News API 的请求。你可能搜索 “redux”，然后搜索 “react”，最后再次搜索 “redux”。它总共发起了3次请求。但是你搜索了 “redux” 两次并且每一次都会执行一次异步操作去获取数据。在客户端的缓存中，你将保存每一个搜索结果。当发起一个请求时，它首先检查这个请求的结果是否已经在缓存中。如果在，那就使用缓存。否则发起一个新的 API 去获取数据。

In order to have a client cache for each result, you have to store multiple `results` rather than one `result` in your internal component state. The results object will be a map with the search term as key and the result as value. Each result from the API will be saved by search term (key).

为了实现在客户端对每一个结果进行缓存，你必须在你的内部组件的状态中存储过个`结果`而不是一个。这些结果对象将会与搜索属于映射成一个键值对。每一个从 API 得到的结果会通过搜索的术语（key）保存下来。

At the moment, your result in the local state looks similar to the following:

此时，在本地状态中，你的result看起像：

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Imagine you have made two API requests. One for the search term "redux" and another one for "react". The results object should look like the following:

想想一下你已经发起了两次 API 请求。一次搜索 “redux”，另一次搜索 “react”。你的 results 对象应该看起来像：

{title="Code Playground",lang="javascript"}
~~~~~~~~
results: {
  redux: {
    hits: [ ... ],
    page: 2,
  },
  react: {
    hits: [ ... ],
    page: 1,
  },
  ...
}
~~~~~~~~

Let's implement a client-side cache with React `setState()`. First, rename the `result` object to `results` in the initial component state. Second, define a temporary `searchKey` which is used to store each `result`.

让我们用 React 的 `setState()` 方法来实现一个客户端缓存。首先，在初始化组件状态中重命名 `result` 对象为 `results`。第二，定义一个临时的 `searchKey` 用来储存每个 `result`。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      results: null,
      searchKey: '',
# leanpub-end-insert
      searchTerm: DEFAULT_QUERY,
    };

    ...

  }

  ...

}
~~~~~~~~

The `searchKey` has to be set before each request is made. It reflects the `searchTerm`. You might wonder: Why don't we use the `searchTerm` in the first place? That's a crucial part to understand before continuing with the implementation. The `searchTerm` is a fluctuant variable, because it gets changed every time you type into the Search input field. However, in the end you will need a non fluctuant variable. It determines the recent submitted search term to the API and can be used to retrieve the correct result from the map of results. It is a pointer to your current result in the cache and thus can be used to display the current result in your `render()` method.

`searchKey` 必须在发起请求之前设值。它将映射到 `searchTerm` 上。你可能会想：为什么我们不一开始就用 `searchTerm` 呢？这是在我们继续之前需要理解的重点。`searchTerm` 是一个动态的变量，因此它在每次输入搜索关键字后发生变化。然而，最终你学要一个静态的变量。它确定了最近提交给 API 的搜索词，并可用于从结果集中检索正确的结果。它是一个指向缓存中当前结果的指针，因此可以用来在 `render()` 方法中显示当前结果。

{title="src/App.js",lang=javascript}
~~~~~~~~
componentDidMount() {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
}

onSearchSubmit(event) {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
  event.preventDefault();
}
~~~~~~~~

Now you have to adjust the functionality where the result is stored to the internal component state. It should store each result by `searchKey`.

现在，你必须修改将结果存储到内部组件状态中的功能了。它应该用 `searchKey` 来存储每个结果。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    const { hits, page } = result;
# leanpub-start-insert
    const { searchKey, results } = this.state;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];
# leanpub-end-insert

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    this.setState({
# leanpub-start-insert
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
# leanpub-end-insert
    });
  }

  ...

}
~~~~~~~~

The `searchKey` will be used as the key to save the updated hits and page in a `results` map.

`searchKey` 将会被用作键（key）来保存更新后的 `results` 集中的 hits 和 page。

First, you have to retrieve the `searchKey` from the component state. Remember that the `searchKey` gets set on `componentDidMount()` and `onSearchSubmit()`.

首先，你必须从组件状态中检索 `searchKey`。记住 `searchKey` 是在 `componentDidMount()` 和 `onSearchSubmit()` 中被设置的。

Second, the old hits have to get merged with the new hits as before. But this time the old hits get retrieved from the `results` map with the `searchKey` as key.

第二，像之前一样老的 hits 必须被合并到新的 hits 中。但是这一次老的 hits 可以用 `searchKey` 作为键从 `results` 集中检索出来。

Third, a new result can be set in the `results` map in the state. Let's examine the `results` object in `setState()`.

第三，在状态中一个新的 result 可以被设置在 `results` 集中。让我们来看一下 `setState()` 中的 `results` 对象。

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

The bottom part makes sure to store the updated result by `searchKey` in the results map. The value is an object with a hits and page property. The `searchKey` is the search term. You already learned the `[searchKey]: ...` syntax. It is an ES6 computed property name. It helps you to allocate values dynamically in an object.

下面部分的代码保证了更新后的 result 对象用 `searchKey` 作为键保存在 results 集中。这个键的值是一个拥有 hits 和 page 属性的对象。而 `searchKey` 是搜索词。这样你就已经学会 `[searchKey]: ...` 这样的语法。这个语法中，ES6 通过计算得到的属性名，它可以帮助你动态分配对象的值。

The upper part needs to spread all other results by `searchKey` in the state by using the object spread operator. Otherwise you would lose all results that you have stored before.

上面部分的代码，通过对象扩展运算符，将所有的其他包含在 results 集中的对象通过 `searchKey` 展开。否则，你将失去之前所有储存过的 results。

Now you store all results by search term. That's the first step to enable your cache. In the next step, you can retrieve the result depending on the non fluctuant `searchKey` from your map of results. That's why you had to introduce the `searchKey` in the first place as non fluctuant variable. Otherwise the retrieval would be broken when you would use the fluctuant `searchTerm` to retrieve the current result, because this value might change when you would use the Search component.

现在你通过搜索词将所有 results 储存起来。这是实现缓存的第一步。接下来，你可以根据不变的 `searchKey` 从 results 集中检索 result。这就是为什么一开始你必须引进 `searchKey` 作为一个静态变量。否则，当你用动态的 `searchTerm` 去检索当前的 result 时，这个检索过程会崩溃，以为这个值可能在你使用 Search 组件时已经被改变了。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        <div className="interactions">
# leanpub-start-insert
          <Button onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
# leanpub-end-insert
            More
          </Button>
        </div>
      </div>
    );
  }
}
~~~~~~~~

Since you default to an empty list when there is no result by `searchKey`, you can spare the conditional rendering for the Table component now. Additionally you will need to pass the `searchKey` rather than the `searchTerm` to the "More" button. Otherwise your paginated fetch depends on the `searchTerm` value which is fluctuant. Moreover make sure to keep the fluctuant `searchTerm` property for the input field in the "Search" component.

由于你没有 `searchKey` 的 result 时默认为空列表，所以你现在可以节省 Table 组件的条件渲染了。此外，你学要传 `searchKey` 给 “More” 按钮而不是 `searchTerm`。否则，你抓取分页的搜索词将是 `searchTerm` 这个可变的值。另外，确保 “Search” 组件的输入字段用的是动态的 `searchTerm` 属性。

The search functionality should work again. It stores all results from the Hacker News API.

这样，搜索功能应该可以再次工作了。它会保存所有从 Hacker News API 返回的结果。

Additionally the `onDismiss()` method needs to get improved. It still deals with the `result` object. Now it has to deal with multiple `results`.

此外，`onDismiss()` 方法也需要优化。它仍然处理 `result` 对象。现在它必须处理 `results` 了。

{title="src/App.js",lang=javascript}
~~~~~~~~
  onDismiss(id) {
# leanpub-start-insert
    const { searchKey, results } = this.state;
    const { hits, page } = results[searchKey];
# leanpub-end-insert

    const isNotId = item => item.objectID !== id;
# leanpub-start-insert
    const updatedHits = hits.filter(isNotId);

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
    });
# leanpub-end-insert
  }
~~~~~~~~

The "Dismiss" button should work again.

“Dismiss” 按钮应该可以再次工作了。

However, nothing stops the application from sending an API request on each search submit. Even though there might be already a result, there is no check that prevents the request. Thus the cache functionality is not complete yet. It caches the results, but it doesn't make use of them. The last step would be to prevent the API request when a result is available in the cache.

然后，现在还没有任何东西阻止应用每次都对一个搜索提交发起一次 API 请求，还没有任何阻止请求的检查。因此缓存功能仍然没有完善。应用缓存了所有 result，但它还没有将这些 result 利用起来。所有我们的最后一步就是当缓存中存在可以用的 result 时，阻止发起 API 请求。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

# leanpub-start-insert
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
# leanpub-end-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  needsToSearchTopStories(searchTerm) {
    return !this.state.results[searchTerm];
  }
# leanpub-end-insert

  ...

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
# leanpub-start-insert

    if (this.needsToSearchTopStories(searchTerm)) {
      this.fetchSearchTopStories(searchTerm);
    }

# leanpub-end-insert
    event.preventDefault();
  }

  ...

}
~~~~~~~~

Now your client makes a request to the API only once although you search for a search term twice. Even paginated data with several pages gets cached that way, because you always save the last page for each result in the `results` map. Isn't that a powerful approach to introduce caching to your application? The Hacker News API provides you with everything you need to even cache paginated data effectively.

现在你的客户端只会发起一次请求就是你搜索了同一个词两次。甚至是分页的数据也会以这种方式缓存，因为你总是将每个结果的最后一页保存在 `results` 集中。这不是一个强大的方法来将缓存引入到你的应用程序中吗？Hacker News API 为你提供所需的一切，甚至可以有效缓存分页数据。

## Error Handling 错误处理

Everything is in place for your interactions with the Hacker News API. You even have introduced an elegant way to cache your results from the API and make use of its paginated list functionality to fetch an endless list of sublists of stories from the API. But there is one piece missing. Unfortunately it is often missed when developing applications nowadays: error handling. It is too easy to implement the happy path without worrying about the errors that can happen along the way.

你与 Hacker News API 交互的所有东西都已就位。你甚至已经引入了一种优雅的方式来缓存来自 API 的结果，并利用分页列表功能从 API 中获取无尽的报道子列表。但是还忽略了一个东西。不幸的是，在日常开发中这个东西经常被遗忘：错误处理。不用担心错误的发生，只实现主逻辑这样的做法实在太轻松了。

In this chapter, you will introduce an efficient solution to add error handling for your application in case of an erroneous API request. You have already learned about the necessary building blocks in React to introduce error handling: local state and conditional rendering. Basically, the error is only another state in React. When an error occurs, you will store it in the local state and display it with a conditional rendering in your component. That's it. Let's implement it in the App component, because it's the component that is used to fetch the data from the Hacker News API in the first place. First, you have to introduce the error in the local state. It is initialized as null, but will be set to the error object in case of an error.

在本章中，你将引进一个高效的解决方法去为你的应用添加一个错误处理，以防发生错误的 API 请求。其实你已经学过了在 React 中构建错误处理的必要模块：本地状态和条件渲染。从根本上说，错误只是 React 的另一种状态。当一个错误发生时，你将它存在本地状态中然后利用条件渲染显示在组件中。就是这么简单。让我们在 App 组件中实现它，因为它是一开始向 Hacker News API 获取数据的组件。首先，你要在本地状态中引入 error 状态。它被初始化为 null，但当错误发生时会被置成一个 error 对象。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
# leanpub-start-insert
      error: null,
# leanpub-end-insert
    };

    ...
  }

...

}
~~~~~~~~

Second, you can use the catch block in your native fetch to store the error object in the local state by using `setState()`. Every time the API request isn't successful, the catch block would be executed.

第二，你可以使用 `setState()` 在本地获取中通过 catch 块来存储错误对象。每次 API 请求不成功，catch 块将被执行。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(e => this.setState({ error: e }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

Third, you can retrieve the error object from your local state in the `render()` method and display a message in case of an error by using React's conditional rendering.

第三，如果错误发生了，你可以在 `render()` 方法中从你的本地状态中检索 error 对象，然后利用条件渲染来显示一个信息。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
# leanpub-start-insert
      error
# leanpub-end-insert
    } = this.state;

    ...

# leanpub-start-insert
    if (error) {
      return <p>Something went wrong.</p>;
    }
# leanpub-end-insert

    return (
      <div className="page">
        ...
      </div>
    );
  }
}
~~~~~~~~

That's it. If you want to test that your error handling is working, you can change the API URL to something else that is non existent.

就这么简单。如果你想测试你的错误处理是否工作，你可以把 API URL 换成一个不存在的 URL。

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

Afterward, you should get the error message instead of your application. It is up to you where you want to place the conditional rendering for the error message. In this case, the whole app isn't displayed anymore. That wouldn't be the best user experience. So what about displaying either the Table component or the error message? The remaining application would still be visible in case of an error.

之后，你应该得到一个错误信息而不是你的应用。在哪里为你的错误信息放置条件渲染由你自己决定。在上述代码下，整个 app 不再被显示。这并不是最佳用户体验。那么该如何显示 Table 组件和错误信息呢？答案是即使发生错误，应用的其余部分应该仍然保持可见。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
            list={list}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

In the end, don't forget to revert the URL for the API to the existent one.

最后，别忘了把 URL 还原成一个真实存在的 URL。

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

Your application should still work, but this time with error handling in case the API request fails.

你的应用应该仍然可以工作，但是这一次如果 API 请求失败应用会有错误处理。

### Exercises: 练习

* read more about [React's Error Handling for Components](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

* 阅读更多 [React 组件的错误处理](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

{pagebreak} 分页符

You have learned to interact with an API in React! Let's recap the last chapters:

你已经学会用 React 去与 API 交互了！让我们回顾一下本章内容：

* React
  * ES6 class component lifecycle methods for different use cases
  * componentDidMount() for API interactions
  * conditional renderings
  * synthetic events on forms
  * error handling
* ES6
  * template strings to compose strings
  * spread operator for immutable data structures
  * computed property names
* General
  * Hacker News API interaction
  * native fetch browser API
  * client- and server-side search
  * pagination of data
  * client-side caching

* React
  * 针对不同用例的ES6类组件生命周期方法
  * componentDidMount() 如何用于 API 交互
  * 条件渲染
  * 表单上的合成事件
  * 错误处理
* ES6
  * 用模板字符串去组合字符串
  * 扩展运算符用于不可变数据结构
  * 可计算的属性名称
* General
  * Hacker News API交互
  * 本机提取浏览器 API
  * 客户端和服务器端搜索
  * 分页数据
  * 客户端缓存

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far.

再次强调，此时小憩一下，将学习的消化理解并应用。你可以试验迄今为止编写的源代码。

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.3).

你可以在官方仓库中找到[源码](https://github.com/rwieruch/hackernews-client/tree/4.3).
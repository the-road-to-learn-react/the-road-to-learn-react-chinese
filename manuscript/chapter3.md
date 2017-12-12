# Getting Real with an API 使用真实的API

Now it's time to get real with an API, because it can get boring to deal with sample data.

现在是时候使用真实的API了，老是处理样本数据会变得很无聊。

If you are not familiar with APIs, I encourage you [to read my journey where I got to know APIs](https://www.robinwieruch.de/what-is-an-api-javascript/).

如果你对API不熟悉，我建议你[去读读我的博客，里面有关于我是怎样了解API的](https://www.robinwieruch.de/what-is-an-api-javascript/)。

Do you know the [Hacker News](https://news.ycombinator.com/) platform? It's a great news aggregator about tech topics. In this book, you will use the Hacker News API to fetch trending stories from the platform. There is a [basic](https://github.com/HackerNews/API) and [search](https://hn.algolia.com/api) API to get data from the platform. The latter one makes sense in the case of this application in order to search stories on Hacker News. You can visit the API specification to get an understanding of the data structure.

你知道 [Hacker News](https://news.ycombinator.com/) 这个平台吗？它是一个很棒的技术新闻整合平台。在本书中，你将使用它的API来获取热门资讯。它有一个 [基础](https://github.com/HackerNews/API) API 和一个 [搜索](https://hn.algolia.com/api) API 来获取数据。后者使我们可以去搜索Hacker News上的资讯。你也可以通过API规范来理解它的数据结构。

## Lifecycle Methods 生命周期方法

You will need to know about React lifecycle methods before you can start to fetch data in your components by using an API. These methods are a hook into the lifecycle of a React component. They can be used in ES6 class components, but not in functional stateless components.

在你开始在组件中通过 API 来获取数据之前，你需要知道React的生命周期方法。这些方法是嵌入 React 组件生命周期中的一组挂钩。它们可以在 ES6 类组件中使用，但是不能在无状态组件中使用。

Do you remember when a previous chapter taught you about JavaScript ES6 classes and how they are used in React? Apart from the `render()` method, there are several methods that can be overridden in a React ES6 class component. All of these are the lifecycle methods. Let's dive into them:

你还记得前章中讲过的JavaScript ES6类以及如何在React中使用它们吗？除了 `render()` 方法外，还有几个方法可以在React ES6 类组件中覆写。所有的这些都是生命周期方法。现在让我们来深入了解他们：

You already know two lifecycle methods that can be used in an ES6 class component: `constructor()` and `render()`.

通过之前的学习，你已经知道两种能够用在ES6类组件中的生命周期方法：`constructor()` 和 `render()`。

The constructor is only called when an instance of the component is created and inserted in the DOM. The component gets instantiated. That process is called mounting of the component.

constructor（构造函数）只有在一个组件实例化并插入到DOM中的时候才会被调用。组件被实例化的过程称作组件的挂载（mount）。

The `render()` method is called during the mount process too, but also when the component updates. Each time when the state or the props of a component change, the `render()` method of the component is called.

`render()` 方法也会在组件挂载的过程中被调用，同时当组件更新的时候也会被调用。每当组件的状态（state）或者属性（props）改变时，组件的 `render()` 方法都会被调用。


Now you know more about the two lifecycle methods and when they are called. You already used them as well. But there are more of them.

现在你了解了更多关于这两个生命周期方法的知识，也知道它们什么时候会被调用了。你也已经在前面的学习中使用过它们了。但是 React 里还有更多的生命周期方法。

The mounting of a component has two more lifecycle methods: `componentWillMount()` and `componentDidMount()`. The constructor is called first, `componentWillMount()` gets called before the `render()` method and `componentDidMount()` is called after the `render()` method.

在组件挂载的过程中还有另外两个生命周期方法： 构造函数（constructor）最先执行，`componentWillMount()` 会在`render()` 方法之前执行，而 `componentDidMount()` 在 `render()` 方法之后执行。

Overall the mounting process has 4 lifecycle methods. They are invoked in the following order:

总而言之，在挂载过程中，这四个方法的调用顺序是这样的：

* constructor()
* componentWillMount()
* render()
* componentDidMount()

But what about the update lifecycle of a component that happens when the state or the props change? Overall it has 5 lifecycle methods in the following order:

但是当组件的状态或者属性改变的时候用来更新组件的生命周期是什么样的呢？总的来说，它一共有5个生命周期方法用于组件更新，调用顺序如下：

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

Last but not least there is the unmounting lifecycle. It has only one lifecycle method: `componentWillUnmount()`.

最后但同样重要的，组件卸载也有生命周期。它只有一个生命周期方法：`componentWillUnmount()`。

After all, you don't need to know all of these lifecycle methods from the beginning. It can be intimidating yet you will not use all of them. Even in a larger React application you will only use a few of them apart from the `constructor()` and the `render()` method. Still, it is good to know that each lifecycle method can be used for specific use cases:

但是毕竟你不用一开始就了解所有生命周期方法。这样可能吓到你，而你也不会用到所有的方法。即使在一个很大的 React 应用当中，除了 `constructor()` 和 `render()` 比较常用外，你只会用到一小部分生命周期函数。即使这样，了解每个生命周期方法的适用场景还是对你有帮助的：

* **constructor(props)** - It is called when the component gets initialized. You can set an initial component state and bind class methods during that lifecycle method.

* **constructor(props)** - 它在组件初始化时被调用。在这个方法中，你可以设置初始化状态以及绑定类方法。

* **componentWillMount()** - It is called before the `render()` lifecycle method. That's why it could be used to set internal component state, because it will not trigger a second rendering of the component. Generally it is recommended to use the `constructor()` to set the initial state.

* **componentWillMount()** - 它在 `render()` 方法之前被调用。这就是为什么它可以用作去设置组件内部的状态，因为它不会触发组件的再次渲染。但一般来说，还是推荐在 `constructor()` 中去设置初始化状态。

* **render()** - The lifecycle method is mandatory and returns the elements as an output of the component. The method should be pure and therefore shouldn't modify the component state. It gets an input as props and state and returns an element.

* **render()** - 这个生命周期方法是必须有的，它返回作为组件输出的元素。这个方法应该是一个纯函数，因此不应该在这个方法中修改组件的状态。它把属性和状态作为输入并且返回（需要渲染的）元素

* **componentDidMount()** - It is called only once when the component mounted. That's the perfect time to do an asynchronous request to fetch data from an API. The fetched data would get stored in the internal component state to display it in the `render()` lifecycle method.

* **componentDidMount()** - 它仅在组件挂载后执行一次。这是发起异步请求去 API 获取数据的绝佳时期。获取到的数据将被保存在内部组件的状态中然后在 `render()` 生命周期方法中展示出来。

* **componentWillReceiveProps(nextProps)** - The lifecycle method is called during an update lifecycle. As input you get the next props. You can diff the next props with the previous props, by using `this.props`, to apply a different behavior based on the diff. Additionally, you can set state based on the next props.

* **componentWillReceiveProps(nextProps)** - 这个方法在一个更新生命周期（update lifecycle）中被调用。新的属性会作为它的输入。因此你可以利用 `this.props` 来对比下一个属性和前一个属性，基于对比的结果去实现不同的行为。此外，你可以基于新的属性来设置组件的状态。

* **shouldComponentUpdate(nextProps, nextState)** - It is always called when the component updates due to state or props changes. You will use it in mature React applications for performance optimizations. Depending on a boolean that you return from this lifecycle method, the component and all its children will render or will not render on an update lifecycle. You can prevent the render lifecycle method of a component.

* **shouldComponentUpdate(nextProps, nextState)** - 每次组件因为状态或者属性更改而更新时，它都会被调用。你将在成熟的React应用中使用它来进行性能优化。在一个更新生命周期中，组件及其子组件将根据该方法返回的布尔值来决定是否重新渲染。这样你可以阻止组件的渲染生命周期（render lifecycle）方法，避免不必要的渲染。

* **componentWillUpdate(nextProps, nextState)** - The lifecycle method is immediately invoked before the `render()` method. You already have the next props and next state at your disposal. You can use the method as last opportunity to perform preparations before the render method gets executed. Note that you cannot trigger `setState()` anymore. If you want to compute state based on the next props, you have to use `componentWillReceiveProps()`.

* **componentWillUpdate(nextProps, nextState)** - 这个方法是 render() 执行之前的最后一个方法。你已经拥有下一个属性和状态，它们可以在这个方法中任由你处置。你可以利用这个方法在渲染之前进行最后的准备。注意在这个生命周期方法中你不能再触发 `setState()`。如果你想基于下一个属性计算状态，你必须利用 `componentWillReceiveProps()`。

* **componentDidUpdate(prevProps, prevState)** - The lifecycle method is immediately invoked after the `render()` method. You can use it as opportunity to perform DOM operations or to perform further asynchronous requests.

* **componentDidUpdate(prevProps, prevState)** - 这个方法在 render() 之后立即调用。你可以用它当成操作 DOM 或者执行更多异步请求的机会。

* **componentWillUnmount()** - It is called before you destroy your component. You can use the lifecycle method to perform any clean up tasks.

* **componentWillUnmount()** - 它会在组件销毁之前被调用。你可以利用这个生命周期方法去执行任何清理任务。

The `constructor()` and `render()` lifecycle methods are already used by you. These are the commonly used lifecycle methods for ES6 class components. Actually the `render()` method is required, otherwise you wouldn't return a component instance.

 你已经用过了 `constructor()` 和 `render()` 生命周期方法。对于 ES6 类组件来说他们是常用的生命周期方法。实际上 `render()` 是必须有的，否则它将不会返回一个组件实例。

There is one more lifecycle method: `componentDidCatch(error, info)`. It was introduced in [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) and is used to catch errors in components. For instance, displaying the sample list in your application works just fine. But there could be a case when the list in the local state is set to `null` by accident (e.g. when fetching the list from an external API, but the request failed and you set the local state of the list to null). Afterward, it wouldn't be possible to filter and map the list anymore, because it is `null` and not an empty list. The component would be broken and the whole application would fail. Now, by using `componentDidCatch()`, you can catch the error, store it in your local state, and show an optional message to your application user that an error has happened.

还有另一个生命周期方法：`componentDidCatch(error, info)`。它在 [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) 中引入，用来捕获组件的错误。举例来说，在你的应用中展示样本数据本来是没问题的。但是可能会有列表的本地状态被意外设置成 `null` 的情况发生（例如从外部 API 获取列表失败时，你把本地状态设置为空了）。然后它就不能像之前一样去过滤（filter）和映射（map）这个列表，因为它不是一个空列表（`[]`）而是 `null`。这样这个组件就会崩溃，然后整个应用就会挂掉。现在你可以用 `componentDidCatch()` 来捕获错误，将它存在本地的状态中，然后像用户展示一条信息，说明应用发生了错误。

### Exercises: 练习：

* read more about [lifecycle methods in React](https://facebook.github.io/react/docs/react-component.html)
* read more about [the state related to lifecycle methods in React](https://facebook.github.io/react/docs/state-and-lifecycle.html)
* read more about [error handling in components](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

* 阅读更多 [React 生命周期函数](https://facebook.github.io/react/docs/react-component.html)的内容。
* 阅读更多 [React 中状态与生命周期函数的关系](https://facebook.github.io/react/docs/state-and-lifecycle.html)的内容。
* 阅读更多[组件错误处理](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)的内容。

## Fetching Data 获取数据

Now you are prepared to fetch data from the Hacker News API. There was one lifecycle method mentioned that can be used to fetch data: `componentDidMount()`. You will use the native fetch API in JavaScript to perform the request.

现在你已经做好了从 Hacker News API 获取数据的准备。 我们可以用前文曾提到过的 `componentDidMount()` 生命周期方法来获取数据。你将使用 JavaScript 原生的 fetch API 来发起请求。

Before we can use it, let's set up the URL constants and default parameters to breakup the API request into chunks.

 在开始之前，让我们设置好 URL 常量和默认参数，来将 API 请求分解成几步。

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

在 JavaScript ES6 中，你可以用[模板字符串（template strings）](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)去连接字符串你将用它来拼接最终 API 的访问地址。

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

这样就可以保证以后你 URL 组合的灵活性。

But let's get to the API request where you will use the url. The whole data fetch process will be presented at once, but each step will be explained afterward.

让我们开始使用 API 请求，在这个请求中将用到上述的网址。整个数据获取的过程在下面代码中一次给出，但后面会对每一步做详细解释。

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

这段代码做了很多事。我想把它分成更小的代码段，但是那样又会让人很难去理解每段代码之间的关系。接下来我就来详细解释代码中的每一步。

First, you can remove the sample list of items, because you return a real list from the Hacker News API. The sample data is not used anymore. The initial state of your component has an empty result and default search term now. The same default search term is used in the input field of the Search component and in your first request.

首先，你可以移除样本列表了，因为你将从 Hacker News API 得到一个真实的列表。这些样本数据已经没用了。现在组件将一个空的列表结果以及一个默认的搜索项作为初始状态。这个默认搜索项也同样用在 Search 组件的输入字段和第一个（API）请求中。

Second, you use the `componentDidMount()` lifecycle method to fetch the data after the component did mount. In the very first fetch, the default search term from the local state is used. It will fetch "redux" related stories, because that is the default parameter.

其次，在组件挂载之后，它用了 `componentDidMount()` 生命周期方法去获取数据。在第一次获取数据时，使用的是本地状态中的默认搜索项。它将获取与 “redux” 相关的资讯，因为它是默认的参数。

Third, the native fetch API is used. The JavaScript ES6 template strings allow it to compose the URL with the `searchTerm`. The URL is the argument for the native fetch API function. The response needs to get transformed to a JSON data structure, which is a mandatory step in a native fetch function when dealing with JSON data structures, and can finally be set as result in the internal component state. In addition, the catch block is used in case of an error. If an error happens during the request, the function will run into the catch block instead of the then block. In a later chapter of the book, you will include the error handling.

再次，这里使用的是原生的 fetch API。JavaScript ES6 模板字符串允许组件利用 `searchTerm` 来组成 URL。该 URL 是原生 fetch API 函数的参数。返回的响应需要被转化成 JSON 格式的数据结构。这是在处理 JSON 数据结构时，原生的 fetch API 中的强制步骤。最终处理后的响应可以设置为内部组件状态的结果。此外，我们用一个 catch 块来防止出现错误。如果在发起请求时出现错误，这个函数会进入到 catch 中而不是 then 中。在本书之后的章节中，将涵盖错误处理的内容。

Last but not least, don't forget to bind your new component methods in the constructor.

最后但同样重要的是，不要忘记在构造函数中绑定你的组件方法。

Now you can use the fetched data instead of the sample list of items. However, you have to be careful again. The result is not only a list of data. [It's a complex object with meta information and a list of hits which are in our case the stories](https://hn.algolia.com/api). You can output the internal state with `console.log(this.state);` in your `render()` method to visualize it

现在你可以用获取的数据去代替样本数据了。然而，你必须注意一点，这个结果不仅仅是一个数据的列表。它是一个复杂的对象，[它包含了元数据信息以及一系列的hits，在我们的应用里就是这些资讯](https://hn.algolia.com/api)。你可以在 `render()` 方法中用 `console.log(this.state);` 将这些信息打印出来，以便有一个直观的认识。

In the next step, you will use the result to render it. But we will prevent it from rendering anything, so we will return null, when there is no result in the first place. Once the request to the API succeeded, the result is saved to the state and the App component will re-render with the updated state.

在接下来的步骤中，你将把之前的得到的结果渲染出来。但我们不会什么都渲染，所以在刚开始没有拿到结果时，我们会返回空。一旦 API 请求成功，我们会将结果保存在状态里，然后 App 组件将用更新后的状态重新渲染。

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

让我们回顾一下在组将的整个生命周期中发生了什么。首先组件通过构造函数得到初始化，之后它将初始化的状态渲染出来。但是当结果为空时组件返 null，这阻止了组件的渲染。React允许组件通过返回 null 来不渲染任何东西。接着 `componentDidMount()` 生命周期函数执行。在这个方法中你从 Hacker News API 中异步地拿到了数据。一旦数据到达，组件就通过 `setSearchTopStories()` 函数改变组件内部的状态。之后，因为状态的更新，更新生命周期开始运行。组件再次执行 `render()` 方法，但这次组件的内部状态中的结果已经填充，不再是空了。因此组件将重新渲染 Table 组件的内容。

You used the native fetch API that is supported by most browsers to perform an asynchronous request to an API. The *create-react-app* configuration makes sure that it is supported in every browser. There are third-party node packages that you can use to substitute the native fetch API: [superagent](https://github.com/visionmedia/superagent) and [axios](https://github.com/mzabriskie/axios).

你使用了大多数浏览器支持的原生 fetch API 来执行对 API 的异步请求。*create-react-app* 中的配置保证了它被所有浏览器支持。你也可以使用第三方库来代替原生 fetch API，例如：[superagent](https://github.com/visionmedia/superagent) 和 [axios](https://github.com/mzabriskie/axios).

Back to your application: The list of hits should be visible now. However, there are two regression bugs in the application now. First, the "Dismiss" button is broken. It doesn't know about the complex result object and still operates on the plain list from the local state when dismissing an item. Second, when the list is displayed but you try to search for something else, the list gets filtered on the client-side even though the initial search was made by searching for stories on the server-side. The perfect behvaior would be to fetch another result object from the API when using the Search component. Both regression bugs will be fixed in the following chapters.

让我们重回到你的应用，现在你应该可以看到资讯列表了。然而，现在应用中仍然存在两个bug。第一，“Dismiss” 按钮不工作。因为它还不能处理这个复杂的 result 对象。当我们点击 “Dismiss” 按钮时，它仍然在操作之前那个简单的 result 对象。第二，当这个列表显示出来之后，你再尝试搜索其他的东西时，虽然我们的第一次初始化时的资讯搜索在服务器端进行，但这一次它只会去搜索客户端的样本数据。我们期待的行为是：当我们使用 Search 组件时，从 API 拿到新的结果，而不是去过滤样本数据。不用担心，两个 bug 都将在之后的章节中得到修复。

### Exercises: 练习

* read more about [ES6 template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* read more about [the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* read more about [data fetching in React](https://www.robinwieruch.de/react-fetching-data/)

* 阅读更多 [ES6 模板字符串](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)的内容。
* 阅读更多 [本地提取 API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)的内容。
* 阅读更多 [在 React 中获取数据](https://www.robinwieruch.de/react-fetching-data/)的内容。

## ES6 Spread Operators ES6 扩展操作符

The "Dismiss" button doesn't work because the `onDismiss()` method is not aware of the complex result object. It only knows about a plain list in the local state. But it isn't a plain list anymore. Let's change it to operate on the result object instead of the list itself.

“Dismiss” 按钮之所以不工作，是因为 `onDismiss()` 方法不能处理复杂的 result 对象。它现在还只能处理一个本地状态中的简单列表。但是现在这个列表已经不再是一个平铺的简单列表了。现在，让我们去操作这个 result 对象而不是去操作列表。

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

那现在 `setState()` 中发生了什么呢？很遗憾，这个 result 是一个复杂的对象。资讯（hits）列表只是这个对象中所有属性中的其中之一。所以，当某一项资讯从 result 对象中移除时，只能更新资讯列表，其他的属性还是得保持原样。

One approach could be to mutate the hits in the result object. I will demonstrate it, but we won't do it that way.

解决方法之一是直接改变 result 对象中的 hits 字段。我将演示这个方法，但实际操作中我们不会这样去做。_(或者翻译为：我们不推荐这样做？)_

{title="Code Playground",lang="javascript"}
~~~~~~~~
// don`t do this
this.state.result.hits = updatedHits;
~~~~~~~~

React embraces immutable data structures. Thus you shouldn't mutate an object (or mutate the state directly). A better approach is to generate a new object based on the information you have. Thereby none of the objects get altered. You will keep the immutable data structures. You will always return a new object and never alter an object.

React 拥护不可变的数据结构。因此你不应该改变一个对象（或者直接改变状态）。更好的做法是基于现在拥有的资源来创建一个新的对象。这样就没有任何对象被改变了。这样做的好处是，你总是返回一个新对象，而且你之前所有对象的数据结构将保持不变。

Therefore you can use JavaScript ES6 `Object.assign()`. It takes as first argument a target object. All following arguments are source objects. These objects are merged into the target object. The target object can be an empty object. It embraces immutability, because no source object gets mutated. It would look similar to the following:

因此你可以用 JavaScript ES6 中的 `Object.assign()` 函数来到达这样的目的。它把接收的第一个参数作为目标对象，后面的所有参数作为源对象。然后把所有的源对象合并到目标对象中。只要把目标对象设置成一个空对象，我们就得到了一个新的对象。这种做法是拥抱不变性的，因为没有任何源对象被改变。以下是代码实现：

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Latter objects will override former merged objects when they share the same property names. Now let's do it in the `onDismiss()` method:

当遇到相同的属性时，排在后面的对象会覆写先前对象的该属性。现在让我们用它来改写 `onDismiss()` 方法：

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

这样写已经是一个完整的解决方案了。但是在 JavaScript ES6 以及之后的 JavaScript 版本中还有一个更简单的方法。现在我将向你介绍扩展操作符。它只由三个点组成：`...`。当使用它时，一个数组或者对象的值会被拷贝到另一个数组或对象。

Let's examine the ES6 **array** spread operator even though you don't need it yet.

让我们先来看一下 ES6 中**数组**的扩展运算符，虽然你现在还用不到它。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

The `allUsers` variable is a completely new array. The other variables `userList` and `additionalUser` stay the same. You can even merge two arrays that way into a new array.

这里`allUsers` 是一个全新的数组变量，而变量 `userList` 和 `additionalUser` 还是和原来一样。用这个运算符，你甚至可以合并两个数组到一个新的数组中。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Now let's have a look at the object spread operator. It is not JavaScript ES6. It is a [proposal for a next JavaScript version](https://github.com/sebmarkbage/ecmascript-rest-spread) yet already used by the React community. That's why *create-react-app* incorporated the feature in the configuration.

现在让我们来看看对象的扩展运算符。它并不是 JavaScript ES6 中的用法。它是[针对下一个JavaScript版本的提出的](https://github.com/sebmarkbage/ecmascript-rest-spread)，然而它已经在 React 社区开始使用了。这就是为什么需要在 *create-react-app* 配置中加入了这个功能。

Basically it is the same as the JavaScript ES6 array spread operator but with objects. It copies each key value pair into a new object.

对象的扩展运算符和 JavaScript ES6 中数组的扩展运算符用法基本一样。它可以复制一个对象的所有键值对到一个新的对象中。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Multiple objects can be spread like in the array spread example.

类似于之前数组的例子，以下是扩展多个对象的例子。

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

现在 "Dismiss" 按钮可以再次工作了，因为 `onDismiss()` 方法已经能够处理这个复杂的 result 对象了，并且知道当要忽略掉列表中的某一项时怎么去更新列表了。

### Exercises: 练习

* read more about the [ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* read more about the [ES6 array spread operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * the object spread operator is briefly mentioned

* 阅读更多 [ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)的内容。
* 阅读更多 [ES6 数组的扩展操作符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)的内容。
  * 对象的扩展操作符在其中也有简单提到

## Conditional Rendering 条件渲染

The conditional rendering is introduced pretty early in React applications. But not in the case of the book, because there wasn't such an use case yet. The conditional rendering happens when you want to make a decision to render either one or another element. Sometimes it means to render an element or nothing. After all, a conditional rendering simplest usage can be expressed by an if-else statement in JSX.

React 应用很早就引入了条件渲染。但本书中还没有例子，因为目前为止本书中还没有这样的用例。条件渲染用于当你想要决定渲染一个元素或另一个元素时。有些时候也可以是渲染一个元素或者什么都不渲染。而且 _(感觉这里用毕竟不太好)_，一个条件渲染最简单的用法可用 JSX 语法中的 if-else 来实现。

The `result` object in the internal component state is `null` in the beginning. So far, the App component returned no elements when the `result` hasn't arrived from the API. That's already a conditional rendering, because you return earlier from the `render()` lifecycle method for a certain condition. The App component either renders nothing or its elements.

当 `result` 对象在组件内部的状态为 `null` 并且通过 API 请求的 `result` 还没有到达时，App 组件没有返回任何元素。这其实也是一个条件渲染，因为在 `render()` 生命周期方法中你根据一个特定的条件返回不同的值 _(这个earlier不知如何翻译是好)_。根据条件，App 组件渲染它的元素或者什么都不渲染。

But let's go one step further. It makes more sense to wrap the Table component, which is the only component that depends on the `result`, in an independent conditional rendering. Everything else should be displayed, even though there is no `result` yet. You can simply use a ternary operator in your JSX.

现在，让我们更进一步。因为只有 Table 组件的渲染依赖于 `result`，所以将它包在一个独立的条件渲染中才比较合理。即使 `result` 为空，其它的所有组件还是应该被渲染。仅仅使用三目运算符就能在你的在 JSX 中达到这样的目的。

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

这是实现条件渲染的第二种方式。第三种则是运用 `&&` 逻辑运算符。在JavaScript中， `true && 'Hello World'` 的值永远是 “Hello World”。而 `false && 'Hello World'` 的值则永远是 false。

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

在 React 中你也可以利用这个运算符。如果条件判断为 true，`&&` 操作符后面的表达式的值将会被输出。如果条件判断为 false，React 将会忽略并跳过后面的表达式。这个操作符可以用来实现 Table 组件的条件渲染，因为它返回一个 Table 组件或者什么都不返回。

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

这里有一些运用条件渲染的方法。你可以阅读[更多条件渲染的选择：一个详尽的实例列表](https://www.robinwieruch.de/conditional-rendering-react/)这篇文章了解更多。读完它你将了解他们不同的条件渲染用例以及在什么时候使用它们。

After all, you should be able to see the fetched data in your application. Everything except the Table is displayed when the data fetching is pending. Once the request resolves the result and stores it into the local state, the Table is displayed because the `render()` method runs again and the condition in the conditional rendering resolves in favor of displaying the Table component.

现在，你应该能够在你的应用中看到获取的数据。并且当数据正在获取时，你也可以看到除了 Table 组件以外的所有东西。一旦请求完成并且数据存入本地状态之后，Table 组件也将被渲染出来。因为 `render()` 方法再次执行，而且这时条件渲染判定为渲染 Table 组件。

### Exercises: 练习

* read more about [React conditional rendering](https://facebook.github.io/react/docs/conditional-rendering.html)
* read more about [different ways for conditional renderings](https://www.robinwieruch.de/conditional-rendering-react/)

* 阅读更多 [React条件渲染](https://facebook.github.io/react/docs/conditional-rendering.html)的内容。
* 阅读更多 [实现条件渲染的不同方法](https://www.robinwieruch.de/conditional-rendering-react/)的内容。

## Client- or Server-side Search 客货端或服务端搜索

When you use the Search component with its input field now, you will filter the list. That's happening on the client-side though. Now you are going to use the Hacker News API to search on the server-side. Otherwise you would deal only with the first API response which you got on `componentDidMount()` with the default search term parameter.

目前当你使用 Search 组件的输入栏时，你将在客户端上搜索 _(翻译成过滤总感觉有点奇怪)_ 之前的样本列表。所以你现在要做的是使用 Hacker News API 在服务器端来进行搜索。否则，你将只能处理第一次从 `componentDidMount()` 拿到的默认搜索项的 API 响应。

You can define an `onSearchSubmit()` method in your App component which fetches results from the Hacker News API when executing a search in the Search component. It will be the same fetch as in your `componentDidMount()` lifecycle method, but this time with a modified search term from the local state and not with the initial default search term.

你可以在你的 App 组件中定义一个 `onSearchSubmit()` 方法。当 Search 组件执行搜索时，这个方法用来从 Hacker News API 获取结果。这与 `componentDidMount()` 生命周期方法中的获取数据的方式相同，但是这次搜索的的内容变了，不是用初始化设定的默认搜索术项了。

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

现在 Search 组件需要增加一个按钮了。这个按钮用来触发 API 请求。否则每次当你改变输入框中的值时，你就会向 Hacker News API 发起请求。所以用一个按钮，通过点击它来明确什么时候发出 API 请求很有必要。

As alternative you could debounce (delay) the `onChange()` function and spare the button, but it would add more complexity at this time and maybe wouldn't be the desired effect. Let's keep it simple without a debounce for now.

此外，你也可以在通过在 `onChange()` 函数中设置去抖（延迟）来节省这个按钮，但这会加大难度并且得不偿失。我们现在就用简单的方式来做就好了。

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

第二，在你的 Search 组件中加一个按钮。将这个按钮设置为 `type="submit"`，并且表单（form）用它的 `onSubmit` _(这里作者应该是写错了onSubmit属性应该是没有括号的吧)_ 属性去传递 `onSubmit()` 方法。这里 children 属性将作为按钮的显示内容，当然你也可以另作他用。

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

在 Table 组件中，你可以移除过滤功能了，因为已经不会再客户端进行过滤（搜索）了。同时别忘记移除 `isSearched()` 函数。它也不会使用了。现在，当你点击 “Search” 按钮时，搜索结果将直接从 Hacker News API 中得到。

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

现在当你尝试去搜索时，你将注意到浏览器重新加载了。这是本地浏览器在提交一个 HTML 表单后的行为。在 React 中，你会经常遇到用 `preventDefault()` 事件方法来阻止类似于这样的本地浏览器行为。

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

现在你已经能搜索不同的资讯了。非常棒，这说明你现在能与一个真正 API 打交道，这样也就不再需要在客户端进行搜索了。

### Exercises: 练习

* read more about [synthetic events in React](https://facebook.github.io/react/docs/events.html)
* experiment with the [Hacker News API](https://hn.algolia.com/api)

* 阅读更多 [React 中的合成事件](https://facebook.github.io/react/docs/events.html)的内容。
* 试一试 [Hacker News API](https://hn.algolia.com/api)

## Paginated Fetch 分页抓取

Did you have a closer look at the returned data structure yet? The [Hacker News API](https://hn.algolia.com/api) returns more than a list of hits. Precisely it returns a paginated list. The page property, which is `0` in the first response, can be used to fetch more paginated sublists as result. You only need to pass the next page with the same search term to the API.

你仔细看过返回回来的数据结构吗？[Hacker News API](https://hn.algolia.com/api) 不仅仅只返回资讯（hits）的列表。确切的说它返回的是一个分页列表。利用分页属性（在第一个响应中为“0”），将具有相同搜索字项的下一页传递给 API，你就可以获取更多分页的子列表了。

Let's extend the composable API constants so that it can deal with paginated data.

首先，让我们改造一下可组合的 API 常量，以便处理分页数据。

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

现在你可以使用新常量将分页参数添加到你的 API 请求中。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

The `fetchSearchTopStories()` method will take the page as second argument. If you don't provide the second argument, it will fallback to the `0` page for the initial request. Thus the `componentDidMount()` and `onSearchSubmit()` methods fetch the first page on the first request. Every additional fetch should fetch the next page by providing the second argument.

`fetchSearchTopStories()` 函数接收分页作为第二个参数。如果你不提供第二个参数，它将使用‘0’作为初始参数并发起请求。因此 `componentDidMount()` 和 `onSearchSubmit()` 方法在第一个请求中默认获取首页。之后的请求将根据提供的第二个参数抓取下一个页面的数据。

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

现在你可以使用在 `fetchSearchTopStories()` 函数返回的当前页面数据。你也可以通过 `onClick` 点击事件来使用这个方法，以便抓取更多的资讯。现在让我们来实现通过按钮从 Hacker News API 中获取更多的分页数据的功能。你只需要定义 `onClick()` 事件处理器，这个处理器以当前的搜索关键词和下一页的页码作为参数（当前页码+1）。

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

此外，当还没有返回结果时，在你的 `render()` 方法中你应该保证你的默认分页为0。记住 `render()` 方法是在 `componentDidMount()` 生命周期方法去异步获取数据之前调用的。

There is one step missing. You fetch the next page of data, but it will override your previous page of data. It would be ideal to concatenate the old and new list of hits from the local state and new result object. Let's adjust the functionality to add the new data rather than to override it.

这里还遗漏了一步。你抓取了下一个分页的数据，但新数据会覆盖你之前的分页数据。理想的情况下，result 对象中新的列表和本地状态中老的列表应该合并起来才对。现在让我们来实现将新的数据添加到老的数据上而不是去覆盖它。

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

现在在 `setSearchTopStories()` 方法中做了以下一些操作。首先，你从 result 对象中拿到 hits 字段和 page 字段。

Second, you have to check if there are already old hits. When the page is 0, it is a new search request from `componentDidMount()` or `onSearchSubmit()`. The hits are empty. But when you click the "More" button to fetch paginated data the page isn't 0. It is the next page. The old hits are already stored in your state and thus can be used.

第二，你必须检查老的 hits 字段是否存在。当页码为0时，它应该是一个来自 `componentDidMount()` 或者 `onSearchSubmit()` 方法的新的搜索请求。那么 hits 现在是空的。但是当你通过点击 “More” 按钮去抓取更过的分页数据时，页码不为0。此时它是下一页。老的 hits 已经储存在状态中等待着与新的分页合并。

Third, you don't want to override the old hits. You can merge old and new hits from the recent API request. The merge of both lists can be done with the JavaScript ES6 array spread operator.

第三，你不想覆盖老的 hits。你可以合并老的 hits 及 API 返回的新的 hits。并且这两个列表的合并可以通过 JavaScript ES6 数据扩展操作符完成。

Fourth, you set the merged hits and page in the local component state.

第四，你将合并后的 hits 和页码设置到本地组件的状态中。

You can make one last adjustment. When you try the "More" button it only fetches a few list items. The API URL can be extended to fetch more list items with each request. Again, you can add more composable path constants.

你还可以做一个最后的调整。当你尝试点击 “More” 按钮时，它只抓取一定数量的资讯。但在每个请求的中，你可以通过设置 API URL 来获取更多的资讯。再者，你还可以添加更多的可组合路径常量。

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

现在你可以使用这些常量来扩展 API URL了。

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

现在，我们能够在一个请求中从 Hacker News API 获取更多的数据了。如你所见，功能强大的 Hacker News API 为你提供了大量的方法，以便让你用真实数据做练习。在学习新的东西时，你应该利用真实的 API 来让整个过程更加有趣。这就是当我学习一门新的编程语言或者库时，[如何去利用API提供的便利] (https://www.robinwieruch.de/what-is-an-api-javascript/)。 _(这就是当我学习一门新的编程语言或者库时，如何去利用API提供的便利。->这里感觉翻译得不是很好，请多给意见)_

### Exercises: 练习

* experiment with the [Hacker News API parameters](https://hn.algolia.com/api)

* 练习 [Hacker News API 变量](https://hn.algolia.com/api)

## Client Cache 用户缓存

Each search submit makes a request to the Hacker News API. You might search for "redux", followed by "react" and eventually "redux" again. In total it makes 3 requests. But you searched for "redux" twice and both times it took a whole asynchronous roundtrip to fetch the data. In a client-sided cache you would store each result. When a request to the API is made, it checks if a result is already there. If it is there, the cache is used. Otherwise an API request is made to fetch the data.

每次提交表单都会发起一个对 Hacker News API 的请求。你可能先搜索了 “redux”，然后搜索了 “react”，最后再次搜索了 “redux”。这样它总共发起了3次请求。但是你搜索了 “redux” 两次并且每一次都会执行一次异步操作去获取数据。如果有客户端的缓存，它将保存每次搜索的结果。当发起一个请求时，它首先检查这个请求的结果是否已经在缓存中。如果在，那就使用缓存数据。否则发起一个新的 API 去获取数据。

In order to have a client cache for each result, you have to store multiple `results` rather than one `result` in your internal component state. The results object will be a map with the search term as key and the result as value. Each result from the API will be saved by search term (key).

为了实现在客户端对搜索结果的缓存，你必须在你的内部组件的状态中存储多个`结果`而不是一个`结果`。这些结果对象将会与搜索词映射成一个键值对。而每一个从 API 得到的结果会以搜索的词语为键（key）保存下来。

At the moment, your result in the local state looks similar to the following:

此时，在本地状态中，你的result看起应该是这样：

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Imagine you have made two API requests. One for the search term "redux" and another one for "react". The results object should look like the following:

想想一下你已经发起了两次 API 请求。一次搜索 “redux”，另一次搜索 “react”。那你的 results 对象看起来应该是这样：

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

让我们用 React 的 `setState()` 方法来实现客户端缓存。首先，在初始化组件状态中重命名 `result` 对象为 `results`。第二，定义一个临时的 `searchKey` 用来储存单个 `result`。

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

`searchKey` 必须在发起请求之前设值。它的值来自 `searchTerm`。你可能会想：为什么我们不直接使用 `searchTerm` 呢？这是在我们继续之前需要理解的重点。`searchTerm` 是一个动态的变量，因此它随输入的关键字变化而变化。然而，这里你需要的是一个稳定的变量。它保存最近一次提交给 API 的搜索词，也可以用它来检索结果集中的某个结果。由于它指向缓存中的当前返回结果，因此还可以在 `render()` 方法中用来显示当前结果。

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

现在，你必须去调整一下内部组件状态中储存结果的位置。它应该通过 `searchKey` 来存储每个结果。

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

在 `results` 集中，`searchKey` 用作键名（key），其值用来保存更新后的 hits 和 page。

First, you have to retrieve the `searchKey` from the component state. Remember that the `searchKey` gets set on `componentDidMount()` and `onSearchSubmit()`.

首先，你必须从组件状态中检索 `searchKey`。记住 `searchKey` 是在 `componentDidMount()` 和 `onSearchSubmit()` 中设置的。

Second, the old hits have to get merged with the new hits as before. But this time the old hits get retrieved from the `results` map with the `searchKey` as key.

第二，和之前一样，老的 hits 需要合并到新的 hits 中。但是这次老的 hits 是以 `searchKey` 为键名从 `results` 集中找到的。

Third, a new result can be set in the `results` map in the state. Let's examine the `results` object in `setState()`.

第三，在状态里，一个新的 result 可以设置在 `results` 集中。让我们来看看 `setState()` 中的 `results` 对象是什么样子。

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

The bottom part makes sure to store the updated result by `searchKey` in the results map. The value is an object with a hits and page property. The `searchKey` is the search term. You already learned the `[searchKey]: ...` syntax. It is an ES6 computed property name. It helps you to allocate values dynamically in an object.

下面部分的代码是为了保证，通过 `searchKey` 将更新后的 result 对象保存在 results 集中。它包含 hits 和 page 属性的对象。而 `searchKey` 的值就是搜索词。现在你学会了 `[searchKey]: ...` 这样的语法。这个语法中，ES6 是通过计算得到属性名的。它可以帮助你实现动态分配对象的值。

The upper part needs to spread all other results by `searchKey` in the state by using the object spread operator. Otherwise you would lose all results that you have stored before.

上面部分的代码则是用对象扩展运算符将所有其它包含在 results 集中的 `searchKey` 展开。否则，你将会失去之前所有储存过的 results。

Now you store all results by search term. That's the first step to enable your cache. In the next step, you can retrieve the result depending on the non fluctuant `searchKey` from your map of results. That's why you had to introduce the `searchKey` in the first place as non fluctuant variable. Otherwise the retrieval would be broken when you would use the fluctuant `searchTerm` to retrieve the current result, because this value might change when you would use the Search component.

现在你以搜索词为键名，将所有 results 储存了起来。这是实现缓存的第一步。接下来，你可以根据稳定的 `searchKey` 从 results 集中检索 result。这就是为什么一开始你必须引进 `searchKey` 作为一个稳定的 _(不变的？不易发生变化的？静态的？)_ 变量。否则，当你用动态的 `searchTerm` 去检索当前的 result 时，这个检索过程会崩溃，因为它的值可能在你使用 Search 组件时改变过了。

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

由于没有 `searchKey` 的 result 默认为空列表，所以你现在可以节省 Table 组件的条件渲染了。此外，你需要传 `searchKey` 给 “More” 按钮来代替 `searchTerm`。否则，你抓取分页的搜索词将是 `searchTerm` 这个可能变化的值。另外，确保 “Search” 组件的输入字段用的是动态的 `searchTerm`。

The search functionality should work again. It stores all results from the Hacker News API.

这样，搜索功能就可以再次工作了。它会保存所有从 Hacker News API 返回的结果。

Additionally the `onDismiss()` method needs to get improved. It still deals with the `result` object. Now it has to deal with multiple `results`.

此外，`onDismiss()` 方法也需要优化。它仍然只能处理 `result` 对象。现在它必须处理 `results` 了。

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

这样“Dismiss” 按钮就可以再次工作了。

However, nothing stops the application from sending an API request on each search submit. Even though there might be already a result, there is no check that prevents the request. Thus the cache functionality is not complete yet. It caches the results, but it doesn't make use of them. The last step would be to prevent the API request when a result is available in the cache.

然后，现在还没有任何东西阻止应用每次都对一个搜索发起一次 API 请求。即使我们保存了某一个结果，但还差一个用于阻止重复请求它的检查。因此缓存功能仍需完善。虽然应用缓存了所有 result，但它还没有将这些 result 利用起来。所有我们的最后一步就是：对缓存中已经存在的 result 进行搜索时，阻止 API 请求。

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

现在就算你重复搜索一个词，你的客户端也只会发起一次请求。以这种方式进行缓存的话分页的数据也会保存下来，因为结果的每一页都将保存在 `results` 集中。这是不是一个很强大的方法来引入缓存呢？而且 Hacker News API 为你提供了一切，哪怕是分页数据的高效缓存。_(而且 Hacker News API 为你提供了一切，哪怕是有效地缓存分页数据。)_

## Error Handling 错误处理

Everything is in place for your interactions with the Hacker News API. You even have introduced an elegant way to cache your results from the API and make use of its paginated list functionality to fetch an endless list of sublists of stories from the API. But there is one piece missing. Unfortunately it is often missed when developing applications nowadays: error handling. It is too easy to implement the happy path without worrying about the errors that can happen along the way.

你用来与 Hacker News API 交互的所有功能都已就位。你甚至引入了一种优雅的方式来进行 API 缓存，并能够通过分页列表功能从 API 中获取无尽的资讯子列表。但是我们还忽略一项。并且它在日常开发中经常被遗忘，它就是：错误处理。人们常常容易沉浸于主逻辑的开发中，却忘记去添加错误处理。

In this chapter, you will introduce an efficient solution to add error handling for your application in case of an erroneous API request. You have already learned about the necessary building blocks in React to introduce error handling: local state and conditional rendering. Basically, the error is only another state in React. When an error occurs, you will store it in the local state and display it with a conditional rendering in your component. That's it. Let's implement it in the App component, because it's the component that is used to fetch the data from the Hacker News API in the first place. First, you have to introduce the error in the local state. It is initialized as null, but will be set to the error object in case of an error.

在本章中，引入了一个高效的方法来为你的应用添加一个错误处理，以防发生错误的 API 请求 _(API请求发生错误？)_。其实你已经掌握了在 React 中构建错误处理的知识，那就是：本地状态和条件渲染。追本溯源地讲，错误只是 React 的另一种状态。当一个错误发生时，你先将它存在本地状态中，而后利用条件渲染在组件中显示错误信息。听起来是不是很简单。现在让我们在 App 组件中实现它，因为它是向 Hacker News API 发起请求的组件。首先，你要在本地状态中引入 error 这个状态。它初始化为 null，但当错误发生时它会被置成一个 error 对象。

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

第二，通过结合使用catch 块和 `setState()`，你可以捕获错误对象并将它存在本地状态中。如果 API 请求失败，catch 块就会执行。

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

第三，如果错误发生了，你可以在 `render()` 方法中对 error 对象进行本地检索，然后利用条件渲染来显示一个错误信息。

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

就这么简单。如果你想测试错误处理是否工作，你可以把 API URL 换成一个不存在的 URL。

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

Afterward, you should get the error message instead of your application. It is up to you where you want to place the conditional rendering for the error message. In this case, the whole app isn't displayed anymore. That wouldn't be the best user experience. So what about displaying either the Table component or the error message? The remaining application would still be visible in case of an error.

之后，你应该得到一个错误信息而不是应用的渲染。你可以自己决定放置错误信息渲染的位置。在上述代码下，整个 app 不再显示。显然这不是最佳用户体验。我们将 Table 组件和错误信息择一而渲染之如何？这样的话当错误发生时，除了 Table 组件，应用其余的部分仍然可见。

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

你的应用应该仍然可以工作，不过此时如果 API 请求失败应用就已经有错误处理了。

### Exercises: 练习

* read more about [React's Error Handling for Components](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

* 阅读更多 [React 组件的错误处理](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)的内容。

{pagebreak} 分页符

You have learned to interact with an API in React! Let's recap the last chapters:

你已经学会使用 React 去与 API 交互了！现在让我们回顾一下本章内容：

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

再次强调，此时小憩一下，将学过的知识点消化理解并应用是有必要的。你还可以试验一下迄今为止编写的源代码。

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.3).

你可以在官方代码库中找到[源码](https://github.com/rwieruch/hackernews-client/tree/4.3).
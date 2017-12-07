# State Management in React and beyond 

# React 状态管理与进阶

You have already learned the basics of state management in React in the previous chapters. This chapter digs a bit deeper into the topic. You will learn best practices, how to apply them and why you could consider using a third-party state management library.

在前面的章节中，你已经学会了 React 基本的状态管理，本章将会更加深入地进行学习。你将学习到状态管理的最佳实践，如何去应用它们，以及为什么可以考虑使用第三方的状态管理库。

## Lifting State 

## 状态提取???

Only the App component is a stateful ES6 component in your application. It handles a lot of application state and logic in its class methods. Maybe you have noticed that you pass a lot of properties to your Table component. Most of these props are only used in the Table component. In conclusion one could argue that it makes no sense that the App component knows about them.

在你的应用程序中，只有 App 是具有状态的 ES6 组件。在该组件的方法中，包含了许多应用程序的状态和业务逻辑。可能你已经注意到了，Table 组件被传入了大量的 props 参数。而这些参数中的绝大部分只在 Table 组件中被用到。所以有人可能会提出 App 组件拥有这些 props 是毫无意义的。

The whole sort functionality is only used in the Table component. You could move it into the Table component, because the App component doesn't need to know about it at all. The process of refactoring substate from one component to another is known as *lifting state*. In your case, you want to move state that isn't used in the App component into the Table component. The state moves down from parent to child component.

In order to deal with state and class methods in the Table component, it has to become an ES6 class component. The refactoring from functional stateless component to ES6 class component is straight forward.

为了能够在 Table 组件中管理状态和添加方法，需要将其改写成 ES6 class 形式。从无状态的函数组件（functional stateless component）到 ES6 class 组件的重构非常简单明了。

Your Table component as a functional stateless component:

无状态函数组件形式的 Table 组件：

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
    ...
  );
}
~~~~~~~~

Your Table component as an ES6 class component:

ES6 class 形式的 Table 组件：

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
class Table extends Component {
  render() {
    const {
      list,
      sortKey,
      isSortReverse,
      onSort,
      onDismiss
    } = this.props;

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return(
      ...
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Since you want to deal with state and methods in your component, you have to add a constructor and initial state.

由于想要在 Table 组件中管理状态，需要添加构造函数和初始状态。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
# leanpub-start-insert
  constructor(props) {
    super(props);

    this.state = {};
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Now you can move state and class methods regarding the sort functionality from your App component down to your Table component.

现在你可以将状态和有关排序的方法从 App 组件向下移动到 Table 组件中。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      sortKey: 'NONE',
      isSortReverse: false,
    };

    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Don't forget to remove the moved state and `onSort()` class method from your App component.

别忘了将移动后的 state 和  `onSort()` 实例方法从 App 组件中移除。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
      isLoading: false,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
  }

  ...

}
~~~~~~~~

Additionally, you can make the Table component API more lightweight. Remove the props that are passed to it from the App component, because they are handled internally in the Table component now.

除此之外，你还可以让 Table 组件更加轻量。可以将从 App 组件中传入的 props 移除，因为现在这些 props 可以在 Table 组件内部由 state 进行处理。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading
    } = this.state;
# leanpub-end-insert

    ...

    return (
      <div className="page">
        ...
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

Now in your Table component you can use the internal `onSort()` method and the internal Table state.

现在你就可以使用 Table 组件内的 `onSort()` 方法和其内部的 state 了。

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      list,
      onDismiss
    } = this.props;

    const {
      sortKey,
      isSortReverse,
    } = this.state;
# leanpub-end-insert

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return(
      <div className="table">
        <div className="table-header">
          <span style={{ width: '40%' }}>
            <Sort
              sortKey={'TITLE'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Title
            </Sort>
          </span>
          <span style={{ width: '30%' }}>
            <Sort
              sortKey={'AUTHOR'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Author
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'COMMENTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Comments
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'POINTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Points
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            Archive
          </span>
        </div>
        { reverseSortedList.map((item) =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Your application should still work. But you made a crucial refactoring. You moved functionality and state closer into another component. Other components got more lightweight again. Additionally the component API of the Table got more lightweight because it deals internally with the sort functionality.

应用程序应该还是像之前一样正常运行，但是你已经做了非常重要的重构工作。相关的逻辑代码和状态信息从 App 组件移动到了 Table 组件中，这使得 App 组件更加轻量。此外因为 Table 的排序逻辑放在了组件内部，所以它接口也更加轻量了。

The process of lifting state can go the other way as well: from **child to parent component**. It is called as lifting state up. Imagine you were dealing with internal state in a child component. Now you want to fulfill a requirement to show the state in your parent component as well. You would have to lift up the state to your parent component. But it goes even further. Imagine you want to show the state in a sibling component of your child component. Again you would have to lift the state up to your parent component. The parent component deals with the internal state, but exposes it to both child components.

状态提取的过程也可以反过来：从子组件到父组件，这种情形被称为状态提升。想象一下，你在子组件中处理了内部的状态信息。现在为了满足新的需求，在其父组件中也显示该组件的状态信息，你需要将状态提升到父组件中。但是情况还不止这些，想象一下如果你想在子组件的兄弟组件上显示状态相关信息，你还是需要将状态提升到父组件中。在父组件中处理内部状态信息，同时将状态信息暴露给相关的子组件。



### 练习：

* 了解更多关于 [React 的状态提升](https://facebook.github.io/react/docs/lifting-state-up.html)
* 在[使用 Redux 之前学习 React](https://www.robinwieruch.de/learn-react-before-using-redux/)这篇文章中了解更多关于状态提升

## Revisited: setState()

## 再探：setState() 

So far, you have used React `setState()` to manage your internal component state. You can pass an object to the function where you can update partially the internal state.

至此，你已经使用过 React 的  `setState()` 方法来管理组件的内部状态信息。你可以给该函数传入一个对象来改变部分的内部状态信息。

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ foo: bar });
~~~~~~~~

But `setState()` doesn't take only an object. In its second version, you can pass a function to update the state.

但是 `setState()` 方法不仅可以接受对象。在它的第二种形式中，你可以传入一个函数来更新状态信息。

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

Why should you want to do that? There is one crucial use case where it makes sense to use a function over an object. It is when you update the state depending on the previous state or props. If you don't use a function, the internal state management can cause bugs.

为什么你会需要第二种形式呢？使用函数作为参数为不是对象，有一个非常重要的应用场景，就是当更新状态的时候需要取决于之前的状态和 props。如果不使用函数参数的形式，组件的内部状态管理可能会引起 bug。

But why does it cause bugs to use an object over a function when the update depends on the previous state or props? The React `setState()` method is asynchronous. React batches `setState()` calls and executes them eventually. It can happen that the previous state or props changed in between when you would rely on it in your `setState()` call.

当更新状态需要取决于之前的状态和 props 时，为什么使用对象而不是函数作为参数会引起 bug 呢？React 的 `setState()` 方法是异步的。React 批次执行 `setState()` 方法，最终会全部执行完毕。如果你的 `setState()` 方法依赖于之前的状态和属性的话，有可能在批次执行期间，状态和属性的值已经被改变了。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
~~~~~~~~

Imagine that `fooCount` and `barCount`, thus the state or the props, change somewhere else asynchronously when you call `setState()`. In a growing application, you have more than one 'setState()' call across your application. Since `setState()` executes asynchronously, you could rely in the example on stale values.

想象一下像 `fooCount` 和 `barCount` 这样的状态或属性值，在你调用  `setState()` 方法的时候在其他地方被异步地改变了。在不断膨胀的应用程序中，你会有多个  `setState()` 调用。因为 `setState()` 是异步执行的，你可能像上面的例子一样，依赖的值已经被改变了。

With the function approach, the function in `setState()` **is a callback that operates on the state and props at the time of executing the callback function**. Even though `setState()` is asynchronous, with a function it takes the state and props at the time when it is executed.

使用函数参数形式的话，传入 `setState()` 方法的参数是一个回调，该回调会在被执行时传入状态和属性值。尽管 `setState()` 方法是异步的，但是回调会传入调用时的状态和属性值。

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
~~~~~~~~

Now, lets get back to your code to fix this behavior. Together we will fix it for one place where `setState()` is used and relies on the state or props. Afterward, you are able to fix it at other places too.

现在让我们回到代码中来修复这个问题。我们会一起修复一个 `setState()` 依赖于状态和属性的地方，之后你就可以按照同样的方式修复代码中的其他地方。

The `setSearchTopStories()` method relies on the previous state and thus is a perfect example to use a function over an object in `setState()`. Right now, it looks like the following code snippet.

 `setSearchTopStories()` 方法依赖于之前的状态，因此它是个使用函数而不是对象作为 `setState()` 参数的绝佳例子。目前的代码片段如下。

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  const { searchKey, results } = this.state;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  });
}
~~~~~~~~

You extract values from the state, but update the state depending on the previous state asynchronously. Now you can use the functional approach to prevent bugs because of a stale state.

你从 state 中提取了一些值，但是异步的状态更新依赖于之前的状态信息。现在你可以使用函数参数的形式来防止因为脏状态信息造成的 bug。

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

You can move the whole block that you have already implemented into the function. You only have to exchange that you operate on the `prevState` rather than `this.state`.

你可以将已经实现的逻辑移动到函数内部。只需将在 `this.state` 上的操作改为 `prevState` 。

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

  this.setState(prevState => {
# leanpub-start-insert
    const { searchKey, results } = prevState;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    return {
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    };
# leanpub-end-insert
  });
}
~~~~~~~~

That will fix the issue with a stale state. There is one more improvement. Since it is a function, you can extract the function for an improved readability. That's one more advantage to use a function over an object. The function can live outside of the component. But you have to use a higher order function to pass the result to it. After all, you want to update the state based on the fetched result from the API.

如此可以修复脏状态导致的问题。还有一个可以改进的地方。因为它是一个函数，你可以将该函数提取出来从而改善代码的可读性。这是使用函数参数形式相比于对象形式的另一个好处，该函数可以独立于组件。但是你需要使用一个高阶函数并将 result 传给它。毕竟，你想根据 API 的获取结果来更新状态。

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

The `updateSearchTopStoriesState()` function has to return a function. It is a higher order function. You can define this higher order function outside of your App component. Note how the function signature changes slightly now.

`updateSearchTopStoriesState()` 是一个高阶函数，它需要返回一个函数。你可以在 App 组件之外定义这个高阶函数。请注意现在函数的签名有了一些变化。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const updateSearchTopStoriesState = (hits, page) => (prevState) => {
  const { searchKey, results } = prevState;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  return {
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  };
};
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

That's it. The function over an object approach in `setState()` fixes potential bugs yet increases readability and maintainability of your code. Furthermore, it becomes testable outside of the App component. You could export it and write a test for it as exercise.

搞定！`setState()` 中函数参数形式相比于对象参数来说，能够阻止潜在的 bug 同时能够提高代码的可读性和可维护性。此外，它可以在 App 组件之外进行测试。你可以将其导出并写个测试来当作练习。

### 练习：

* 了解更多关于 [在 React 中正确使用 state](https://facebook.github.io/react/docs/state-and-lifecycle.html#using-state-correctly)
* 将所有使用 `setState()` 方法的地方重构为函数参数形式
  - 只重构那些需要的地方，即依赖于之前的 props 和 state
* 重新跑一遍测试，确保一些正常工作

## Taming the State 

## 驾驭 State

The previous chapters have shown you that state management can be a crucial topic in larger applications. In general, not only React but a lot of SPA frameworks struggle with it. Applications got more complex in the recent years. One big challenge in web applications nowadays is to tame and control the state.

前面的章节已经说明，状态管理在大型的应用程序中是个非常关键主题。通常来说，不仅是 React 还有很多其他的单页面应用框架面临这样的问题。近些年来应用程序变得越来越复杂。现在 web 应用程序面临的一个重大挑战是如何驾驭和控制状态。

Compared to other solutions, React already made a big step forward. The unidirectional data flow and a simple API to manage state in a component are indispensable. These concepts make it easier to reason about your state and your state changes. It makes it easier to reason about it on a component level and to a certain degree on a application level.

与其他的解决方案相比，React 已经向前迈进了一大步。单向数据流和简单的组件状态管理 API 非常必要 。这些概念使得推断状态和其改变更加容易，在组件级别以及一定程度上的应用级别的状态推断也更加容易。

In a growing application, it gets harder to reason about state changes. You can introduce bugs by operating on stale state when using an object over a function in `setState()`. You have to lift state around to share necessary or hide unnecessary state across components. It can happen that a component needs to lift up state, because its sibling component depends on it. Perhaps the component is far away in the component tree and thus you have to share the state across the whole component tree. In conclusion components get involved to a greater extent in state management. But after all, the main responsibility of components should be representing the UI, shouldn't it?

在不断膨胀的应用程序中，推断状态的变化变得更加困难。`setState()` 方法使用对象形式而不是函数形式的话，如果在脏状态上进行操作，则可能会引入 bug。为了能够共享状态或者在兄弟组件之间隐藏不必要的状态，你需要将状态进行提升或者降低。有可能组件需要将其状态进行提升，因为它的兄弟组件依赖于这些状态。有可能用到其状态的兄弟组件在组件树中相隔甚远，所以你可能需要在其整个组件树中共享该状态。这样做的结果会使得在状态管理中涉及的组件范围很广。但是毕竟组件的主要职责就是描绘 UI，不是吗？

Because of all these reasons, there exist standalone solutions to take care of the state management. These solutions are not only used in React. However, that's what makes the React ecosystem such a powerful place. You can use different solutions to solve your problems. To address the problem of scaling state management, you might have heard of the libraries [Redux](http://redux.js.org/docs/introduction/) or [MobX](https://mobx.js.org/). You can use either of these solutions in a React application. They come with extensions, [react-redux](https://github.com/reactjs/react-redux) and [mobx-react](https://github.com/mobxjs/mobx-react), to integrate them into the React view layer.

因为以上的这些原因，存在着一些独立的解决方案来解决状态管理问题。这些方案不仅仅可以在 React 中使用，但是却使得 React 的生态更加繁荣。你可以使用不同的解决方案来解决你的问题。为了解决状态管理越来越复杂的问题，你可能听说过 [Redux](http://redux.js.org/docs/introduction/) 或者 [MobX](https://mobx.js.org/)。你可以在 React 应用中使用这些第三方库中的任何一个。它们还有一些扩展，如 [react-redux](https://github.com/reactjs/react-redux) 和 [mobx-react](https://github.com/mobxjs/mobx-react) 来将其连接到 React 的视图层。

Redux and MobX are outside of the scope of this book. When you have finished the book, you will get guidance on how you can continue to learn React and its ecosystem. One learning path could be to learn Redux. Before you dive into the topic of external state management, I can recommend to read this [article](https://www.robinwieruch.de/redux-mobx-confusion/). It aims to give you a better understanding of how to learn external state management.

Redux 和 MobX 超出了本书的讨论范围。当读读完本书的时候，你将获得关于如何继续学习 React 及其生态系统的指导。其中一个学习路线是 Redux。在你深入外部状态管理主题之前，我推荐你阅读这篇 [文章](https://www.robinwieruch.de/redux-mobx-confusion/)。它旨在给你一个如何学习外部状态管理更好的理解。

### 练习：

* 阅读更多关于 [外部状态管理以及如何学习](https://www.robinwieruch.de/redux-mobx-confusion/)
* 看看我的第二本电子书关于 [React 中的状态管理](https://roadtoreact.com/)

{pagebreak}

You have learned advanced state management in React! Let's recap the last chapters:

你已经学习了 React 的高级状态管理！让我们回归一下最后几章的内容。

* React
  * 将状态提升或者下降到合适的组件中
  * setState 方法可以使用函数参数形式来阻止脏状态的 bug
  * 存在外部的解决方案帮助你掌握驾驭 state

你可以从 [官方代码仓库](https://github.com/rwieruch/hackernews-client/tree/4.6) 中找到源代码。
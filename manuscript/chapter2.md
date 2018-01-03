# React 基础

The chapter will guide you through the basics of React. It covers state and interactions in components, because static components are a bit dull, aren't they? Additionally, you will learn about the different ways to declare a component and how to keep components composable and reusable. Be prepared to breathe life into your components.

本章将指导你了解 React 的基础知识。因为静态的组件会有些枯燥，所以内容会包含组件的状态与交互。此外，你将学习用不同方式声明组件以及如何保持组件的可重用, 可组合性。准备好创造你自己的组件。

## Internal Component State

## 组件内部状态

Internal component state, also known as local state, allows you to save, modify and delete properties that are stored in your component. The ES6 class component can use a constructor to initialize internal component state later on. The constructor is called only once when the component initializes.

组件内部状态也被称为局部状态，允许你保存，修改和删除存储在组件内部的属性。使用 ES6 类组件可以在构造函数中初始化组件的状态。 构造函数只会在组件初始化时调用一次。

Let's introduce a class constructor.

让我们引入类构造函数

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

When having a constructor in your ES6 class component, it is mandatory to call `super();` because the App component is a subclass of `Component`. Hence the `extends Component` in your App component declaration. You will learn more about ES6 class components later on.

当你使用 ES6 编写的组件有一个构造函数时，它需要强制地调用 `super();` 方法，因为这个 App 组件是 `Component` 的子类。因此在你的APP组件要声明 `extends Component` 。 你会在后续内容中更详细了解使用 ES6 编写的组件。

You can call `super(props);` as well. It sets `this.props` in your constructor in case you want to access them in the constructor. Otherwise, when accessing `this.props` in your constructor, they would be `undefined`. You will learn more about the props of a React component later on.

你也可以调用 `super(props);`，它会在你的构造函数中设置  `this.props` 以供在构造函数中访问它们。 否则当在构造函数中访问  `this.props` ，会得到 `undefined`。稍后您将了解更多关于 React 组件的 props。

Now, in your case, the initial state in your component should be the sample list of items.

现在，在你的示例中，组件中的初始状态应该是一个列表

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

The state is bound to the class by using the `this` object. Thus you can access the local state in your whole component. For instance, it can be used in the `render()` method. Previously you have mapped a static list of items in your `render()` method that was defined outside of your component. Now you are about to use the list from your local state in your component.

state 通过使用 `this` 绑定在类上。因此，你可以在整个组件中访问到 state。例如它可以用在 `render()` 方法中。此前你已经在 `render()`  方法中映射一个在组件外定义静态列表。现在你可以在组件中使用 state 里的 list了。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

The list is part of the component now. It resides in the internal component state. You could add items, change items or remove items in and from your list. Every time you change your component state, the `render()` method of your component will run again. That's how you can simply change your internal component state and be sure that the component re-renders and displays the correct data that comes from the local state.

现在 list 是组件的一部分。它驻留在组件的 state 中。你可以从 list 中添加、修改或者删除列表项。每次你修改组件的内部状态，组件的 `render` 方法会再次运行。这样你可以简单地修改组件内部状态，确保组件重新渲染并且展示从内部状态获取到的正确数据。

But be careful. Don't mutate the state directly. You have to use a method called `setState()` to modify your state. You will get to know it in a following chapter.

但是需要注意，不要直接修改 state。你必须使用 `setState()` 方法来修改它。你将在接下来的章节了解到它。

### Exercises: 练习：

* experiment with the local state
  * define more initial state in the constructor
  * use and access the state in your `render()` method
* read more about [the ES6 class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

- 练习使用 state
  - 在构造函数中定义更多的初始化 state
  - 在 `render()`  函数中访问使用 state
- 阅读更多关于 [ES6类构造函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## ES6 对象初始化

In JavaScript ES6, you can use a shorthand property syntax to initialize your objects more concisely. Imagine the following object initialization:

在 ES6 中，你可以通过简写属性更加简洁地初始化对象。想象下面的对象初始化：

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

When the property name in your object is the same as your variable name, you can do the following:

当你的对象中的属性名与变量名相同时，您可以执行以下的操作：

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

In your application, you can do the same. The list variable name and the state property name share the same name.

在应用程序中，你也可以这样做。列表变量名和状态属性名称共享同一名称。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

Another neat helper are shorthand method names. In JavaScript ES6, you can initialize methods in an object more concisely.

另一个整洁的辅助办法是简写方法名。在 ES6 中，你能更简洁地初始化一个对象的方法。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

Last but not least, you are allowed to use computed property names in JavaScript ES6.

最后值得一提的是你可以在 ES6 中使用计算属性名

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

Perhaps computed property names make no sense for you yet. Why should you need them? In a later chapter, you will come to a point where you can use them to allocate values by key in a dynamic way in an object. It's neat to generate lookup tables in JavaScript.

或许你目前还觉得计算属性名没有意义。为什么需要他们呢？在后续的章节中，当你为一个对象动态地根据 key 分配值时便会涉及到。在 JavaScript 中生成查找表是很简单的。

### Exercises: 练习：

* experiment with ES6 object initializer
* read more about [ES6 object initializer](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

- ES6 对象初始化练习
- 阅读更多关于  [ES6 对象初始化](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Unidirectional Data Flow 

## 单向数据流

Now you have some internal state in your App component. However, you have not manipulated the local state yet. The state is static and thus is the component. A good way to experience state manipulation is to have some component interaction.

现在你的组件中有一些内部的 state。但是你还没有操纵它们，因此 state 是静态的。一个练习 state 操作好方法是增加一些组件的交互。

Let's add a button for each item in the displayed list. The button says "Dismiss" and is going to remove the item from the list. It could be useful eventually when you only want to keep a list of unread items and dismiss the items that you are not interested in.

让我们为列表中的每一项增加一个按钮。按钮的文案为 “删除” ，意味着将从列表中删除该项。这个按钮在你希望保留未读列表和删除不感兴趣的项时会非常有用。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

The `onDismiss()` class method is not defined yet. We will do it in a moment, but for now the focus should be on the `onClick` handler of the button element. As you can see, the `onDismiss()` method in the `onClick` handler is enclosed by another function. It is an arrow function. That way, you can sneak in the `objectID` property of the `item` object to identify the item that will be dismissed. An alternative way would be to define the function outside of the `onClick` handler and only pass the defined function to the handler. A later chapter will explain the topic of handlers in elements in more detail.

这个类方法  `onDismiss()`  还没有被定义， 我们稍后再来做这件事。目前先把重点放在按钮元素的 ` onClick ` 事件句柄上。正如你看见的，  `onDismiss()`  方法被另外一个函数包裹在 ` onClick ` 事件句柄中，它是一个箭头函数。这样你可以拿到 `item` 对象中的 `objectID` 属性来确定那一项会被删除掉。另外一种方法是在 ` onClick ` 句柄之外定义函数，并只将已定义的函数传到句柄。在后续的章节中会解释更多细节关于元素句柄。

Did you notice the multilines for the button element? Note that elements with multiple attributes get messy as one line at some point. That's why the button element is used with multilines and indentations to keep it readable. But it is not mandatory. It is only a code style recommendation that I highly recommend.

你有没有注意到按钮元素是多行代码的？元素中一行有多个属性会看起来比较混乱。所以这个按钮使用多行格式来书写以保持它的可读性。这虽然不是强制的，但这是我的极力推荐的代码风格。

Now you have to implement the `onDismiss()` functionality. It takes an id to identify the item to dismiss. The function is bound to the class and thus becomes a class method. That's why you access it with `this.onDismiss()` and not `onDismiss()`. The `this` object is your class instance. In order to define the `onDismiss()` as class method, you have to bind it in the constructor. Bindings will be explained in another chapter later on.

现在你需要来完成 `onDismiss()` 的功能，它通过id来标示那一项需被删除。此函数绑定到类，因此成为类方法。这就是为什么你访问它使用 `this.onDismiss()` 而不是 `onDismiss()`。 `this` 对象是类的实例，为了将 `onDismiss()` 定义为类方法，你需要在构造函数中绑定它。绑定稍后将在另一章中详细解释。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~

In the next step, you have to define its functionality, the business logic, in your class. Class methods can be defined the following way.

下一步，你需要在类中定义它的功能和业务逻辑。类方法可以用以下方式定义。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Now you are able to define what happens inside of the class method. Basically you want to remove the item identified by the id from the list and store an updated list to your local state. Afterward, the updated list will be used in the re-running `render()` method to display it. The removed item shouldn't appear anymore.

现在你可以定义方法内部的功能。总的来说你希望从列表中删除由id标识的项，并且保存更新后的列表到 state 中。随后这个更新后列表被使用到再次运行的 `render()` 方法中并渲染，最后这个被删除项就不再显示了。

You can remove an item from a list by using the JavaScript built-in filter functionality. The filter function takes a function as input. The function has access to each value in the list, because it iterates over the list. That way, you can evaluate each item in the list based on a filter condition. If the evaluation for an item is true, the item stays in the list. Otherwise it will be filtered from the list. Additionally, it is good to know that the function returns a new list and doesn't mutate the old list. It supports the convention in React of having immutable data structures.

你可以通过 JavaScript 内置的 filter 方法来删除列表中的一项。fitler 方法以一个函数作为输入。这个函数可以访问列表中的每一项，因为它会遍历整个列表。通过这种方式，你可以基于过滤条件来判断列表的每一项。如果该项判断结果为true，则该项保留在列表中。否则将从列表中过滤掉。另外，好的一点是这个方法会返回一个新的列表也不是改变旧列表。它遵循了 React 中不可变数据的约定。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

In the next step, you can extract the function and pass it to the filter function.

在下一步中，您可以抽取函数并将其传递给 filter 函数。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

In addition, you can do it more concisely by using a JavaScript ES6 arrow function again.

另外，可以通过使用 ES6 的箭头函数让代码更简洁。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

You could even inline it again, like you did in the `onClick` handler of the button, but it might get less readable.

你甚至可以内联到一行内完成，就像在按钮的 `onClick` 句柄做的一样，但如此会损失一些可读性

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

The list removes the clicked item now. However the state isn't updated yet. Therefore you can finally use the `setState()` class method to update the list in the internal component state.

现在已经从列表中删除了点击项，但是 state 还并没有更新。因此你需要最后使用类方法 `setState()` 来更新组件 satate 中的列表了。

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Now run again your application and try the "Dismiss" button. It should work. What you experience now is the **unidirectional data flow** in React. You trigger an action in your view with `onClick()`, a function or class method modifies the internal component state and the `render()` method of the component runs again to update the view.

现在重新运行你的程序并尝试点击“删除”按钮，它应该是工作的。你现在所练习的就是 React 中的**单向数据流**。你在界面通过 `onClick` 触发一个动作，再通过函数或类方法修改组件的 state，最后组件的 `render()` 方法再次运行并更新界面。 	 

![Internal state update with unidirectional data flow](images/set-state-to-render-unidirectional.png)

### Exercises:

* read more about [the state and lifecycle in React](https://facebook.github.io/react/docs/state-and-lifecycle.html)

### 练习:

- 阅读更多关于 [React的状态与生命周期](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Bindings

## 绑定

It is important to learn about bindings in JavaScript classes when using React ES6 class components. In the previous chapter, you have bound your class method `onDismiss()` in the constructor.

当使用 ES6 编写的React组件时，了解在 JavaScript 类的绑定会非常重要。在前面章节，你已经在构造函数中绑定了  `onDismiss()` 方法

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Why would you do that in the first place? The binding step is necessary, because class methods don't automatically bind `this` to the class instance. Let's demonstrate it with the help of the following ES6 class component.

为什么一开始就需要这么做呢？绑定的步骤是非常重要的，因为类方法不会自动绑定 `this` 到实例上。让我们通过下面的代码来做验证。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

The component renders just fine, but when you click the button, you will get `undefined` in your developer console log. That's a main source of bugs when using React, because if you want to access `this.state` in your class method, it cannot be retrieved because `this` is `undefined`. So in order to make `this` accessible in your class methods, you have to bind the class methods to `this`.

组件正确的渲染，但是当你点击按钮时候，你会在开发调试控制台中得到 `undefined` 。这是使用 React 主要的 bug 来源，因为当你想在类方法中访问 `this.state` 时，它不能被检索到是因为 `this` 是 `undefined` 的。所以为了确保 `this` 在类方法中是可访问的，你需要将 `this` 绑定到类方法上。

In the following class component the class method is properly bound in the class constructor.

在下面的组件中，类方法在构造函数中正确绑定。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

When trying the button again, the `this` object, to be more specific the class instance, should be defined and you would be able to access `this.state`, or as you will later learn `this.props`, now.

再次尝试点击按钮，这个 `this` 对象就指向了类的实例。你现在就可以访问到  `this.state` 或者是后面会学习到的 `this.props`。

The class method binding can happen somewhere else too. For instance, it can happen in the `render()` class method.

类方法的绑定也可以写起其他地方，比如写在 `render()` 函数中。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

But you should avoid it, because it would bind the class method every time when the `render()` method runs. Basically it runs every time your component updates which leads to performance implications. When binding the class method in the constructor, you bind it only once in the beginning when the component is instantiated. That's a better approach to do it.

但是你应该避免这样做，因为它会在每次 `render()` 方法执行时绑定类方法。总结来说组件每次运行更新时都会导致性能消耗。当在构造函数中绑定时，绑定只会在组件实例化时运行一次，这样做是一个更好的方式。

Another thing people sometimes come up with is defining the business logic of their class methods in the constructor.

另外有一些人们提出在构造函数中定义业务逻辑类方法。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

You should avoid it too, because it will clutter your constructor over time. The constructor is only there to instantiate your class with all its properties. That's why the business logic of class methods should be defined outside of the constructor.

你同样也应该避免这样，因为随着时间的推移它会让你的构造函数变得混乱。构造函数目的只是实例化你的类以及所有的属性。这就是为什么我们应该把业务逻辑应该定义在构造函数之外。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // do something
  }

  doSomethingElse() {
    // do something else
  }

  ...
}
~~~~~~~~

Last but not least, it is worth to mention that class methods can be autobound automatically without binding them explicitly by using JavaScript ES6 arrow functions.

最后值得一提的是类方法可以通过 ES6 的箭头函数做到自动地绑定。

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

If the repetitive binding in the constructor annoys you, you can go ahead with this approach instead. The official React documentation sticks to the class method bindings in the constructor. That's why the book will stick to those as well.

如果在构造函数中重复的绑定比较困扰你，你可以使用这种方式代替。在 React 的官方文档中坚持在构造函数中绑定类方法，所以本书也会采用同样方式。

### Exercises:

* try the different approaches of bindings and console log the `this` object

### 练习:

- 尝试绑定不同的方法并且控制台中打印 `this` 对象

## Event Handler

The chapter should give you a deeper understanding of event handlers in elements. In your application, you are using the following button element to dismiss an item from the list.

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

That's already a complex use case, because you have to pass a value to the class method and thus you have to wrap it into another (arrow) function. So basically, it has to be a function that is passed to the event handler. The following code wouldn't work, because the class method would be executed immediately when you open the application in the browser.

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

When using `onClick={doSomething()}`, the `doSomething()` function would execute immediately when you open the application in your browser. The expression in the handler is evaluated. Since the returned value of the function isn't a function anymore, nothing would happen when you click the button. But when using `onClick={doSomething}` whereas `doSomething` is a function, it would be executed when clicking the button. The same rules apply for the `onDismiss()` class method that is used in your application.

However, using `onClick={this.onDismiss}` wouldn't suffice, because somehow the `item.objectID` property needs to be passed to the class method to identify the item that is going to be dismissed. That's why it can be wrapped into another function to sneak in the property. The concept is called higher order functions in JavaScript and will be explained briefly later on.

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

A workaround would be to define the wrapping function somewhere outside and only pass the defined function to the handler. Since it needs access to the individual item, it has to live in the inside of the map function block.

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

After all, it has to be a function that is passed to the element's handler. As an example, try this code instead:

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

It will run when you open the application in the browser but not when you click the button. Whereas the following code would only run when you click the button. It is a function that is executed when you trigger the handler.

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

In order to keep it concise, you can transform it into a JavaScript ES6 arrow function again. That's what we did with the `onDismiss()` class method too.

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

Often newcomers to React have difficulties with the topic of using functions in event handlers. That's why I tried to explain it in more detail here. In the end, you should end up with the following code in your button to have a concisely inlined JavaScript ES6 arrow function that has access to the `objectID` property of the `item` object.

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

Another performance relevant topic, that is often mentioned, are the implications of using arrow functions in event handlers. For instance, the `onClick` handler for the `onDismiss()` method is wrapping the method in another arrow function to be able to pass the item identifier. So every time the `render()` method runs, the handler instantiates the higher order arrow function. It *can* have an impact on your application performance, but in most cases you will not notice it. Imagine you have a huge table of data with 1000 items and each row or column has such an arrow function in an event handler. Then it is worth to think about the performance implications and therefore you could implement a dedicated Button component to bind the method in the constructor. But before that happens it is premature optimization. It is more valuable to focus on learning React itself.

### Exercises:

* try the different approaches of using functions in the `onClick` handler of your button

## Interactions with Forms and Events

Let's add another interaction for the application to experience forms and events in React. The interaction is a search functionality. The input of the search field should be used to filter your list temporary based on the title property of an item.

In the first step, you are going to define a form with an input field in your JSX.

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

In the following scenario you will type into the input field and filter the list temporarily by the search term that is used in the input field. To be able to filter the list based on the value of the input field, you need to store the value of the input field in your local state. But how do you access the value? You can use **synthetic events** in React to access the event payload.

Let's define a `onChange` handler for the input field.

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

The function is bound to the component and thus a class method again. You have to bind and define the method.

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

When using a handler in your element, you get access to the synthetic React event in your callback function's signature.

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

The event has the value of the input field in its target object. Hence you are able to update the local state with the search term by using `this.setState()` again.

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

Additionally, you shouldn't forget to define the initial state for the `searchTerm` property in the constructor. The input field should be empty in the beginning and thus the value should be an empty string.

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

Now you store the input value to your internal component state every time the value in the input field changes.

A brief note about updating the local state in a React component. It would be fair to assume that when updating the `searchTerm` with `this.setState()` the list needs to be passed as well to preserve it. But that isn't the case. React's `this.setState()` is a shallow merge. It preserves the sibling properties in the state object when updating one sole property in it. Thus the list state, even though you have already dismissed an item from it, would stay the same when updating the `searchTerm` property.

Let's get back to your application. The list isn't filtered yet based on the input field value that is stored in the local state. Basically you have to filter the list temporarily based on the `searchTerm`. You have everything you need to filter it. So how to filter it temporarily now? In your `render()` method, before you map over the list, you can apply a filter on it. The filter would only evaluate if the `searchTerm` matches title property of the item. You have already used the built-in JavaScript filter functionality, so let's do it again. You can sneak in the filter function before the map function, because the filter function returns a new array and thus the map function can be used on it in such a convenient way.

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

Let's approach the filter function in a different way this time. We want to define the filter argument, the function that is passed to the filter function, outside of the ES6 class component. There we don't have access to the state of the component and thus we have no access to the `searchTerm` property to evaluate the filter condition. We have to pass the `searchTerm` to the filter function and have to return a new function to evaluate the condition. That's called a higher order function.

Normally I wouldn't mention higher order functions, but in a React book it makes total sense. It makes sense to know about higher order functions, because React deals with a concept called higher order components. You will get to know the concept later in the book. Now again, let's focus on the filter functionality.

First, you have to define the higher order function outside of your App component.

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

The function takes the `searchTerm` and returns another function, because after all the filter function takes a function as its input. The returned function has access to the item object because it is the function that is passed to the filter function. In addition, the returned function will be used to filter the list based on the condition defined in the function. Let's define the condition.

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

The condition says that you match the incoming `searchTerm` pattern with the title property of the item from your list. You can do that with the built-in `includes` JavaScript functionality. Only when the pattern matches, you return true and the item stays in the list. When the pattern doesn't match the item is removed from the list. But be careful with pattern matching: You shouldn't forget to lower case both strings. Otherwise there will be mismatches between a search term 'redux' and an item title 'Redux'. Since we are working on a immutable list and return a new list by using the filter function, the original list in the local state isn't modified at all.

One thing is left to mention: We cheated a bit by using the built-in includes JavaScript functionality. It is already an ES6 feature. How would that look like in JavaScript ES5? You would use the `indexOf()` function to get the index of the item in the list. When the item is in the list, `indexOf()` will return its index in the array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Another neat refactoring can be done with an ES6 arrow function again. It makes the function more concise:

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

One could argue which function is more readable. Personally I prefer the second one. The React ecosystem uses a lot of functional programming concepts. It happens often that you will use a function which returns a function (higher order functions). In JavaScript ES6, you can express these more concisely with arrow functions.

Last but not least, you have to use the defined `isSearched()` function to filter your list. You pass it the `searchTerm` property from your local state, it returns the filter input function, and filters your list based on the filter condition. Afterward it maps over the filtered list to display an element for each list item.

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

The search functionality should work now. Try it yourself in the browser.

### Exercises:

* read more about [React events](https://facebook.github.io/react/docs/handling-events.html)
* read more about [higher order functions](https://en.wikipedia.org/wiki/Higher-order_function)

## ES6 Destructuring

There is a way in JavaScript ES6 for an easier access to properties in objects and arrays. It's called destructuring. Compare the following snippet in JavaScript ES5 and ES6.

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

While you have to add an extra line each time you want to access an object property in JavaScript ES5, you can do it in one line in JavaScript ES6. A best practice for readability is to use multilines when you destructure an object into multiple properties.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

The same goes for arrays. You can destructure them too. Again, multilines will keep your code scannable and readable.

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

Perhaps you have noticed that the local state object in the App component can get destructured the same way. You can shorten the filter and map line of code.

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

You can do it the ES5 or ES6 way:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

But since the book uses JavaScript ES6 most of the time, you should stick to it.

### Exercises:

* read more about [ES6 destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Controlled Components

You already learned about the unidirectional data flow in React. The same law applies for the input field, which updates the local state with the `searchTerm` in order to filter the list. When the state changes, the `render()` method runs again and uses the recent `searchTerm` from the local state to apply the filter condition.

But didn't we forget something in the input element? A HTML input tag comes with a `value` attribute. The value attribute usually has the value that is shown in the input field. In this case it would be the `searchTerm` property. However, it seems like we don't need that in React.

That's wrong. Form elements such as `<input>`, `<textarea>` and `<select>` hold their own state in plain HTML. They modify the value internally once someone changes it from the outside. In React that's called an **uncontrolled component**, because it handles its own state. In React, you should make sure to make those elements **controlled components**.

How should you do that? You only have to set the value attribute of the input field. The value is already saved in the `searchTerm` state property. So why not access it from there?

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

That's it. The unidirectional data flow loop for the input field is self-contained now. The internal component state is the single source of truth for the input field.

The whole internal state management and unidirectional data flow might be new to you. But once you are used to it, it will be your natural flow to implement things in React. In general, React brought a novel pattern with the unidirectional data flow to the world of single page applications. It is adopted by several frameworks and libraries by now.

### Exercises:

* read more about [React forms](https://facebook.github.io/react/docs/forms.html)

## Split Up Components 拆分组件 

You have one large App component now. It keeps growing and can become confusing eventually. You can start to split it up into chunks of smaller components.

现在，你有一个大型的 App 组件。它在不停地扩展，最终可能会变得混乱。你可以开始将它拆分成若干个更小的组件。

Let's start to use a component for the search input and a component for the list of items.

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

You can pass those components properties which they can use themselves. In the case of the App component it needs to pass the properties managed in the local state and its class methods.

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

Now you can define the components next to your App component. Those components will be ES6 class components as well. They render the same elements like before.

现在你可以接着 App 组件定义这些组件。这些组件仍然是 ES6 类组件，它们会渲染和之前相同的元素。

The first one is the Search component.

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

The second one is the Table component.

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

Now you have three ES6 class components. Perhaps you have noticed the `props` object that is accessible via the class instance by using `this`. The props, short form for properties, have all the values you have passed to the components when you used them in your App component. That way, components can pass properties down the component tree.

现在你有了三个 ES6 类组件。你可能已经注意到，`props` 对象可以通过这个类实例的 `this` 来访问。props 是 properties 的简写，当你在 App 组件里面使用它时，它有你传递给这些组件的所有值。这样，组件可以沿着组件树向下传递属性。

By extracting those components from the App component, you would be able to reuse them somewhere else. Since components get their values by using the props object, you can pass every time different props to your components when you use them somewhere else. These components became reusable.

从 App 组件中提取这些组件之后，你就可以在别的地方去重用它们了。因为组件是通过 props 对象来获取它们的值，所以当你在别的地方重用它时，你可以每一次都传递不同的 props，这些组件就变得可复用了。

### Exercises: 练习：

* figure out further components that you could split up as you have done with the Search and Table components
* 从已经完成的 Search 和 Table 组件中找出可以进一步提取的组件
  * but don't do it now, otherwise you will run into conflicts in the next chapters
  * 但是不要现在就去做，否则在接下来的几个章节你会遇到冲突。  
  
## Composable Components 可组合组件

There is one more little property which is accessible in the props object: the `children` prop. You can use it to pass elements to your components from above, which are unknown to the component itself, but make it possible to compose components into each other. Let's see how this looks like when you only pass a text (string) as a child to the Search component.

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

Now the Search component can destructure the children property from the props object. Then it can specify where the children should be displayed.

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

The "Search" text should be visible next to your input field now. When you use the Search component somewhere else, you can choose a different text if you like. After all, it is not only text that you can pass as children. You can pass an element and element trees (which can be encapsulated by components again) as children. The children property makes it possible to weave components into each other.

现在，你应该可以在输入框旁边看到这个 "Search" 文本了。当你在别的地方使用 Search 组件时，如果你喜欢，你可以选择一个不同的文本。总之，它不仅可以把文本作为子元素传递，还可以将一个元素或者元素树 (它还可以再次封装成组件) 作为子元素传递。children 属性让组件相互交织在一起成为可能。

### Exercises: 练习：

* read more about [the composition model of React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)
* 阅读更多关于 [React 组件模型](https://facebook.github.io/react/docs/composition-vs-inheritance.html) 的内容
## Reusable Components 可复用组件

Reusable and composable components empower you to come up with capable component hierarchies. They are the foundation of React's view layer. The last chapters mentioned the term reusability. You can reuse the Table and Search components by now. Even the App component is reusable, because you could instantiate it somewhere else again.

可复用和可组合组件让你能够给出合理的组件分层，它们是 React 视图层的基础。前面几章提到了可重用性的术语。现在你可以复用 Search 和 Table 组件了。甚至 App 组件都是可复用的了，因为你可以在别的地方重新实例化它。

Let's define one more reusable component, a Button component, which gets reused more often eventually.

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

It might seem redundant to declare such a component. You will use a `Button` component instead of a `button` element. It only spares the `type="button"`. Except for the type attribute you have to define everything else when you want to use the Button component. But you have to think about the long term investment here. Imagine you have several buttons in your application, but want to change an attribute, style or behavior for the button. Without the component you would have to refactor every button. Instead the Button component ensures to have only one single source of truth. One Button to refactor all buttons at once. One Button to rule them all.

声明这样一个组件可能看起来有点多余。你将会用 `Button` 组件来替代 `button` 元素。它只省去了 `type="button"`。当你想使用 Button 组件的时候，你还得去定义除了 type 之外的所有属性。但这里你必须要考虑到长期投资。想象在你的应用中有若干个 button，但是你想改变它们的一个属性、样式或者行为。如果没有这个组件的话，你就必须重构每个 button。相反，Button 组件拥有单一可信数据源。一个 Button 组件可以立即重构所有 button。一个 Button 组件统治所有的 button。

Since you already have a button element, you can use the Button component instead. It omits the type attribute, because the Button component specifies it.

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

The Button component expects a `className` property in the props. The `className` attribute is another React derivate for the HTML attribute class. But we didn't pass any `className` when the Button was used. In the code it should be more explicit in the Button component that the `className` is optional.

Button 组件期望在 props 里面有一个 `className` 属性. `className` 属性是 React 基于 HTML 属性 `class` 的另一个衍生物。但是当使用 Button 组件时，我们并没有传递任何 `className` 属性，所以在 Button 组件的代码中，我们更应该明确地标明 `className` 是可选的。

Therefore, you can use the default parameter which is a JavaScript ES6 feature.

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

Now, whenever there is no `className` property specified when using the Button component, the value will be an empty string instead of `undefined`.

这样当使用 Button 组件时，若没有指定 `className` 属性，它的值就是一个空字符串，而非 `undefined`。

### Exercises: 练习：

* read more about [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

* 阅读更多关于 [ES6 默认参数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters) 的内容
## Component Declarations 组件声明

By now you have four ES6 class components. But you can do better. Let me introduce functional stateless components as alternative for ES6 class components. Before you will refactor your components, let's introduce the different types of components in React.

现在你已经有四个 ES6 类组件了，但是你可以做得更好。让我来介绍一下函数式无状态组件 (functional stateless components)，作为除了 ES6 类组件的另一个选择。在重构你的组件之前，让我来介绍一下 React 不同的组件类型。

* **Functional Stateless Components:** These components are functions which get an input and return an output. The input are the props. The output is a component instance thus plain JSX. So far it is quite similar to an ES6 class component. However, functional stateless components are functions (functional) and they have no local state (stateless). You cannot access or update the state with `this.state` or `this.setState()` because there is no `this` object. Additionally, they have no lifecycle methods. You didn't learn about lifecycle methods yet, but you already used two: `constructor()` and `render()`. Whereas the constructor runs only once in the lifetime of a component, the `render()` class method runs once in the beginning and every time the component updates. Keep in mind that functional stateless component have no lifecycle methods, when you arrive at the lifecycle methods chapter later on.

* **函数式无状态组件:** 这类组件就是函数，它们接收一个输入并返回一个输出。输入是 props，输出就是一个普通的 JSX 组件实例。到这里，它和 ES6 类组件非常的相似。然而，函数式无状态组件是函数 (函数式的)，并且它们没有本地状态 (无状态的)。你不能通过 `this.state` 或者 `this.setState()` 来访问或者更新状态，因为这里没有 `this` 对象。此外，它也没有生命周期方法。虽然你还没有学过生命周期方法，但是你已经用到了其中两个：`constructor()` and `render()`。constructor 在一个组件的生命周期中只执行一次，而 `render()` 方法会在最开始执行一次，并且每次组件更新时都会执行。当你阅读到后面关于生命周期方法的章节时，要记得函数式无状态组件是没有生命周期方法的。

* **ES6 Class Components:** You already used this type of component declaration in your four components. In the class definition, they extend from the React component. The `extend` hooks all the lifecycle methods, available in the React component API, to the component. That way you were able to use the `render()` class method. Additionally, you can store and manipulate state in ES6 class components by using `this.state` and `this.setState()`.

* **ES6 类组件:** 在你的四个组件中，你已经使用过这类组件了。在类的定义中，它们继承自 React 组件。`extend` 会注册所有的生命周期方法，只要在 React component API 中，都可以在你的组件中使用。通过这种方式你可以使用 `render()` 类方法。此外，通过使用 `this.state` 和 `this.setState()`，你可以在 ES6 类组件中储存和操控 state。

* **React.createClass:** The component declaration was used in older versions of React and still in JavaScript ES5 React applications. But [Facebook declared it as deprecated](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html) in favor of JavaScript ES6. They even added a [deprecation warning in version 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). You will not use it in the book.

* **React.createClass:** 这类组件声明曾经在老版本的 React 中使用，仍然存在于很多 ES5 React 应用中。但是为了支持 JavaScript ES6，[Facebook 声明它已经不推荐使用了](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html)。他们还[在 React 15.5 中加入了不推荐使用的警告](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html)。你不会在本书使用它。

So basically there are only two component declarations left. But when to use functional stateless components over ES6 class components? A rule of thumb is to use functional stateless components when you don't need local state or component lifecycle methods. Usually you start to implement your components as functional stateless components. Once you need access to the state or lifecycle methods, you have to refactor it to an ES6 class component. In our application, we started the other way around for the sake of learning React.

因此这里基本只剩下两种组件声明了。但是什么时候更适合使用函数式无状态组件而非 ES6 类组件？一个经验法则就是当你不需要本地状态或者组件生命周期方法时，你就应该使用函数式无状态组件。最开始一般使用函数式无状态组件来实现你的组件，一旦你需要访问 state 或者生命周期方法时，你就必须要将它重构成一个 ES6 类组件。在我们的应用中，为了学习 React，我们采取了相反的方式。

Let's get back to your application. The App component uses internal state. That's why it has to stay as an ES6 class component. But the other three of your ES6 class components are stateless. They don't need access to `this.state` or `this.setState()`. Even more, they have no lifecycle methods. Let's refactor together the Search component to a stateless functional component. The Table and Button component refactoring will remain as your exercise.

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

That's basically it. The props are accessible in the function signature and the return value is JSX. But you can do more code wise in a functional stateless component. You already know the ES6 destructuring. The best practice is to use it in the function signature to destructure the props.

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

But it can get better. You know already that ES6 arrow functions allow you to keep your functions concise. You can remove the block body of the function. In a concise body an implicit return is attached thus you can remove the return statement. Since your functional stateless component is a function, you can keep it concise as well.

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

The last step was especially useful to enforce only to have props as input and JSX as output. Nothing in between. Still, you could *do something* in between by using a block body in your ES6 arrow function.

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

But you don't need it for now. That's why you can keep the previous version without the block body. When using block bodies, people often tend to do too many things in the function. By leaving the block body out, you can focus on the input and output of your function.

但是你现在并不需要这样做，这也是为什么你可以让之前的版本没有块声明。当使用块声明时，人们往往容易在这个函数里面做过多的事情。通过移除块声明，你可以专注在函数的输入和输出上。

Now you have one lightweight functional stateless component. Once you would need access to its internal component state or lifecycle methods, you would refactor it to an ES6 class component. In addition you saw how JavaScript ES6 can be used in React components to make them more concise and elegant.

现在你已经有一个轻量的函数式无状态组件了。一旦你需要访问它的内部组件状态或者生命周期方法，你最好将它重构成一个 ES6 类组件。另外，你也已经看到，JavaScript ES6 是如何被用到 React 组件中并让它们变得更加的简洁和优雅。

### Exercises: 练习：

* refactor the Table and Button component to stateless functional components
* 将 Table 和 Button 组件重构成函数式无状态组件
* read more about [ES6 class components and functional stateless components](https://facebook.github.io/react/docs/components-and-props.html)
* 阅读更多关于 [ES6 类组件和函数式无状态组件](https://facebook.github.io/react/docs/components-and-props.html) 的内容

## Styling Components 给组件声明样式

Let's add some basic styling to your application and components. You can reuse the *src/App.css* and *src/index.css* files. These files should already be in your project since you have bootstrapped it with *create-react-app*. They should be imported in your *src/App.js* and *src/index.js* files too. I prepared some CSS which you can simply copy and paste to these files, but feel free to use your own style at this point.

让我们给你的应用和组件添加一些基本的样式。你可以复用 *src/App.css* 和 *src/index.css* 文件。因为你是用 *create-react-app* 来创建的，所以这些文件应该已经在你的项目中了。它们应该也被引入到你的 *src/App.js* 和 *src/index.js* 文件中了。我准备了一些 CSS，你可以直接复制粘贴到这些文件中，你也可以随意使用你自己的样式。

First, styling for your overall application.

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

Second, styling for your components in the App file.

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

Now you can use the style in some of your components. Don't forget to use React `className` instead of `class` as HTML attribute.

现在你可以在一些组件中使用这些样式。但是别忘了使用 React 的 `className`， 而不是 HTML 的 `class` 属性。

First, apply it in your App ES6 class component.

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

Second, apply it in your Table functional stateless component.

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

Now you have styled your application and components with basic CSS. It should look quite decent. As you know, JSX mixes up HTML and JavaScript. Now one could argue to add CSS in the mix as well. That's called inline style. You can define JavaScript objects and pass them to the style attribute of an element.

现在你已经给你的应用和组件添加了基本的 CSS 样式，看起来应该非常不错。如你所知，JSX 混合了 HTML 和 JavaScript。现在有人呼吁将 CSS 也加入进去，这就叫作内联样式 (inline style)。你可以定义 JavaScript 对象，并传给一个元素的 style 属性。

Let's keep the Table column width flexible by using inline style.

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

The style is inlined now. You could define the style objects outside of your elements to make it cleaner.

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

After that you would use them in your columns: `<span style={smallColumn}>`.

随后你可以将它们用于你的 columns ：`<span style={smallColumn}>`。

In general, you will find different opinions and solutions for style in React. You used pure CSS and inline style now. That's sufficient to get started.

总而言之，关于 React 中的样式，你会找到不同的意见和解决方案。现在你已经用过纯 CSS 和 内联样式了。这足以开始。

I don't want to be opinionated here, but I want to leave you some more options. You can read about them and apply them on your own. But if you are new to React, I would recommend to stick to pure CSS and inline style for now.

在这里我不想下定论，但是想给你一些更多的选择。你可以自行阅读并应用它们。但是如果你刚开始使用 React，目前我会推荐你坚持纯 CSS 和内联样式。

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)
* [CSS 模块化](https://github.com/css-modules/css-modules)

{pagebreak}

You have learned the basics to write your own React application! Let's recap the last chapters:

你已经学习了编写一个 React 应用所需要的基础知识了！让我们来回顾一下前面几个章节:

* React
  * use `this.state` and `setState()` to manage your internal component state
  * 使用 `this.state` 和 `setState()` 来管理你的内部组件状态
  * pass functions or class methods to your element handler
  * 将函数或者类方法传递到你的元素处理器
  * use forms and events in React to add interactions
  * 在 React 中使用表单或者事件来添加交互 
  * unidirectional data flow is an important concept in React
  * 在 React 中单向数据流是一个非常重要的概念
  * embrace controlled components
  * 拥抱 controlled components
  * compose components with children and reusable components
  * 通过 children 和可复用组件来组合组件
  * usage and implementation of ES6 class components and functional stateless components
  * ES6 类组件和函数式无状态组件的使用方法和实现
  * approaches to style your components
  * 给你的组件声明样式的方法
* ES6
  * functions that are bound to a class are class methods
  * 绑定到一个类的函数叫作类方法
  * destructuring of objects and arrays
  * 解构对象和数组
  * default parameters
  * 默认参数
* General
  * higher order functions
  * 高阶函数

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far. Additionally you can read more in the official [documentation](https://facebook.github.io/react/docs/installation.html).

是该再休息一下了，吸收这些知识然后转化成你自己的东西。你可以用你已有的代码来做个实验。另外，你可以进一步阅读[官方文档](https://facebook.github.io/react/docs/installation.html)

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.2).
你可以在[官方代码仓库](https://github.com/rwieruch/hackernews-client/tree/4.2)找到源码
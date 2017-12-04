# Basics in React

The chapter will guide you through the basics of React. It covers state and interactions in components, because static components are a bit dull, aren't they? Additionally, you will learn about the different ways to declare a component and how to keep components composable and reusable. Be prepared to breathe life into your components.

## Internal Component State

Internal component state, also known as local state, allows you to save, modify and delete properties that are stored in your component. The ES6 class component can use a constructor to initialize internal component state later on. The constructor is called only once when the component initializes.

Let's introduce a class constructor.

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

You can call `super(props);` as well. It sets `this.props` in your constructor in case you want to access them in the constructor. Otherwise, when accessing `this.props` in your constructor, they would be `undefined`. You will learn more about the props of a React component later on.

Now, in your case, the initial state in your component should be the sample list of items.

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

But be careful. Don't mutate the state directly. You have to use a method called `setState()` to modify your state. You will get to know it in a following chapter.

### Exercises:

* experiment with the local state
  * define more initial state in the constructor
  * use and access the state in your `render()` method
* read more about [the ES6 class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## ES6 Object Initializer

In JavaScript ES6, you can use a shorthand property syntax to initialize your objects more concisely. Imagine the following object initialization:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

When the property name in your object is the same as your variable name, you can do the following:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

In your application, you can do the same. The list variable name and the state property name share the same name.

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

### Exercises:

* experiment with ES6 object initializer
* read more about [ES6 object initializer](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Unidirectional Data Flow

Now you have some internal state in your App component. However, you have not manipulated the local state yet. The state is static and thus is the component. A good way to experience state manipulation is to have some component interaction.

Let's add a button for each item in the displayed list. The button says "Dismiss" and is going to remove the item from the list. It could be useful eventually when you only want to keep a list of unread items and dismiss the items that you are not interested in.

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

Did you notice the multilines for the button element? Note that elements with multiple attributes get messy as one line at some point. That's why the button element is used with multilines and indentations to keep it readable. But it is not mandatory. It is only a code style recommendation that I highly recommend.

Now you have to implement the `onDismiss()` functionality. It takes an id to identify the item to dismiss. The function is bound to the class and thus becomes a class method. That's why you access it with `this.onDismiss()` and not `onDismiss()`. The `this` object is your class instance. In order to define the `onDismiss()` as class method, you have to bind it in the constructor. Bindings will be explained in another chapter later on.

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

You can remove an item from a list by using the JavaScript built-in filter functionality. The filter function takes a function as input. The function has access to each value in the list, because it iterates over the list. That way, you can evaluate each item in the list based on a filter condition. If the evaluation for an item is true, the item stays in the list. Otherwise it will be filtered from the list. Additionally, it is good to know that the function returns a new list and doesn't mutate the old list. It supports the convention in React of having immutable data structures.

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

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

The list removes the clicked item now. However the state isn't updated yet. Therefore you can finally use the `setState()` class method to update the list in the internal component state.

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

![Internal state update with unidirectional data flow](images/set-state-to-render-unidirectional.png)

### Exercises:

* read more about [the state and lifecycle in React](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Bindings

It is important to learn about bindings in JavaScript classes when using React ES6 class components. In the previous chapter, you have bound your class method `onDismiss()` in the constructor.

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

In the following class component the class method is properly bound in the class constructor.

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

The class method binding can happen somewhere else too. For instance, it can happen in the `render()` class method.

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

Another thing people sometimes come up with is defining the business logic of their class methods in the constructor.

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

### Exercises:

* try the different approaches of bindings and console log the `this` object

>## Event Handler 
## 事件处理

>The chapter should give you a deeper understanding of event handlers in elements. In your application, you are using the following button element to dismiss an item from the list. 

本章节会让你对元素的事件处理有更深入的了解，在你的应用程序中，你使用下面的按钮来从列表中忽略一项内容

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

>That's already a complex use case, because you have to pass a value to the class method and thus you have to wrap it into another (arrow) function. So basically, it has to be a function that is passed to the event handler. The following code wouldn't work, because the class method would be executed immediately when you open the application in the browser. 

这已经是一个复杂的例子了，因为你必须传递一个参数到类的方法，因此你需要将它封装到另一个(箭头)函数中，基本上，它必须是一个调用事件处理的函数。下面的代码不会工作，因为类方法会在浏览器中打开程序时立即执行。

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

>When using `onClick={doSomething()}`, the `doSomething()` function would execute immediately when you open the application in your browser. The expression in the handler is evaluated. Since the returned value of the function isn't a function anymore, nothing would happen when you click the button. But when using `onClick={doSomething}` whereas `doSomething` is a function, it would be executed when clicking the button. The same rules apply for the `onDismiss()` class method that is used in your application. 

当使用 `onClick={doSomething()}` 时，`doSomething()` 函数会在浏览器打开程序时立即执行，监听表达式是函数执行的返回值而不再是函数，所以点击按钮时不会有任何事发生。但当使用 `onClick={doSomething}` 时，因为 `doSomething` 是一个函数，所以它会在点击按钮时执行。同样的规则也适用于在程序中使用的 `onDismiss()` 类方法。

>However, using `onClick={this.onDismiss}` wouldn't suffice, because somehow the `item.objectID` property needs to be passed to the class method to identify the item that is going to be dismissed. That's why it can be wrapped into another function to sneak in the property. The concept is called higher order functions in JavaScript and will be explained briefly later on. 

然而，使用 `onClick={this.onDismiss}` 并不够，因为 `item.objectID` 属性需要传递给类方法来识别那个将要被忽略的项，这就是为什么它需要被封装到另一个函数中来传递这个属性。这个概念在 JavaScript 中被称为高阶函数，稍后会做简要解释。

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

>A workaround would be to define the wrapping function somewhere outside and only pass the defined function to the handler. Since it needs access to the individual item, it has to live in the inside of the map function block. 

一个解决方案事在外部定义一个包装函数，并且只将定义的函数传递给处理程序。因为它需要访问单独的项，所以它必须位于 map 函数块的内部。

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

>After all, it has to be a function that is passed to the element's handler. As an example, try this code instead: 

毕竟，它必须是一个传递给元素处理程序的函数。作为一个示例，请尝试以下代码：

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

>It will run when you open the application in the browser but not when you click the button. Whereas the following code would only run when you click the button. It is a function that is executed when you trigger the handler. 

它将会在你在浏览器打开程序时执行，但点击按钮时并不会。而下面的代码只会在点击按钮时执行。它是一个在触发事件时才会执行的函数。

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

>In order to keep it concise, you can transform it into a JavaScript ES6 arrow function again. That's what we did with the `onDismiss()` class method too. 

为了保持简洁，你可以把它转成一个 JavaScript ES6 的箭头函数。和我们在 `onDismiss()` 类方法时做的一样。

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

>Often newcomers to React have difficulties with the topic of using functions in event handlers. That's why I tried to explain it in more detail here. In the end, you should end up with the following code in your button to have a concisely inlined JavaScript ES6 arrow function that has access to the `objectID` property of the `item` object. 

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

>Another performance relevant topic, that is often mentioned, are the implications of using arrow functions in event handlers. For instance, the `onClick` handler for the `onDismiss()` method is wrapping the method in another arrow function to be able to pass the item identifier. So every time the `render()` method runs, the handler instantiates the higher order arrow function. It *can* have an impact on your application performance, but in most cases you will not notice it. Imagine you have a huge table of data with 1000 items and each row or column has such an arrow function in an event handler. Then it is worth to think about the performance implications and therefore you could implement a dedicated Button component to bind the method in the constructor. But before that happens it is premature optimization. It is more valuable to focus on learning React itself. 

另一个性能相关的话题会经常被提到是在事件处理程序中使用箭头函数的影响。例如，`onClick` 事件处理中的 `onDismiss()` 方法被封装在另一个箭头函数中以便能传递项标识。因此每次 `render()` 执行时，事件处理程序就会实例化一个高阶箭头函数，它可能会对你的程序性能产生影响，但在大多数情况下你都不会注意到这个问题。假设你有一个包含1000个项目的巨大数据表，每一行或者列在事件处理程序中都有这样一个箭头函数，这个时候就需要考虑性能影响，因此你可以实现一个专用的按钮组件来在构造函数中绑定方法，但这是一个不成熟的优化。在现在，专注到学习 React 会更有价值。

### Exercises: 练习：

* try the different approaches of using functions in the `onClick` handler of your button 

>尝试在按钮的 `onClick` 处理程序中使用函数的不同方法

>## Interactions with Forms and Events
## 和表单交互

>Let's add another interaction for the application to experience forms and events in React. The interaction is a search functionality. The input of the search field should be used to filter your list temporary based on the title property of an item.

让我们在程序中加入表单来体验 React 和表单事件的交互，我们将在程序中加入搜索功能，列表项的内容会根据输入框的内容进行过滤。

>In the first step, you are going to define a form with an input field in your JSX.

第一步，你需要在 JSX 中定义一个带有输入框的表单。

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

>In the following scenario you will type into the input field and filter the list temporarily by the search term that is used in the input field. To be able to filter the list based on the value of the input field, you need to store the value of the input field in your local state. But how do you access the value? You can use **synthetic events** in React to access the event payload.

在下面的场景中，将会使用在输入框中的内容作为搜索字段来临时过滤列表。为了能根据输入框的值过滤列表，你需要将输入框的值储存在你的本地状态中，但是如何访问这个值呢，你可以使用 React 的 **合成事件** 来访问事件返回值。

>Let's define a `onChange` handler for the input field.

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

>The function is bound to the component and thus a class method again. You have to bind and define the method.

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

>When using a handler in your element, you get access to the synthetic React event in your callback function's signature.

在元素中使用监听时，你可以在回调函数的签名中访问到 React 的合成事件。

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

>The event has the value of the input field in its target object. Hence you are able to update the local state with the search term by using `this.setState()` again.

该事件中有输入框的值在 target 对象中，因此你可以使用 `this.setState()` 来更新本地的搜索词的状态了。

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

>Additionally, you shouldn't forget to define the initial state for the `searchTerm` property in the constructor. The input field should be empty in the beginning and thus the value should be an empty string.

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

>Now you store the input value to your internal component state every time the value in the input field changes.

现在你将会把输入框每次变化的输入值储存到组件的内部状态中。

>A brief note about updating the local state in a React component. It would be fair to assume that when updating the `searchTerm` with `this.setState()` the list needs to be passed as well to preserve it. But that isn't the case. React's `this.setState()` is a shallow merge. It preserves the sibling properties in the state object when updating one sole property in it. Thus the list state, even though you have already dismissed an item from it, would stay the same when updating the `searchTerm` property.

关于一个在 React 组件中更新状态的简要说明，在使用 `this.setState()` 更新 `searchTerm` 时应该把这个列表也传递进去来保留它才是公平的，但事实并非如此，React 的 `this.setState()` 是一个浅合并，在更新一个唯一的属性时，他会保留状态对象中的其他属性，因此即使你已经在列表状态中排除了一个项，在更新 `searchTerm` 属性时也会保持不变。

>Let's get back to your application. The list isn't filtered yet based on the input field value that is stored in the local state. Basically you have to filter the list temporarily based on the `searchTerm`. You have everything you need to filter it. So how to filter it temporarily now? In your `render()` method, before you map over the list, you can apply a filter on it. The filter would only evaluate if the `searchTerm` matches title property of the item. You have already used the built-in JavaScript filter functionality, so let's do it again. You can sneak in the filter function before the map function, because the filter function returns a new array and thus the map function can be used on it in such a convenient way.

让我们回到你的程序中，现在列表还没有根据储存在本地状态中的输入字段进行过滤。基本上，你已经具有了根据 `searchTerm` 临时过滤列表的所有东西。那么怎么暂时的过滤它呢？你可以在 `render()` 方法中，在 map 映射列表之前，插入一个过滤的方法。这个过滤方法将只会匹配标题属性中有 `searchTerm` 内容的列表项。你已经使用过了 JavaScript 内置的 filter 功能，让我们再用一次，你可以在 map 函数之前加入 filter 函数，因为 filter 函数返回一个新的数组，所以 map 函数可以这样方便的使用。

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

>Let's approach the filter function in a different way this time. We want to define the filter argument, the function that is passed to the filter function, outside of the ES6 class component. There we don't have access to the state of the component and thus we have no access to the `searchTerm` property to evaluate the filter condition. We have to pass the `searchTerm` to the filter function and have to return a new function to evaluate the condition. That's called a higher order function.

让我们用一种不同的方式来处理过滤函数，我们想在 ES6 组件类之外定义一个传递给过滤函数的函数参数，在这里我们不能访问到组件内的状态，所以无法访问 `searchTerm` 属性来作为筛选条件求值，我们需要传递 `searchTerm` 到过滤函数并返回一个新函数来根据条件求值，这叫做高阶函数。

>Normally I wouldn't mention higher order functions, but in a React book it makes total sense. It makes sense to know about higher order functions, because React deals with a concept called higher order components. You will get to know the concept later in the book. Now again, let's focus on the filter functionality.

一般来说，我不会提到高阶函数，但在 React 中了解高阶函数是有意义的，因为在 React 中有一个高阶组件的概念，你将在这本书的后面了解到这个概念。但现在，让我们关注到过滤器的功能。

>First, you have to define the higher order function outside of your App component.

首先，你需要在程序组件外定义一个高阶函数。

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

>The function takes the `searchTerm` and returns another function, because after all the filter function takes a function as its input. The returned function has access to the item object because it is the function that is passed to the filter function. In addition, the returned function will be used to filter the list based on the condition defined in the function. Let's define the condition.

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

>The condition says that you match the incoming `searchTerm` pattern with the title property of the item from your list. You can do that with the built-in `includes` JavaScript functionality. Only when the pattern matches, you return true and the item stays in the list. When the pattern doesn't match the item is removed from the list. But be careful with pattern matching: You shouldn't forget to lower case both strings. Otherwise there will be mismatches between a search term 'redux' and an item title 'Redux'. Since we are working on a immutable list and return a new list by using the filter function, the original list in the local state isn't modified at all.

条件是列表中项目的标题属性和输入的 `searchTerm` 参数相匹配，你可以使用 JavaScript 内置的 `includes` 功能来实现这一点。只有满足匹配时才会返回 true 并将项目保留在列表中。当不匹配时，项目会从列表中移除。但需要注意的是，你需要把输入内容和待匹配的内容都转换成小写，否则，搜索词 'redux' 和列表中标题叫 'Redux' 的项目无法匹配。由于我们使用的是一个不可变的列表，并使用 filter 函数返回一个新列表，所以本地状态中的原始列表根本就没有被修改过。

>One thing is left to mention: We cheated a bit by using the built-in includes JavaScript functionality. It is already an ES6 feature. How would that look like in JavaScript ES5? You would use the `indexOf()` function to get the index of the item in the list. When the item is in the list, `indexOf()` will return its index in the array.

还有一点需要注意，我们使用了 Javascript 内置的 includes 功能，它已经是一个 ES6 的特性了。这在 ES5 中该如何实现呢？你将使用 `indexOf()` 函数来获取列表中项的索引，当项目在列表中时，`indexOf()` 将会返回它的索引。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

>Another neat refactoring can be done with an ES6 arrow function again. It makes the function more concise:

另一个优雅的重构可以用 ES6 箭头函数完成，它可以让函数更加整洁:

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

人们会争论哪个函数更易读，就我个人而言，我更习惯第二个。React 的生态使用了大量的函数式编程概念。通常情况下，你会使用一个函数返回另一个函数（高阶函数）。在 JavaScript ES6 中，可以使用箭头函数更简洁的表达这些。

>Last but not least, you have to use the defined `isSearched()` function to filter your list. You pass it the `searchTerm` property from your local state, it returns the filter input function, and filters your list based on the filter condition. Afterward it maps over the filtered list to display an element for each list item.

最后，你需要使用定义的 `isSearched()` 函数来过滤你的列表，你从本地状态中传递 `searchTerm` 属性返回一个根据条件过滤列表的输入过滤函数。之后它会映射过滤后的列表用于显示每个列表项的元素。

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

>The search functionality should work now. Try it yourself in the browser.

搜索功能现在应该起作用了，在浏览器中自己尝试一下。

>### Exercises:
### 练习：

>* read more about [React events](https://facebook.github.io/react/docs/handling-events.html)
* 阅读更多 [React 事件](https://facebook.github.io/react/docs/handling-events.html) 相关内容
>* read more about [higher order functions](https://en.wikipedia.org/wiki/Higher-order_function)
* 阅读更多 [高阶函数](https://en.wikipedia.org/wiki/Higher-order_function) 相关内容

>## ES6 Destructuring
## ES6 解构

>There is a way in JavaScript ES6 for an easier access to properties in objects and arrays. It's called destructuring. Compare the following snippet in JavaScript ES5 and ES6.

在 JavaScript ES6 中有一种更方便的方法来访问对象和数组的属性，叫做解构。比较下面 JavaScript ES5 和 ES6 的代码片段。

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

>While you have to add an extra line each time you want to access an object property in JavaScript ES5, you can do it in one line in JavaScript ES6. A best practice for readability is to use multilines when you destructure an object into multiple properties.

在 JavaScript ES5 中每次访问对象的属性都需要额外添加一行代码，但在 JavaScript ES6 中可以在一行中进行。可读性最好的方法是在将对象解构成多个属性时使用多行。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

>The same goes for arrays. You can destructure them too. Again, multilines will keep your code scannable and readable.

对于数组一样可以使用解构，同样，多行代码会使你的代码保持可读性。

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

>Perhaps you have noticed that the local state object in the App component can get destructured the same way. You can shorten the filter and map line of code.

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

>You can do it the ES5 or ES6 way:

你可以使用 ES5 或者 ES6 的方式来做：

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

>But since the book uses JavaScript ES6 most of the time, you should stick to it.

由于这本书大部分时候都使用了 JavaScript ES6，所以你应该坚持使用它。

>### Exercises:
### 练习：

>* read more about [ES6 destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

* 阅读更多[ES6 解构](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)的相关内容

>## Controlled Components

## 受控组件

>You already learned about the unidirectional data flow in React. The same law applies for the input field, which updates the local state with the `searchTerm` in order to filter the list. When the state changes, the `render()` method runs again and uses the recent `searchTerm` from the local state to apply the filter condition.

你已经了解了 React 中的单向数据流, 同样的规则适用于更新本地状态 `searchTerm` 来过滤列表的输入框。当状态变化时，`render()` 方法将再次运行，并使用最新状态中的`searchTerm` 值来作为过滤条件。

>But didn't we forget something in the input element? A HTML input tag comes with a `value` attribute. The value attribute usually has the value that is shown in the input field. In this case it would be the `searchTerm` property. However, it seems like we don't need that in React.

但是我们是否忘记了输入元素的一些东西？一个 HTML 输入标签带有一个 `value` 属性，这个属性通常有一个值作为输入框的显示，在本例中，它是 `searchTerm` 属性。然而，看起来我们在 React 好像并不需要它。

>That's wrong. Form elements such as `<input>`, `<textarea>` and `<select>` hold their own state in plain HTML. They modify the value internally once someone changes it from the outside. In React that's called an **uncontrolled component**, because it handles its own state. In React, you should make sure to make those elements **controlled components**.

这是错误的，表单元素比如 `<input>`, `<textarea>` 和 `<select>` 会以原生 HTML 的形式保存他们自己的状态。一旦有人从外部做了一些修改，它们就会修改内部的值，在 React 中这被称为**不受控组件**，因为它们自己处理状态。在 React 中，你应该确保这些元素变为**受控组件**

>How should you do that? You only have to set the value attribute of the input field. The value is already saved in the `searchTerm` state property. So why not access it from there?

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

>That's it. The unidirectional data flow loop for the input field is self-contained now. The internal component state is the single source of truth for the input field.

就是这样。现在输入框的单项数据流循环是自包含的，组件内部状态是输入框的唯一数据来源。

>The whole internal state management and unidirectional data flow might be new to you. But once you are used to it, it will be your natural flow to implement things in React. In general, React brought a novel pattern with the unidirectional data flow to the world of single page applications. It is adopted by several frameworks and libraries by now.

整个内部状态管理和单向数据流可能对你来说比较新，但你一旦习惯了它，你就会自然而然的在 React 中实现它。一般来说，React 带来一种新的模式，将单向数据流引入到单页面应用的生态中，到目前为止，它已经被几个框架和库所采用。

>### Exercises:
### 练习

>* read more about [React forms](https://facebook.github.io/react/docs/forms.html)

* 阅读更多[React 表单](https://facebook.github.io/react/docs/forms.html)相关内容

## Split Up Components

You have one large App component now. It keeps growing and can become confusing eventually. You can start to split it up into chunks of smaller components.

Let's start to use a component for the search input and a component for the list of items.

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

The first one is the Search component.

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

By extracting those components from the App component, you would be able to reuse them somewhere else. Since components get their values by using the props object, you can pass every time different props to your components when you use them somewhere else. These components became reusable.

### Exercises:

* figure out further components that you could split up as you have done with the Search and Table components
  * but don't do it now, otherwise you will run into conflicts in the next chapters

## Composable Components

There is one more little property which is accessible in the props object: the `children` prop. You can use it to pass elements to your components from above, which are unknown to the component itself, but make it possible to compose components into each other. Let's see how this looks like when you only pass a text (string) as a child to the Search component.

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

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-inserdd
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

### Exercises:

* read more about [the composition model of React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

## Reusable Components

Reusable and composable components empower you to come up with capable component hierarchies. They are the foundation of React's view layer. The last chapters mentioned the term reusability. You can reuse the Table and Search components by now. Even the App component is reusable, because you could instantiate it somewhere else again.

Let's define one more reusable component, a Button component, which gets reused more often eventually.

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

Since you already have a button element, you can use the Button component instead. It omits the type attribute, because the Button component specifies it.

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

Therefore, you can use the default parameter which is a JavaScript ES6 feature.

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

### Exercises:

* read more about [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Component Declarations

By now you have four ES6 class components. But you can do better. Let me introduce functional stateless components as alternative for ES6 class components. Before you will refactor your components, let's introduce the different types of components in React.

* **Functional Stateless Components:** These components are functions which get an input and return an output. The input are the props. The output is a component instance thus plain JSX. So far it is quite similar to an ES6 class component. However, functional stateless components are functions (functional) and they have no local state (stateless). You cannot access or update the state with `this.state` or `this.setState()` because there is no `this` object. Additionally, they have no lifecycle methods. You didn't learn about lifecycle methods yet, but you already used two: `constructor()` and `render()`. Whereas the constructor runs only once in the lifetime of a component, the `render()` class method runs once in the beginning and every time the component updates. Keep in mind that functional stateless component have no lifecycle methods, when you arrive at the lifecycle methods chapter later on.

* **ES6 Class Components:** You already used this type of component declaration in your four components. In the class definition, they extend from the React component. The `extend` hooks all the lifecycle methods, available in the React component API, to the component. That way you were able to use the `render()` class method. Additionally, you can store and manipulate state in ES6 class components by using `this.state` and `this.setState()`.

* **React.createClass:** The component declaration was used in older versions of React and still in JavaScript ES5 React applications. But [Facebook declared it as deprecated](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html) in favor of JavaScript ES6. They even added a [deprecation warning in version 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). You will not use it in the book.

So basically there are only two component declarations left. But when to use functional stateless components over ES6 class components? A rule of thumb is to use functional stateless components when you don't need local state or component lifecycle methods. Usually you start to implement your components as functional stateless components. Once you need access to the state or lifecycle methods, you have to refactor it to an ES6 class component. In our application, we started the other way around for the sake of learning React.

Let's get back to your application. The App component uses internal state. That's why it has to stay as an ES6 class component. But the other three of your ES6 class components are stateless. They don't need access to `this.state` or `this.setState()`. Even more, they have no lifecycle methods. Let's refactor together the Search component to a stateless functional component. The Table and Button component refactoring will remain as your exercise.

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

Now you have one lightweight functional stateless component. Once you would need access to its internal component state or lifecycle methods, you would refactor it to an ES6 class component. In addition you saw how JavaScript ES6 can be used in React components to make them more concise and elegant.

### Exercises:

* refactor the Table and Button component to stateless functional components
* read more about [ES6 class components and functional stateless components](https://facebook.github.io/react/docs/components-and-props.html)

## Styling Components

Let's add some basic styling to your application and components. You can reuse the *src/App.css* and *src/index.css* files. These files should already be in your project since you have bootstrapped it with *create-react-app*. They should be imported in your *src/App.js* and *src/index.js* files too. I prepared some CSS which you can simply copy and paste to these files, but feel free to use your own style at this point.

First, styling for your overall application.

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

First, apply it in your App ES6 class component.

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

Let's keep the Table column width flexible by using inline style.

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

In general, you will find different opinions and solutions for style in React. You used pure CSS and inline style now. That's sufficient to get started.

I don't want to be opinionated here, but I want to leave you some more options. You can read about them and apply them on your own. But if you are new to React, I would recommend to stick to pure CSS and inline style for now.

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)

{pagebreak}

You have learned the basics to write your own React application! Let's recap the last chapters:

* React
  * use `this.state` and `setState()` to manage your internal component state
  * pass functions or class methods to your element handler
  * use forms and events in React to add interactions
  * unidirectional data flow is an important concept in React
  * embrace controlled components
  * compose components with children and reusable components
  * usage and implementation of ES6 class components and functional stateless components
  * approaches to style your components
* ES6
  * functions that are bound to a class are class methods
  * destructuring of objects and arrays
  * default parameters
* General
  * higher order functions

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far. Additionally you can read more in the official [documentation](https://facebook.github.io/react/docs/installation.html).

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.2).

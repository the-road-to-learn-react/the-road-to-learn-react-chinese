# Introduction to React

The chapter gives you an introduction to React. You may ask yourself: Why should I learn React in the first place? The chapter might give you the answer to that question. Afterward, you will dive into the ecosystem by bootstrapping your first React application from scratch with zero-configuration. Along the way, you will get an introduction to JSX and ReactDOM. So be prepared for your first React components.

## Hi, my name is React.

**Why should you bother to learn React?** In recent years single page applications ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) have become popular. Frameworks like Angular, Ember and Backbone helped JavaScript developers to build modern web applications beyond the usage of vanilla JavaScript and jQuery. The list of these popular solutions is not exhaustive. There exists a wide range of SPA frameworks. When you consider the release dates, most of them are among the first generation of SPAs: Angular 2010, Backbone 2010 and Ember 2011.

The initial React release was 2013 by Facebook. React is not an SPA framework but a view library. It is the V in the [MVC](https://de.wikipedia.org/wiki/Model_View_Controller) (model view controller). It only enables you to render components as viewable elements in a browser. Yet the whole ecosystem around React makes it possible to build single page applications.

But why should you consider using React over the first generation of SPA frameworks? While the first generation of frameworks tried to solve a lot of things at once, React only helps you to build your view layer. It's a library and not a framework. The idea behind it: Your view is a hierarchy of composable components.

In React you can keep the focus on your view layer before you introduce more aspects to your application. Every other aspect is another building block for your SPA. These building blocks are essential to build a mature application. They come with two advantages.

First, you can learn the building blocks step by step. You don't have to worry about understanding them altogether. It is different from a framework that gives you every building block from the start. This book focuses on React as the first building block. More building blocks follow eventually.

Second, all building blocks are interchangeable. It makes the ecosystem around React such an innovative place. Multiple solutions are competing with each other. You can pick the most appealing solution for you and your use case.

The first generation of SPA frameworks arrived at an enterprise level. They are more rigid. React stays innovative and gets adopted by multiple tech thought leader companies like [Airbnb, Netflix and of course Facebook](https://github.com/facebook/react/wiki/Sites-Using-React). All of them invest in the future of React and are content with React and the ecosystem itself.

React is probably one of the best choices for building modern web applications nowadays. It only delivers the view layer, [but the React ecosystem is a whole flexible and interchangeable framework](https://www.robinwieruch.de/essential-react-libraries-framework/). React has a slim API, an amazing ecosystem and a great community. You can read about my experiences [why I moved from Angular to React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/). I highly recommend to have an understanding why you would choose React over another framework or library. After all, everyone is keen to experience where React will lead us in the next years.

### Exercises

* read about [why I moved from Angular to React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* read about [React's flexible ecosystem](https://www.robinwieruch.de/essential-react-libraries-framework/)

## Requirements

If you are coming from a different SPA framework or library, you should already be familiar with the basics of web development. If you have just started in web development, you should feel comfortable with HTML, CSS and JavaScript ES5 to learn React. The book will smoothly transition to JavaScript ES6 and beyond. I encourage you to join the official [Slack Group](https://slack-the-road-to-learn-react.wieruch.com/) for the book to get help or to help others.

### Editor and Terminal

What about the development environment? You will need a running editor or IDE and terminal (command line tool). You can [follow my setup guide](https://www.robinwieruch.de/developer-setup/). It is adjusted for MacOS users, but you can substitute most of the tools for other operating system. There is a ton of articles out there that will show you how to setup a web development environment in a more elaborated way for your OS.

Optionally, you can use git and GitHub on your own, while conducting the exercises in the book, to keep your projects and the progress in repositories on GitHub. There exists a [little guide](https://www.robinwieruch.de/git-essential-commands/) on how to use these tools. But once again, it is not mandatory for the book and can be overwhelming when learning everything from scratch. So you can skip it if you are a newcomer in web development to focus on the essential parts taught in this book.

### Node and NPM

Last but not least, you will need an installation of [node and npm](https://nodejs.org/en/). Both are used to manage libraries you will need along the way. In this book, you will install external node packages via npm (node package manager). These node packages can be libraries or whole frameworks.

You can verify your versions of node and npm on the command line. If you don't get any output in the terminal, you need to install node and npm first. These are only my versions during the time writing this book:

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.3.0
npm --version
*v5.5.1
~~~~~~~~

## node and npm

This chapter gives you a little crash course in node and npm. It is not exhaustive, but you will get all the necessary tools. If you are familiar with both of them, you can skip the chapter.

The **node package manager** (npm) allows you to install external **node packages** from the command line. These packages can be a set of utility functions, libraries or whole frameworks. They are the dependencies of your application. You can either install these packages to your global node package folder or to your local project folder.

Global node packages are accessible from everywhere in the terminal and you have to install them only once to your global directory. You can install a global package by typing in your terminal:

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

The `-g` flag tells npm to install the package globally. Local packages are used in your application. For instance, React as a library will be a local package which can be required in your application for usage. You can install it via the terminal by typing:

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

In the case of React it would be:

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

The installed package will automatically appear in a folder called *node_modules/* and will be listed in the *package.json* file next to your other dependencies.

But how to initialize the *node_modules/* folder and the *package.json* file for your project in the first place? There is a npm command to initialize a npm project and thus a *package.json* file. Only when you have that file, you can install new local packages via npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

The `-y` flag is a shortcut to initialize all the defaults in your *package.json*. If you don't use the flag, you have to decide how to configure the file. After initializing your npm project you are good to install new packages via `npm install <package>`.

One more word about the *package.json*. The file enables you to share your project with other developers without sharing all the node packages. The file has all the references of node packages used in your project. These packages are called dependencies. Everyone can copy your project without the dependencies. The dependencies are references in the *package.json*. Someone who copies your project can simply install all packages by using `npm install` on the command line. The `npm install` script takes all the dependencies listed in the *package.json* file and installs them in the *node_modules/* folder.

I want to cover one more npm command:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

The `--save-dev` flag indicates that the node package is only used in the development environment. It will not be used in production when you deploy your application on a server. What kind of node package could that be? Imagine you want to test your application with the help of a node package. You need to install that package via npm, but want to exclude it from your production environment. Testing should only happen during the development process but not when your application is already running in production. There you don't want to test your application anymore. It should be tested already and work out of the box for your users. That's only one use case where you would want to use the `--save-dev` flag.

You will encounter more npm commands on your way. But these will be sufficient for now.

### Exercises:

* setup a throw away npm project
  * create a new folder with `mkdir <folder_name>`
  * navigate into the folder with `cd <folder_name>`
  * execute `npm init -y` or `npm init`
  * install a local package like React with `npm install react`
  * have a look into the *package.json* file and the *node_modules/* folder
  * find out on your own how to uninstall the *react* node package again
* read more about [npm](https://docs.npmjs.com/)

## Installation

There are multiple approaches to get started with a React application.

The first one is to use a CDN. That may sound more complicated than it is. A CDN is a [content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network). Several companies have CDNs that host files publicly for people to consume them. These files can be libraries like React, because after all the bundled React library is only a *react.js* JavaScript file. It can be hosted somewhere and you can require it in your application.

How to use a CDN to get started in React? You can inline the `<script>` tag in your HTML that points to a CDN url. To get started in React you need two files (libraries): *react* and *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

But why should you use a CDN when you have npm to install node packages such as React?

When your application has a *package.json* file, you can install *react* and *react-dom* from the command line. The requirement is that the folder is initialized as npm project by using `npm init -y` with a *package.json* file. You can install multiple node packages in one line with npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

That approach is often used to add React to an existing application that is managed with npm.

Unfortunately that's not everything. You would have to deal with [Babel](http://babeljs.io/) to make your application aware of JSX (the React syntax) and JavaScript ES6. Babel transpiles your code so that browsers can interpret JavaScript ES6 and JSX. Not all browsers are capable of interpreting the syntax. The setup includes a lot of configuration and tooling. It can be overwhelming for React newcomers to bother with all the configuration.

Because of this reason, Facebook introduced *create-react-app* as a zero-configuration React solution. The next chapter will show you how to setup your application by using this bootstrapping tool.

### Exercises:

* read more about [React installations](https://facebook.github.io/react/docs/installation.html)

## Zero-Configuration Setup

In the Road to learn React, you will use [create-react-app](https://github.com/facebookincubator/create-react-app) to bootstrap your application. It's an opinionated yet zero-configuration starter kit for React introduced by Facebook in 2016. People would [recommend it to beginners by 96%](https://twitter.com/dan_abramov/status/806985854099062785). In *create-react-app* the tooling and configuration evolve in the background while the focus is on the application implementation.

To get started, you will have to install the package to your global node packages. After that, you always have it available on the command line to bootstrap new React applications.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

You can check the version of *create-react-app* to verify a successful installation on your command line:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~

Now you can bootstrap your first React application. We call it *hackernews*, but you can choose a different name. The bootstrapping takes a couple of seconds. Afterward, simply navigate into the folder:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Now you can open the application in your editor. The following folder structure, or a variation of it depending on the *create-react-app* version, should be presented to you:

{title="Folder Structure",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
~~~~~~~~

A short break down of the folder and files. It is fine if you don't understand all of them in the beginning.

* **README.md:** The .md extension indicates that the file is a markdown file. Markdown is used as a lightweight markup language with plain text formatting syntax. Many source code projects come with a *README.md* file to give you initial instructions about the project. When pushing your project to a platform such as GitHub eventually, the *README.md* file will show its content prominently when you access the repository. Because you have used *create-react-app*, your *README.md* should be the same as shown in the official [create-react-app GitHub repository](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** The folder has all the node packages that were and are installed via npm. Since you have used *create-react-app*, there should be already a couple of node modules installed for you. Usually you will never touch this folder, but only install and uninstall node packages with npm from the command line.

* **package.json:** The file shows you a list of node package dependencies and other project configuration.

* **.gitignore:** The file indicates all files and folders that shouldn't be added to your remote git repository when using git. They should only live in your local project. The *node_modules/* folder is such a use case. It is sufficient to share the *package.json* file with your peers to enable them to install all dependencies on their own without sharing the whole dependency folder.

* **public/:** The folder holds all your files when building your project for production. Eventually all your written code in the *src/* folder will be bundled into a couple of files when building your project and placed in the public folder.

After all, you don't need to touch the mentioned files and folders. In the beginning everything you need is located in the *src/* folder. The main focus lies on the *src/App.js* file to implement React components. It will be used to implement your application, but later you might want to split up your components into multiple files whereas each file maintains one or a few components on its own.

Additionally, you will find a *src/App.test.js* file for your tests and a *src/index.js* as entry point to the React world. You will get to know both files in a later chapter. In addition, there is a *src/index.css* and a *src/App.css* file to style your general application and your components. They all come with default style when you open them.

The *create-react-app* application is a npm project. You can use npm to install and uninstall node packages to your project. Additionally it comes with the following npm scripts for your command line:

{title="Command Line",lang="text"}
~~~~~~~~
// Runs the application in http://localhost:3000
npm start

// Runs the tests
npm test

// Builds the application for production
npm run build
~~~~~~~~

The scripts are defined in your *package.json*. Your boilerplate React application is bootstrapped now. The exciting part comes in the exercises to finally run your bootstrapped application in the browser.

### Exercises:

* `npm start` your application and visit the application in your browser
* run the interactive `npm test` script
* run the `npm run build` script and verify that a *build/* folder was added to your project (you can remove it again afterward; note that the build folder can be used later on to [deploy your application](https://www.robinwieruch.de/deploy-applications-digital-ocean/))
* make yourself familiar with the folder structure
* make yourself familiar with the content of the files
* read more about [the npm scripts and create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduction to JSX

Now you will get to know JSX. It is the syntax in React. As mentioned before, *create-react-app* has already bootstrapped a boilerplate application for you. All files come with default implementations. Let's dive into the source code. The only file you will touch in the beginning will be the *src/App.js* file.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Don't let yourself get confused by the import/export statements and class declaration. These features are already JavaScript ES6. We will revisit those in a later chapter.

In the file you have an **React ES6 class component** with the name App. It is a component declaration. Basically after you have declared a component, you can use it as element everywhere in your application. It will produce an **instance** of your **component** or in other words: the component gets instantiated.

The **element** it returns is specified in the `render()` method. Elements are what components are made of. It is useful to understand the differences between component, instance and element.

Pretty soon, you will see where the App component is instantiated. Otherwise you wouldn't see the rendered output in the browser, would you? The App component is only the declaration, but not the usage. You would instantiate the component somewhere in your JSX with `<App />`.

The content in the render block looks pretty similar to HTML, but it's JSX. JSX allows you to mix HTML and JavaScript. It's powerful yet confusing when you are used to separate your HTML and JavaScript. That's why a good starting point is to use basic HTML in your JSX. In the beginning, remove all the distracting content in the file.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Now, you only return HTML in your `render()` method without JavaScript. Let's define the "Welcome to the Road to learn React" as a variable. A variable can be used in your JSX by using curly braces.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

It should work when you start your application on the command line with `npm start` again.

Additionally you might have noticed the `className` attribute. It reflects the standard `class` attribute in HTML. Because of technical reasons, JSX had to replace a handful of internal HTML attributes. You can find all of the [supported HTML attributes in the React documentation](https://facebook.github.io/react/docs/dom-elements.html). They all follow the camelCase convention. On your way to learn React, you will come across some more JSX specific attributes.

### Exercises:

* define more variables and render them in your JSX
  * use a complex object to represent an user with a first name and last name
  * render the user properties in your JSX
* read more about [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* read more about [React components, elements and instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

> ## ES6 const and let
## ES6 const 和 let

> I guess you noticed that we declared the variable `helloWorld` with a `var` statement. JavaScript ES6 comes with two more options to declare your variables: `const` and `let`. In JavaScript ES6, you will rarely find `var` anymore.

我猜你已经注意到了，我们在前面的例子中使用的是关键字 `var` 来声明变量 `helloWorld` 的。JavaScript ES6中引入了另外两个声明变量的关键字：`const` 和 `let`。在JavaScript ES6中，你将会很少能看到 `var` 了。

> A variable declared with `const` cannot be re-assigned or re-declared. It cannot get mutated (changed, modified). You embrace immutable data structures by using it. Once the data structure is defined, you cannot change it.

被 `const` 声明的变量不能被重新赋值或重新声明。换句话说，它将不能再被改变。你可以使用它创建不可变数据结构，一旦数据结构被定义好，你就不能再改变它了。

{title="Code Playground",lang="javascript"}
~~~~~~~~
> // not allowed
// 这种写法是不可行的
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

> A variable declared with `let` can get mutated.

被关键字 `let` 声明的变量可以被改变。

{title="Code Playground",lang="javascript"}
~~~~~~~~
> // allowed
// 这种写法是可行的
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

> You would use it when you would need to re-assign a variable.

当一个变量需要被重新赋值的话，你应该使用 `let` 去声明它。

> However, you have to be careful with `const`. A variable declared with `const` cannot get modified. But when the variable is an array or object, the value it holds can get updated. The value it holds is not immutable.

然而，你必须小心地使用 `const` 。使用 `const` 声明的变量不能被改变，但是如果这个变量是数组或者对象的话，它里面持有的内容可以被更新。它里面持有的内容不是不可改变的。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

> But when to use each declaration? There are different opinions about the usage. I suggest using `const` whenever you can. It indicates that you want to keep your data structure immutable even though values in objects and arrays can get modified. If you want to modify your variable, you can use `let`.

但是，不同的声明方式应该在什么时候使用呢？有很多的选择。我的建议是在任何你可以使用 `const` 的时候使用它。这表示尽管对象和数组的内容是可以被修改的，你仍希望保持该数据结构不可变。而如果你想要改变你的变量，就使用 `let` 去声明它。

> Immutability is embraced in React and its ecosystem. That's why `const` should be your default choice when you define a variable. Still, in complex objects the values within can get modified. Be careful about this behavior.

React和它的生态是拥抱不可变的。这就是为什么 `const` 应该是你定义一个变量时的默认选择。当然，一个复杂的对象中的内容还是可能会被改变，请当心这种改变。

> In your application, you should use `const` over `var`.

在你的应用中，你应该用 `const` 来代替 `var`。

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

> ### Exercises:

### 练习:

> * read more about [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
> * read more about [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
> * research more about immutable data structures
>   * why do they make sense in programming in general
>   * why are they used in React and its ecosystem

* 阅读更多关于 [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) 的内容
* 阅读更多关于 [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) 的内容
* 研究更多关于不可变数据结构的内容
  * 在通常情况下，为什么他们是有意义的
  * 为什么他们会被React和它的生态使用

> ## ReactDOM
## ReactDOM

> Before you continue with the App component, you might want to see where it is used. It is located in your entry point to the React world: the *src/index.js* file.

在你学习这个 App 组件之前，你可能想知道它被用在了什么地方。它在你的React世界的入口文件 *src/index.js* 中

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~

>  Basically `ReactDOM.render()` uses a DOM node in your HTML to replace it with your JSX. That's how you can easily integrate React in every foreign application. It is not forbidden to use `ReactDOM.render()` multiple times across your application. You can use it at multiple places to bootstrap simple JSX syntax, a React component, multiple React components or a whole application. But in plain React application you will only use it once to bootstrap your whole component tree.

简单来说，`ReactDOM.render()` 会使用你的 JSX 来替换你的HTML中的一个 DOM 节点。这样你就可以很容易地把 React 集成到每一个其他的应用中。`ReactDOM.render()` 可以在你的应用中多次使用。你可以在多个地方用它来加载简单的 JSX 语法、单个 React 组件、多个 React 组件或者整个应用。但是在一个纯 React 的应用中，你只会使用它一次，来加载你的整个组件树。

>  `ReactDOM.render()` expects two arguments. The first argument is JSX that gets rendered. The second argument specifies the place where the React application hooks into your HTML. It expects an element with an `id='root'`. You can open your *public/index.html* file to find the id attribute.

`ReactDOM.render()` 有两个传入参数。第一个是准备渲染的 JSX。第二个参数指定了React应用在你的HTML中的放置的位置。这个位置是一个 `id='root'`的元素。你可以在文件 *public/index.html* 中找到这个id属性。

In the implementation `ReactDOM.render()` already takes your App component. However, it would be fine to pass simpler JSX as long as it is JSX. It doesn't have to be an instantiation of a component.

在实现中，`ReactDOM.render()` 总会很好地渲染你的 App 组件。你可以将一个简单的 JSX 直接用 JSX 的方式传入，而不是必须传入一个组件的实例。

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

> ### Exercises:
>

> * open the *public/index.html* to see where the React applications hooks into your HTML
> * read more about [rendering elements in React](https://facebook.github.io/react/docs/rendering-elements.html)
>

### 练习:

- 打开 *public/index.html* 文件并且找到 React 应用放置在你的HTML的位置
- 查看更多关于 [元素渲染](https://facebook.github.io/react/docs/rendering-elements.html) 的内容

> ## Hot Module Replacement
>

## 模块热替换



> There is one thing that you can do in the *src/index.js* file to improve your development experience as a developer. But it is optional and shouldn't overwhelm you in the beginning when learning React.

作为一个开发者，你可以在 *src/index.js* 中做一件事情来提高你的开发体验。但是这件事情是可选的，不要让它在你刚开始学习 React 的事情占用你的过多时间。

> In *create-react-app* it is already an advantage that the browser automatically refreshes the page when you change your source code. Try it by changing the `helloWorld` variable in your *src/App.js* file. The browser should refresh the page. But there is a better way of doing it.

用 *create-react-app* 创建的项目有一个优点，那就是浏览器可以在你更改源代码的时候自动刷新页面。试试改变 *src/App.js* 文件中的变量 `helloWorld` ，改变之后浏览器应该会刷新页面。但是有一个更好的方式实现这个功能。

> Hot Module Replacement (HMR) is a tool to reload your application in the browser. The browser doesn't perform a page refresh. You can easily activate it in *create-react-app*. In your *src/index.js*, your entry point to React, you have to add one little configuration.

模块热替换（HMR）是一个帮助你在浏览器中重新加载应用的工具，并且它不需要让浏览器刷新页面。你可以在 *create-react-app* 中很容易地开启这个工具：在你 React 的入口文件 *src/index.js* 中，添加一些配置代码。

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

# leanpub-start-insert
if (module.hot) {
  module.hot.accept();
}
# leanpub-end-insert
~~~~~~~~

> That's it. Try again to change the `helloWorld` variable in your *src/App.js* file. The browser shouldn't perform a page refresh, but the application reloads and shows the correct output. HMR comes with multiple advantages:

配置完成。接下来再尝试在你的 src/App.js 文件中更改一下变量 `helloWorld`，浏览器应该不会刷新页面，但是应用还是会重新加载并且显示正确的输出。HMR 带来很多的优点：

>  Imagine you are debugging your code with `console.log()` statements. These statements will stay in your developer console, even though you change your code, because the browser doesn't refresh the page anymore. That can be convenient for debugging purposes.

想象一下你正在使用 `console.log()` 调试你的代码。由于浏览器不再会刷新页面，所以即使你更改了你的代码，这些调试信息也会完整地保持在你的开发控制台中。这让调试变得很方便。

> In a growing application a page refresh delays your productivity. You have to wait until the page loads. A page reload can take several seconds in a large application. HMR takes away this disadvantage.

在一个不断开发的应用中，刷新页面将会降低你的生产效率：你必须得等待页面加载完毕。一个大的应用可能会花很多秒钟才能刷新完页面。使用HMR 可以避免这个缺点。

> The biggest benefit is that you can keep the application state with HMR. Imagine you have a dialog in your application with multiple steps and you are at step 3. Basically it is a wizard. Without HMR you would change the source code and your browser refreshes the page. You would have to open the dialog again and would have to navigate from step 1 to step 3. With HMR your dialog stays open at step 3. It keeps the application state even though the source code changes. The application itself reloads, but not the page.

使用 HMR 最大的好处是你可以保持应用的状态。想象一下你的应用中有一个包含很多步骤的对话框，而现在你正在第三步中，换句话说这就是一个向导。如果没有 HMR 的话，当你更改源代码的时候你的浏览器将会刷新页面，你就不得不再次打开这个对话框并且从步骤一开始导航到步骤三。而如果你使用 HMR 的话，你的对话框将会始终保持打开在步骤三的状态。尽管你的源代码改变了，但是应用的状态也会被保持。应用本身会被重新加载，而不是页面被重新加载。

> ### Exercises:
>
> * change your *src/App.js* source code a few times to see HMR in action
> * watch the first 10 minutes of [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) by Dan Abramov

### 练习：

- 改变几次你的 *src/App.js* 中的源代码，来观察 HMR 的工作方式
- 观看 Dan Abramov 的视频 [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) 的前十分钟

> ## Complex JavaScript in JSX

## JSX 中的复杂 Javascript

> Let's get back to your App component. So far you rendered some primitive variables in your JSX. Now you will start to render a list of items. The list will be sample data in the beginning, but later you will fetch the data from an external [API](https://www.robinwieruch.de/what-is-an-api-javascript/). That will be far more exciting.

让我们回到你的 App 组件中。到目前为止你在你的 JSX 中渲染了一些简单的变量。现在你可以开始渲染一个列表了。这个列表一开始可以是一些示例数据，但是以后你可以从一个外部 [API](https://www.robinwieruch.de/what-is-an-api-javascript/) 中获取数据。这会让人更加兴奋。

> First you have to define the list of items.

首先你需要定义一个列表。

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

> The sample data will reflect the data we will fetch later on from the API. An item in the list has a title, an url and an author. Additionally it comes with an identifier, points (which indicate how popular an article is) and a count of comments.

这个示例数据反应的是我们准备用 API 获取的数据。列表中的每一个成员都有标题、链接和作者信息。另外它还包含有标识符、分数（表示这个文章的流行程度）和评论的数量。

> Now you can use the built-in JavaScript `map` functionality in your JSX. It enables you to iterate over your list of items to display them. Again you will use curly braces to encapsulate the JavaScript expression in your JSX.

现在你可以在你的 JSX 中使用 JavaScript 内置的 `map` 函数。这个函数可以让你遍历你的列表来显示其中的成员。同样的，你需要用花括号把 JavaScript 包含在你的 JSX 中。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

> Using JavaScript in HTML is pretty powerful in JSX. Usually you might have used `map` to convert one list of items to another list of items. This time you use `map` to convert a list of items to HTML elements.

在 JSX 中使用 HTML 中的 JavaScript 是很强大的。通常情况下你可以用 `map` 来将一个列表转换成另一个列表。在这个例子中，你使用 `map` 函数将一个列表转换成一组 HTML 元素。

> So far, only the `title` will be displayed for each item. Let's display some more of the item properties.

到目前为止，每个成员只有 `title` 会被显示。让我们显示一些它们的其他属性。

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

> You can see how the map function is simply inlined in your JSX. Each item property is displayed in a `<span>` tag. Moreover the url property of the item is used in the `href` attribute of the anchor tag.

你可以看到 `map` 函数是如何简单地内联到你的 JSX 中的。每一个成员属性会被显示成一个 `<span>` 标签。此外，`url` 属性在另一个标签中被用作为 `href` 属性。

> React will do all the work for you and display each item. But you should add one helper for React to embrace its full potential and improve its performance. You have to assign a key attribute to each list element. That way React is able to identify added, changed and removed items when the list changes. The sample list items come with an identifier already.

React 会帮你完成所有的工作然后显示每一个成员。但是你应该在 React 中添加一个辅助属性来拥抱它的的潜能并提高它的性能。你需要给列表的每一个成员加上一个关键字（key）属性。这样的话 React 可以在列表发生变化的时候识别其中成员的添加、更改和删除的状态。这个示例数据中已经有一个标识符了。

{title="src/App.js",lang=javascript}
~~~~~~~~
{list.map(function(item) {
  return (
# leanpub-start-insert
    <div key={item.objectID}>
# leanpub-end-insert
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

> You should make sure that the key attribute is a stable identifier. Don't make the mistake of using index of the item in the array. The array index isn't stable at all. For instance, when the list changes its order, React will have a hard time identifying the items properly.

你应该确保这个关键字属性是一个稳定的标识符。不要错误地使用列表成员在数组的索引作为关键字。列表成员的索引是完全不稳定的。在下面的这个例子中，当列表的排序改变了之后，React 将很难正确地识别这些成员。

{title="src/App.js",lang=javascript}
~~~~~~~~
// don't do this
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

> You are displaying both list items now. You can start your app, open your browser and see both items of the list displayed.

你现在可以显示列表的所有成员了。你可以开启你的应用，打开浏览器然后查看这些显示出的列表成员。

> ### Exercises:
>
> * read more about [React lists and keys](https://facebook.github.io/react/docs/lists-and-keys.html)
> * recap the [standard built-in array functionalities in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
> * use more JavaScript expressions on your own in JSX
>

### 练习:

- 查看更过关于 [React 列表和关键字](https://facebook.github.io/react/docs/lists-and-keys.html) 的内容
- 简要重述 [JavaScript 中标准内建数组函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- 在 JSX 中使用更多的 JavaScript 表达式

> ## ES6 Arrow Functions

## ES6 箭头函数

> JavaScript ES6 introduced arrow functions. An arrow function expression is shorter than a function expression.

JavaScript ES6 引入了箭头函数。箭头函数表达式比普通的函数表达式更加简洁。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

> But you have to be aware of its functionalities. One of them is a different behavior with the `this` object. A function expression always defines its own `this` object. Arrow function expressions still have the `this` object of the enclosing context. Don't get confused when using `this` in an arrow function.

但是你需要注意它的功能。一个不同的地方是关于 `this` 对象的。一个普通的函数表达式总会定义它自己的 `this` 对象。但是箭头函数表达式仍然会使用包含它的语境下的 `this` 对象。不要被这种箭头函数的 `this` 对象困惑了。

> There is another valuable fact about arrow functions regarding the parenthesis. You can remove the parenthesis when the function gets only one argument, but have to keep them when it gets multiple arguments.

关于箭头函数的括号还有一个值得关注的点。如果函数只有一个参数，你就可以移除掉参数的括号，但是如果有多个参数，你就必须保留这个括号。

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, key => { ... }

// allowed
(item, key) => { ... }
~~~~~~~~

> However, let's have a look at the `map` function. You can write it more concisely with an ES6 arrow function.

不管怎样，让我们再看一下 `map` 函数。你可以用 ES6 的箭头函数更加简洁地把它写出来。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item => {
# leanpub-end-insert
  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

> Additionally, you can remove the *block body*, meaning the curly braces, of the ES6 arrow function. In a *concise body* an implicit return is attached. Thus you can remove the return statement. That will happen more often in the book, so be sure to understand the difference between a block body and a concise body when using arrow functions.

此外，在 ES6 的箭头函数中，你可以用简洁函数体来替换块状函数体（用花括号包含的内容），简洁函数体的返回不用显示声明。这样你就可以移除掉函数返回声明。在这本书中这种表达式将会被更多地使用，所以你要确保能够在使用箭头函数的时候要明白块状函数体和简洁函数体的区别。

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item =>
# leanpub-end-insert
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
# leanpub-start-insert
)}
# leanpub-end-insert
~~~~~~~~

> Your JSX looks more concise and readable now. It omits the function statement, the curly braces and the return statement. Instead a developer can focus on the implementation details.

现在你的 JSX 变得更加简洁和可读了。函数声明表达式、花括号和返回声明都被省略了。开发者就可以更加专注在实现细节上。

> ### Exercises:
>
> * read more about [ES6 arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
>

### 练习:

- 阅读更多关于 [ES6 箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 的内容

> ## ES6 Classes

## ES6 类

> JavaScript ES6 introduced classes. A class is commonly used in object-oriented programming languages. JavaScript was and is very flexible in its programming paradigms. You can do functional programming and object-oriented programming side by side for their particular use cases.

JavaScript ES6 引入了类的概念。类通常在面向对象编程语言中被使用。JavaScript 的编程范式在过去和现在都是非常灵活的。你可以根据使用情况一边使用函数式编程一边使用面向对象编程。

> Even though React embraces functional programming, for instance with immutable data structures, classes are used to declare components. They are called ES6 class components. React mixes the good parts of both programming paradigms.

尽管 React 为了例如不可变数据结构等的特性而拥抱函数式编程，但是它还是使用类来声明组件。这些组件被称为 ES6 类组件。React 混合使用了两种编程范式中的有益的部分。

> Let's consider the following Developer class to examine a JavaScript ES6 class without thinking about a component.

作为 JavaScript ES6 类的例子，让我们先不管组件，思考如下这个 Developer 类。

{title="Code Playground",lang="javascript"}
~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~

> A class has a constructor to make it instantiable. The constructor can take arguments to assign it to the class instance. Additionally a class can define functions. Because the function is associated with a class, it is called a method. Often it is referenced as a class method.

类都有一个用来实例化自己的构造函数。这个构造函数可以用来传入参数来赋给类的实例。此外，类可以定义函数。因为这个函数被关联给了类，所以它被称为方法。通常它被当称为类的方法。

> The Developer class is only the class declaration. You can create multiple instances of the class by invoking it. It is similar to the ES6 class component, that has a declaration, but you have to use it somewhere else to instantiate it.

这个 Developer 类只有类的声明。你可以使用它来创建多个类的示例。它和 ES6 类组件很类似，都有声明，但是你需要在别的地方实例化这个类。

> Let's see how you can instantiate the class and how you can use its methods.

让我们你可以怎样初始化类以及怎么使用它的方法。

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

> React uses JavaScript ES6 classes for ES6 class components. You already used one ES6 class component.

React 使用 JavaScript ES6 类来实现 ES6 类组件。你已经使用过一个 ES6 类组件了。

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

> The App class extends from `Component`. Basically you declare the App component, but it extends from another component. What does extend mean? In object-oriented programming you have the principle of inheritance. It is used to pass over functionalities from one class to another class.

这个 App 类继承自 `Component`。简单来说，你可以声明你的 App 组件，但是这个组件需要继承自另一个组件。继承是什么意思？在一个面向对象编程的语言中，你需要遵循继承原则。它可以把功能从一个类传递到另一个类。

> The App class extends functionality from the Component class. To be more specific, it inherits functionalities from the Component class. The Component class is used to extend a basic ES6 class to a ES6 component class. It has all the functionalities that a component in React needs to have. The render method is one of these functionalities that you have already used. You will learn about other component class methods later on.

这个 App 类就从 Component 类中继承了它的功能。这个 Component 类是从一个基本 ES6 类中继承来的 ES6 组件类。它有一个 React 组件所需要的所有功能。渲染（render）方法就是其中你可以使用的一个功能。之后你可以学到更多其他组件类的方法。

> The `Component` class encapsulates all the implementation details of a React component. It enables developers to use classes as components in React.

这个 `Component` 类封装了所有 React 类需要的实现细节。它使得开发者们可以在 React 中使用类来创建组件。

> The methods a React `Component` exposes is the public interface. One of these methods has to be overridden, the others don't need to be overridden. You will learn about the latter ones when the book arrives at lifecycle methods in a later chapter. The `render()` method has to be overridden, because it defines the output of a React `Component`. It has to be defined.

React `Component` 类暴露出来的方法都是公共的接口。这些方法中有一个方法必须被重写，其他的则不一定要被重写。你会在以后的讲述生命周期的章节中学到它们。这个 `render()` 方法是必须被重写的方法，因为它定义了一个 React 组件的输出。它必须被定义。

> Now you know the basics around JavaScript ES6 classes and how they are used in React to extend them to components. You will learn more about the Component methods when the book describes React lifecycle methods.

现在你已经知道了 JavaScript ES6 类的基本内容，以及它们是怎么在 React 中被继承为组件的。在本书描述 React 生命周期方法地方，你可以学到更多 Component 的方法。

> ### Exercises:
>
> * read more about [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)
>

### 练习:

- 阅读更多关于 [ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 的内容

{pagebreak}

> You have learned to bootstrap your own React application! Let's recap the last chapters:

你已经学会如何开始一个你自己的 React 应用了！让我们回顾一下这一章的内容：

> * React
>   * create-react-app bootstraps a React application
>   * JSX mixes up HTML and JavaScript to define the output of React components in their render methods
>   * components, instances and elements are different things in React
>   * `ReactDOM.render()` is an entry point for a React application to hook React into the DOM
>   * built-in JavaScript functionalities can be used in JSX
>     * map can be used to render a list of items as HTML elements

* React

  * 使用 create-react-app 创建一个 React 应用

  * JSX 混合使用了 HTML 和 JavaScript 在React 组件的方法中定义它的输出

  * React 中，组件、示例和元素是不同的概念

  * `ReactDOM.render()` 是 React 应用连接 DOM 的入口方法

  * JavaScript 内建函数可以在 JSX 中使用

    - map 可以被用来把列表成员渲染成 HTML 的元素


> * ES6
>   * variable declarations with `const` and `let` can be used for specific use cases
>     * use const over let in React applications
>   * arrow functions can be used to keep your functions concise
>   * classes are used to define components in React by extending them

ES6

- 根据不同的使用场景，选择用 `const` 和 `let` 来声明变量
  - 在 React 应用中尽量使用 const 来声明变量
- 箭头函数可以用来是你的函数变得更简洁
- 在 React 中，通过继承类的方式来声明组件

> It makes sense to take a break at this point. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far.

现在我们可以休息一下。巩固下这章的内容。你可以试验一下你现在所写的代码。

> You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.1).

你可以在[官方代码库](https://github.com/rwieruch/hackernews-client/tree/4.1)中找到源代码。
# Final Steps to Production
# 距离上线部署仅有的最后一步

The last chapters will show you how to deploy your application to production. You will use the free hosting service Heroku. On the way to deploy your application, you will learn more about *create-react-app*.

在最后的章节中我们会向你展示如何部署你的应用到产品环境。你可以使用Heroku来免费托管和部署应用，在学习如何部署React应用的同时也可以了解更多*create-react-app*的相关特性。

## Eject
## 弹出

The following step and knowledge is **not necessary** to deploy your application to production. Still, I want to explain it to you. *create-react-app* comes with one feature to keep it extendable but also to prevent a vendor lock-in. A vendor lock-in usually happens when you buy into a technology but there is no escape hatch of using it in the future. Fortunately in *create-react-app* you have such an escape hatch with "eject".

接下来的步骤和知识对于产品环境的部署来说*并不是必须*的。

In your *package.json* you will find the scripts to *start*, *test* and *build* your application. The last script is *eject*. You could try it, but there is no way back. **It is a one-way operation. Once you eject, you can't go back!** If you just started to learn React, it makes no sense to leave the convenient environment of *create-react-app*.

在你的*package.json*中你可以找到命令“start”、“test”、“build”去启动、测试、构建应用。在最后一个命令是*eject*。你可以试着去执行它，但是这个命令只能被执行一次并且不能撤回。*这是一个破坏性的命令，一旦执行就不能反悔*，如果你只是学习React，使用了这个命令将会丢失*create-react-app*提供的那些便利功能。

If you would run `npm run eject`, the command would copy all the configuration and dependencies to your *package.json* and a new *config/* folder. You would convert the whole project into a custom setup with tooling that includes Babel and Webpack. After all, you would have full control over all these tools.

如果你运行 `npm run eject`，这个命令会复制所有的配置文件和依赖到你的*package.json*并且创建一个*config*文件夹来存放配置文件。这样你就可以通过修改Bable和Webpack配置来自定义整个项目，毕竟现在你拿回了整个项目的控制权。

The official documentation says that *create-react-app* is suitable for small to mid size projects. You shouldn't feel obligated to use the "eject" command.

官方推荐*create-react-app*适合给小、中型项目，所以你不用感到愧疚运行“eject”命令来移除掉*create-react-app*并拿回控制权。

### Exercises:
### 练习

* read more about [eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)
* 阅读更多关于“eject” [eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)


## Deploy your App
## 部署你的APP

In the end, no application should stay on localhost. You want to go live. Heroku is a platform as a service where you can host your application. They offer a seamless integration with React. To be more specific: It's possible to deploy a *create-react-app* in minutes. It is a zero-configuration deployment which follows the philosophy of *create-react-app*.

最后，没有应用只是满足于放到本地环境。如果你想发布你的成果到线上，Heroku是一个PASS服务，可以托管你的应用。他们
提供了和React的无缝集成，甚至可能在几分钟内部署一个用*create-react-app*创建的应用。只要遵守*create-react-app*设计原则就可以实现零配置的部署。

You need to fulfill two requirements before you can deploy your application to Heroku:

* install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line)
* create a [free Heroku account](https://www.heroku.com/)

在开始部署你的应用到Heroku之前你需要先做两件事：

* 安装Heroku命令行工具 [Heroku CLI](https://devcenter.heroku.com/articles/heroku-command-line)
* 创建一个Heroku账号 [free Heroku account](https://www.heroku.com/)

If you have installed Homebrew, you can install the Heroku CLI from command line:

如果你在你的Mac上装了Homebrew，你可以像下面这样操作

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Now you can use git and Heroku CLI to deploy your application.
现在你可以使用git和Heroku命令行来部署你的应用

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

That's it. I hope your application is up and running now. If you run into problems you can check the following resources:
搞定。我希望你的应用在线上运行一切正常。如果你遇到什么问题，这里有一些资源可以参考。

* [Git and GitHub Essentials](https://www.robinwieruch.de/git-essential-commands/)
* [Deploying React with Zero Configuration](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack for create-react-app](https://github.com/mars/create-react-app-buildpack)

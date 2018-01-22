# 前言


《React 学习之道》会教您一些React的基础知识。通过这套教程，您可以用纯React构建一个真正可用的应用程序，而不需要去理会其他复杂的工具。我将为您逐一介绍从开发环境的准备到部署上线的全部过程。本书每一章都包含一些额外的索引资料以及课后练习。在读完本书之后，您将会有能力依靠自己构建一个React应用。我，Robin Wieruch，以及整个社区会持续维护和更新这些资料。


通过《React 学习之道》，在开始陷入到更庞大的React生态圈之前，我想为您奠定一个良好的基础。它会通过一个真实可用的React应用来解释基本概念、设计模式以及最佳实践。


您将会学习构建您自己的React应用。这个应用会涉及一些真正可用的功能，比如分页，客户端缓存，以及像搜索和排序这样的交互功能。另外在这个过程中，您会慢慢从JavaScript ES5过渡到JavaScript ES6。我希望这本书能充分体现我对React和JavaScript的热情所在，并让您能够开始您的开发旅程。

{pagebreak}


# 读者赠言


**[Muhammad Kashif](https://twitter.com/appsdevpk/status/848625244956901376):** “《React 学习之道》是一本独一无二的书，我推荐给任何想要学习React基础和进阶技巧的学生或者专业人士。她包含了诸多启发性的小提示和绝无仅有的技术点。书中虽然引用了大量例子和参考资料，但最后都被用到我们要解决的问题上，这体现了编写本书令人惊叹地缜密。我有17年的互联网和桌面开发经验，阅读本书之前，我在学习React的过程中缺并不顺利。而这本书就像魔术一样有用。”


**[Andre Vargas](https://twitter.com/andrevar66/status/853789166987038720):** “Robin Wieruch的《React 学习之道》是一本非常牛的书！我所学到的绝大部分有关React甚至是ES6的知识都是通过她得来的！”


**[Nicholas Hunt-Walker, Instructor of Python at a Seattle Coding School](https://twitter.com/nhuntwalker/status/845730837823840256):** “这是一本我读过的最严谨和最实用的编程书籍之一。一本完整的React和ES6使用说明。”


**[Austin Green](https://twitter.com/AustinGreen/status/845321540627521536):** “非常感谢，真的很喜欢这本书。完美的学习曲线，不管是React，ES6，还是抽象编程概念。”


**[Nicole Ferguson](https://twitter.com/nicoleffe/status/833488391148822528):** “这个周末跟着Robin的课程学习React，我发现这一切太有意思了。这几乎让我感到羞愧。”


**[Karan](https://twitter.com/kvss1992/status/889197346344493056):** “刚刚完成这个课程。这是全世界最好的学习React和JS的一本书。完美展现了ES的优雅。膜拜！ :)”


**[Eric Priou](https://twitter.com/erixtekila/status/840875459730657283):** “Robin的《React 学习之道》是必读的一本书。简明扼要地介绍了React和Javascript。”


**一个新手开发:** “作为一个开发新手，我刚刚完成了这本书的学习，非常感谢写了这本书。她非常容易上手，我相信自己在接下来的几天可以开始从头开发一个新应用。这本书比我之前试过的官方React入门文档好很多（由于缺乏细节，我并未能够完成）。每个章节后面的练习题对我有很好的激励效果。”


**一个学生:** “这是最好的学习React的一本书。我们可以一边做练习项目，一边学习知识点，然后还能紧扣我们的学习主题。我发现「边码边学」是最好的掌握编程的方法，而这本书完完全全是这样教我的。”


**[Thomas Lockney](https://www.goodreads.com/review/show/1880673388):** “这是一本非常扎实的介绍React的书，而不是试着把事情搞复杂。我本来只想尝试理解看看这本书到底讲了什么，然后我得出了上面的结论。我并没有跟着所有的脚注来学习我还没有注意到的新的ES6语法（我当然不会说我一直没有注意到，Bob）（译者注：这个是在博客中与另外一个朋友互动的话）。对于那些没有及时了解到这些新功能，并且很勤奋的跟着练习的朋友们，我想很肯定地对你们说，你们能学到的会不仅仅是这本书所教的东西。”

{pagebreak}


# 儿童教育


这本书应该能让任何人都可以学习React。但是，由于这本书一开始是用英文写的，并不是所有人都有能力使用这些资源。所以我想用这个项目来支持那些发展中国家教孩子们英语的项目。


* 2017/4/11 - 2017/04/18，[学习React的回馈](https://www.robinwieruch.de/giving-back-by-learning-react/) 

{pagebreak}

# 问题解答


**我怎么获取最新的版本？** 你可以[订阅](https://www.getrevue.co/profile/rwieruch)邮件通知或者在 [Twitter](https://twitter.com/rwieruch) 上关注我来获取更新。一旦你拿到这本书的副本,她会自己保持更新。只是如果有更新，必须下载新的副本。 


**这本书用的是最新版本的React吗？** 这本书总是会随着React版本更新而更新。通常情况下书籍在发行后很快就过时了。因为这本书是自出版的，所以我可以随时更新它。


**这本书包括Redux的内容吗？** 不包括。不过我写了第二本书。《React 学习之道》会给你奠定一个坚实的基础，这样你可以继续之后的高阶内容。同样一个应用，在本书中，示例应用程序的实现将会向你证明，不需要Redux也可以搭建一个完整的React应用程序。在读完这本书之后，你应该有能力自己搭建一个不用Redux的应用。然后你可以读我的第二本书来学习[Redux](https://roadtoreact.com/course-details?courseId=TAMING_THE_STATE)。


**这本书会用到JavaScript ES6吗？** 是的。但是别担心。如果你熟悉ES5就可以了。所有的JavaScript ES6语法，在我们学习React的过程中，都逐步会从ES5转换成ES6。每个语法都会有详细的解释。这本书不只是用来学习React，同时也包含所有相关的ES6语法。


**以后你会添加新的章节吗？** 你可以从更新日志章节找到已经发生的重大更新。当然也会有一些较小的改动不会被公示出来。总体来说，这取决于社区是否让我继续写这本书。如果大家很喜欢这本书，我会继续写更多的章节以及改进已有的内容。我会保持书中的内容包含最新的最佳实践，概念和设计模式。


**我在读这本书的时候遇到困难，怎么获取帮助？** 这本书有一个 [Slack 聊天组](https://slack-the-road-to-learn-react.wieruch.com/)给读者。你可以加入这个频道获取帮助，或者帮助其他人。不管怎样，帮助其他人也有助于你自己的学习。


**如果我在书中发现一些问题，有地方可以帮助解决吗？** 如果你发现问题，请加入Slack聊天组。另外，你可以看看本书[Github上的问题](https://github.com/rwieruch/the-road-to-learn-react/issues)。有可能你的问题已经有人遇到了，并且解决方案也在上面。如果找不到同样的问题，请新建一个新的问题来解释你遇到的麻烦，比如提供一个截图，和一些细节信息（比如页数、Nodejs的版本等）。之后，我会修复所有问题，然后发布一个新的版本。


**我能提供帮助来改进这本书吗？** 是的。你可以在[在GitHub上贡献](https://github.com/rwieruch/the-road-to-learn-react)直接提出你的想法和代码。我并不宣称自己是一个专家，也不是用英语母语进行写作。我会非常感谢您的帮助。


**我可以为这个项目做出其他支持吗？** 是的。欢迎联系我。我贡献了非常多的个人时间到开源教程和学习资源的制作。你可以看看[我的介绍](https://www.robinwieruch.de/about/)。 我非常高兴能得到您在[Patreon](https://www.patreon.com/rwieruch)的赞助。


**写这本书的有什么特别的出发点吗？** 是的。我想让你花点时间想一想谁比较适合学习React。这个人可能已经表现出一些兴趣，可能已经学习过一段时间，或者有可能并没有发现自己其实可以学习React。你可以找到他们然后把这本书分享给他。这对我真的非常重要，这本书就旨与他人分享。

{pagebreak}

# 更新日志

**10. January 2017:**

* [v2 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/18)
* even more beginner friendly
* 37% more content
* 30% improved content
* 13 improved and new chapters
* 140 pages of learning material
* [+ interactive course of the book on educative.io](https://www.educative.io/collection/5740745361195008/5676830073815040)

**08. March 2017:**

* [v3 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/34)
* 20% more content
* 25% improved content
* 9 new chapters
* 170 pages of learning material

**15. April 2017:**

* upgrade to React 15.5

**5. July 2017:**

* upgrade to node 8.1.3
* upgrade to npm 5.0.4
* upgrade to create-react-app 1.3.3

**17. October 2017:**

* upgrade to node 8.3.0
* upgrade to npm 5.5.1
* upgrade to create-react-app 1.4.1
* upgrade to React 16
* [v4 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/72)
* 15% more content
* 15% improved content
* 3 new chapters (Bindings, Event Handlers, Error Handling)
* 190+ pages of learning material
* [+9 Source Code Projects](https://roadtoreact.com/course-details?courseId=THE_ROAD_TO_LEARN_REACT)

{pagebreak}

# 怎么读这本书？


我希望这本书在你打算写一个应用的时候，能够教会你React。这是一个实践性很强的React学习指南，而不是一个React资料索引。你会在这本书的指引下写一个Hacker News（译者注：一个著名黑客论坛）应用，会需要和真实API交互。她包括了很多有趣的话题，包括React状态管理，缓存和交互效果（排序和搜索）。在这个过程中你可以学到React的最佳实践和设计模式。


另外，本书可以让你平滑地从Javascript ES5过渡到JavaScript ES6。React采用了非常多的JavaScript ES6语法，我希望告诉你怎么使用它们。


一般来说，本书每一章都会在前一章基础上继续搭建。每一章都会教你一些新的知识点。务必不要跳过任何内容。你应该实践和领会每一个步骤。你可以自己写出实现，然后阅读书中的内容。每一章之后，我提供了一些阅读资料和练习题。如果你真的想学好React，我非常推荐读完这些阅读资料并上手完成练习题。完成一章的学习之后，请保证自己对所学内容熟练掌握之后，再继续后面的章节。


最终，你会拥有一个完整的可以部署上线的应用。我非常渴望能看到你的杰作，如果你完成了这本书，请给我发消息。最后一章会给你很多选择来继续学习和应用 React。总之你会发现在[我的个人网站](https://www.robinwieruch.de/)有非常多的React的相关话题。


既然你在看这本书，我猜你还是个React新手。那真是太好了。最后我希望你能给我你的反馈，来帮助我完善学习资料，以帮助人人都可以掌握React。你可以直接提交代码到[GitHub](https://github.com/rwieruch/the-road-to-learn-react)，或者在[Twitter](https://twitter.com/rwieruch)给我发消息。

# 你可以期望学到什么（目前为止...）

* [Hacker News的React版本](https://intense-refuge-78753.herokuapp.com/)
* 没有复杂的配置
* 用create-react-app来初始化你的应用
* 高效而轻量级的代码
* 只用React setState来做状态管理（目前为止...）
* 从JavaScript ES5一路平滑过渡到ES6
* React setState和生命周期函数的用法
* 和真实API的交互（Hacker News）
* 高级用户交互
  * 客户端排序
  * 客户端过滤
  * 服务器端搜索
* 客户端缓存的实现
* 高阶函数和高阶组件
* 用Jest进行组件的切片(snapshot)测试
* 用Enzyme进行组件的单元测试
* 过程中学到一些有用的工具库
* 过程中的练习题和扩展阅读
* 认同和巩固你的所学
* 将你的应用部署到产品环境

{pagebreak}

---
title: 面试大招之系统设计
date: 2019-07-29 15:57:56
tags: [面试,招聘,系统设计]
author: xizhibei
issue_link: https://github.com/xizhibei/blog/issues/116
---
<!-- en_title: system-design-as-interview -->

在我们团队经常进行的面试中，往往会用关于算法、数据库与架构设计之类的问题来考察候选人的能力。不过我一般不会直接问，而是将这些问题安排在系统设计里面，或者也可以叫做情景题。

<!-- more -->

### 面试过程

常见的过程就是，挑选一个常见的系统，去掉那些繁杂无关的小功能，只留下最核心的业务逻辑，然后让候选人设计这个系统。步骤一般按数据库、业务逻辑、接口的顺序让候选人设计，这里自底向上的顺序不是很重要，也有人习惯自顶向下的，候选人能够按你的要求完成整体设计即可。

对于系统的挑选，比如新闻、购物车、论坛、排行榜之类的小型系统（如果候选人简历中有说明做过类似的系统设计，就可以在他设计过的系统的基础上进行进一步的设计），然后让候选人设计这个系统。一开始会给一些条件限制，比如这个系统里面的用户角色只有两种，没有评论、没有登录、没有个人页面等，设计的时候只需要核心功能，毕竟我们面试的时间有限。

然后，要求他开始设计，我们面试过的大部分候选人就会自然地从数据库结构开始入手，数据库一旦设计完成，我就会简单问问这么设计的原因，然后选什么数据库。这里就是考察候选人数据库经验的地方了，用过什么数据库，用到什么程度都会在这个过程中有体现。

设计完数据库，就会让他接着设计功能的实现，需要注意的点在于这些功能的业务逻辑是怎样的，能不能达到我们给他定的要求。这里重点考察的是软件架构、算法以及对于业务的理解能力，同时这里面还可以考一些细节：文档如何做，预计要有多少机器，放在云服务上的成本是多少，是否有成本更低的方案，与客户端如何配合等。

最后，就是接口的设计了，可以与功能一起设计，但主要是知识点了，比如你要求他设计 RESTFul、RPC 或者 GraphQL，也可以让他自己去选，这些内容就偏知识点了，其实可以与功能设计合在一起。

最后时间够还可以加题目，比如系统的维护，目前越来越多的公司都让开发工程师去负责维护线上的应用了，因此也可以一同考察：问他实现的功能如何部署、监控，会需要用到什么架构组件，这些组价需要如何维护。

这些设计的过程中，依据时间与候选人的能力，会重复一到三次，不断对他设计的系统进行挑战：

1.  挑问题：如果遇到大用户量，高并发的请求的情况下，哪些模块会出现问题？比如涉及到多个表 join 的接口；
2.  加功能：在这个系统中，如果要继续开发新功能该如何设计？比如自有商城的购物车需要支持第三方商家；
3.  改功能：现在，产品经理说功能做的不对（顺便黑一把产品），你需要将系统中的某个模块改掉，该如何设计？比如原有功能是给某个机构设计的，现在不是了，要做成 SaaS 系统，需要支持多个机构；

这个过程中，**最重要的还是候选人的沟通能力与思路**，良好的情况下是候选人边设计边与我们沟通，比如对于限制的条件有问题，有个细节不够明确。而候选人一旦沉默时间过长，就需要给他提示了，让他说下当前的思路，**用问问题的方式提示他，这点非常重要**，因为候选人肯定有弱点，回想平时我们自己设计系统的时候，也会需要经历与同事沟通，查资料的过程。

我们也不妨把他当做现在的同事，与他一起协作完成这个系统的设计，一个是展示你们团队的工作方式，另一个也是给后面是否招聘他作参考：**你是否愿意跟这样的同事一起工作？**。

其它的面试内容，也可以回顾下我之前写过的几篇文章：

1.  [那些你可能在面试时会忽略的事](https://github.com/xizhibei/blog/issues/73)
2.  [谈谈如何当一名合适的面试官](https://github.com/xizhibei/blog/issues/36)
3.  [谈谈人才招聘](https://github.com/xizhibei/blog/issues/63)

下面补充两个细节内容，也是在招聘过程中，与多个候选人沟通的时候互相探讨出来的。

### 业务逻辑的实现途径：TDD

接着上面的业务逻辑的内容继续说，我们在实现相关的功能的时候，做的比较好的方式，可以考虑 TDD (Test Driven Development) 即测试驱动开发。

具体流程就是，一旦设计完数据库，可以将业务逻辑放在一边，专注于先将接口打通，即先设计接口、写接口文档，对于未实现的接口统一返回没有内容的 HTTP status 200（其实更好的做法是返回 Mock 的数据），这样也方便客户端的同事同时开发，然后先编写相关的业务逻辑测试代码，最后才去实现业务逻辑。

Bob 大叔（Robert Cecil Martin）在《代码整洁之道》中说过三个 TDD 法则：

> 1.  You can’t write any production code until you have first written a failing unit test：在写第一个单元测试之前，你不能写任何的生产代码；
> 2.  You can’t write more of a unit test than is sufficient to fail, and not compiling is failing：只可编写刚好无法通过的单元测试，不能编译也算不通过；
> 3.  You can’t write more production code than is sufficient to pass the currently failing unit test：只可编写刚好足以通过当前失败测试的生产代码；

这三个法则基本上可以指导我们该如何用 TDD 的方式去实现业务逻辑。

如果有候选人对 TDD 很推崇并能说出如何实践的，可以考虑作为一个加分项。

### 数据库设计的问题

问：ID 是否应该有意义？
答：不应该，想象下你用文件名去作为文档的 ID，那么之后文档将不能修改文件名，以及假如有重复的文件名该如何处理？软件架构设计原则告诉我们，模块间不能依赖于实现，而是应该依赖于抽象，我们的实现可以随便更改，但是抽象却几乎不会变。因此，ID 一旦在系统有了现实含义，那便是留下了一个大坑，你不知道产品之后会有什么改动，到时候重构数据库的底层就会充满风险了。

问：时间该用什么储存？
答：尽量采用数据库提供的时间格式，不要用数值来存储，主要原因是会带来**调试问题**，数据是给人看的，如果数据库返回的时间数据都是数字，会给开发人员带来查看与调试成本，增加开发难度。就如 Unix 的设计哲学来说，用文本而不是二进制来实现通信协议的意义也在于此。在如今这个计算与存储成本越来越低的时代，开发人员的时间才是最宝贵的，对开发人员友好的格式才会更有价值。

### P.S.

最后，无论你是候选人还是面试官，[The System Design Primer](https://github.com/donnemartin/system-design-primer) ，我都推荐你看。


***
首发于 Github issues: https://github.com/xizhibei/blog/issues/116 ，欢迎 Star 以及 Watch

{% post_link footer %}
***
# 前端性能优化の备忘录（2018版）

![Performance](https://img.w3ctech.com/p.png)

![smashingmagazine](https://img.w3ctech.com/character-15.png)

性能（优化）的重要性，想必大家心里都有点 B 数，对吧。不过我想问的是，大家是否真的知道啥是性能瓶颈呢？JS 文件的体积过大（expensive JavaScript），web 字体的下载速度过慢（slow web font delivery），图片的尺寸过大（heavy images），渲染的速度过慢（sluggish rendering）等因素是不是诱发性能瓶颈的原因呢？（性能优化）是否值得你去实践 [tree-shaking](https://doc.webpack-china.org/guides/tree-shaking/)、[scope hoisting](https://zhuanlan.zhihu.com/p/27980441)、[code-splitting](https://doc.webpack-china.org/guides/code-splitting/) 以及其它的载入技巧（包括 [intersection observer](https://www.w3.org/TR/intersection-observer/)、[clients hints](https://github.com/igrigorik/http-client-hints)、[CSS containment](https://www.w3.org/TR/css-contain-1/)、[HTTP/2](https://http2.github.io/)、[service workers](https://www.w3.org/TR/service-workers-1/)）呢？（抛开上面提到的一些问题，我觉得）最为重要的一点，**大家是否知道性能优化从何开始**以及知道如何树立长期的性能优化意识？（And, most importantly, where do we even start improving performance and how do we establish a performance culture long-term?）。

在以前（Back in the day），大家对性能优化总是摆出一副“秋后算账”（a mere afterthought）的样子。（举个栗子），就算它的工作内容只是对资源进行压缩、合并、优化或者对服务器配置文件（的配置项）做一些微调，（万万没想到的是），大家还是习惯性地把（针对性能优化的）排期拖到项目末尾。现在我们再回过头看，性能优化似乎变得更加重要啦。

对性能进行优化，不仅仅只是出于技术层面的忧虑（concern）：真的是因为性能优化非常重要，尤其是在（我们急需）工作流引入性能优化环节且需要根据性能优化建议，来提示（我们）进行决策的情况下，性能优化就会显得尤为必要。**另外性能必须是可以不断被量化、被监控以及被优化的**。现在随着 web 应用的复杂性日益增加，（自然而然）就会给性能指标分析（keep track of）带来新的挑战，为啥呢？这是因为性能指标之间的差异性非常大。不过这也取决于使用的设备、浏览器、协议、网络类型以及其它能够对性能产生影响的潜在因素，包括 CDN、ISP、缓存（cache）、代理（proxy）、防火墙（firewall）、负载均衡（load balancer）以及服务器（server）。

所以，在（网站）项目刚启动时，我们就应该（对网站）采取（一些有助于网站）性能提升的手段，并且该过程一直要持续到网站的最终版本发布上线。在这段时间里，假设我们需要牢记这些优化手段，并且希望针对这些优化手段做一下总结，（内心 OS ：天啦噜，这总结应该长啥样呀）？往下阅读，你就会发现一篇非常客观的年度总结：前端性能优化の备忘录2018版（简单来说，这是一篇能够确保让你解决类似如何让网站响应更加迅速、访问更加流畅等前端性能优化问题的年度好文）。

（当然，你也可以只[下载前端性能优化の备忘录PDF版本](https://www.dropbox.com/s/8h9lo8ee65oo9y1/front-end-performance-checklist-2018.pdf?dl=0)（0.129 MB）或者[去 Apple Pages 下载前端性能优化の备忘录](https://www.dropbox.com/s/yjedzbyj32gzd9g/performance-checklist-1.1.pages?dl=0)（0.236MB）。让我们一起愉快的优化前端性能吧！）

## 准备好了的话，那就设立两个小目标吧（Getting Ready: Planning And Metrics）

虽说 Micro-optimization 可以让你的性能优化手段起作用，不过（我觉得大家）还是应该在思想层面上设立一个明确的目标，这是因为目标一旦被量化，你在项目期间作出的所有决定都会受到影响，所以说设立明确的目标才至关重要。（至于如何解决前端性能问题），这里有很多不同的优化模型（model），不过我们下面将要讨论的（优化模型）内容可能十分武断，所以（在使用上述优化模型时），务必确定性能优化手段的优先级。

### 树立性能优化意识（Establish a performance culture）

对于很多团队来说，他们的前端工程师不但知道哪些问题是潜在的性能问题，而且还知道应该使用哪种载入技巧来解决。不过（这并没什么卵用，为啥这样说呢？这是因为），（在性能优化的这件事上），只要开发或者设计那边有一方跟市场部同学意见不合，那么性能优化将会变成一句口号（performance isn't going to sustain long-term）。所以说，分析那些（常见的）用户反馈并且去思考如何提升性能，还是会在（一定程度上）帮你缓解一些常见的性能问题。

现在不管你用的是手机还是台式机，这两种设备都是可以跑性能测试的，而且（性能测试工具）还能够针对这两种设备定量的给出测试结果（measure outcomes）。在存在真实数据的情况下，这些（指的是测试结果）都将会有助于你建立起以公司为样本数据的研究案例（case study）。另外，利用好 [WPO Stats](https://wpostats.com/) 上的数据，包括实验得出的数据以及案例给出的数据。（为啥这样说呢？这是因为）这些数据能够在（一定程度上）提升你对业务的熟悉程度（sensitivity），这其中也包括了帮助你去理解性能（为啥如此）重要的背后原因以及性能对用户体验、业务指标的影响。光嘴上说性能优化很重要，这是远远不够的。还是需要你去制定一些易量化（measurable）、易追踪（trackable）的目标，并且一直执行下去。

### 目标：网站总是要快人一步（Goal: Be at least 20% faster than your fastest competitor）
[心理学研究](https://www.smashingmagazine.com/2015/09/why-performance-matters-the-perception-of-time/#the-need-for-performance-optimization-the-20-rule)表明，如果你想让用户感觉你网站（的载入速度）就是比其它网站快，那么你网站（的载入速度）至少要比其它网站快20%才行。另外，研究主要的竞争对手，比如收集他们在手机以及台式机上的一些表现以及他们设置的一些阈值，（在一定程度上）能够帮你超越竞争对手。为了让上述结果更加精确，（你需要做的事是），首先去查看分析报告，得出一些用户的用户行为。对啦，如果你的分析报告只是用于测试，那么你也可以只模拟出90%的场景。然后去收集数据，并将收集的数据做成[表格（spreadsheet）](http://v3.danielmall.com/articles/how-to-make-a-performance-budget/)，（并且根据自身的情况），剔除部分（20%）数据，这样你就可以设立自己的小目标啦，例如完成一篇关于[性能开销（performance budgets）](http://bradfrost.com/blog/post/performance-budget-builder/)的分析报告。现在，你就可以开启你的性能测试之旅啦。

假设你记住了（测试过程中所花费的）性能开销，然后想通过尝试脚本最小化的方法来让交互时间的数据更好看，接下来你会发现你已经踏上了性能优化的“不归路”(a reasonable path)。另外我想说的是，Lara Hogan 写的那篇关于[如何处理设计与性能开销之间的关系（guide on how to approach designs with a performance budget）](http://designingforperformance.com/weighing-aesthetics-and-performance/#approach-new-designs-with-a-performance-budget) 的博文，给了设计师很多非常有用的建议，顺便安利 [Performance Budget Calculator](http://www.performancebudget.io/)、[Browser Calories](https://browserdiet.com/calories/) 这两个工具，（为啥呢？这是因为）上面的两个工具都能够对静态资源的性能开销进行统计（感谢 [Karolina Szczur](https://medium.com/@fox/talk-the-state-of-the-web-3e12f8e413b3) 的提醒）。

![Performance budget builder by Brad Frost](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/231e97c1-4bfa-4dff-85a7-93e0a16b2690/performance-budget-lbp9l7-c-scalew-862-opt.png)

除了性能开销这个因素要考虑之外，还需要去思考用户提的一些需求（critical customer tasks），为啥呢？这是因为，这些需求对业务来说，最为有利。对啦，**针对一些关键性的操作**，你还要去设置并且讨论出合理的**时间阈值**，还有就是，（针对页面啥时候处于）“UX ready 状态”，可以在你统计用户时间（user timing）的时候，增加一些大家都认可的标识。另外在很多情况下，用户的一些操作行为会涉及多个部门。所以，就（用户操作所带来的）时间开销是否合理（acceptable timings）这个问题来说，各个部门如果能抱成一团的话，那么将会有助于避免一些问题的发生，比如，在将来的某个时候，各个部门为了性能问题来“撕逼”（discussions）。最后就是，在你引入资源以及新增功能的时候，确保上述操作所带来的开销，是可以看得见摸得着的。然后就是，确保大家都能够知道这些开销是怎么来的（Make sure that additional costs of added resources and features are visible and understood）。

而且，正如 Patrick Meenan 所建议的那样，在设计（资源载入环节）的过程中，**知道如何规划好（plan out）资源之间的载入顺序以及知道这些顺序之间的权衡利弊（trade-offs），真的是很有必要**。（为啥呢？试想一下），假设在项目开始之前，你就针对一些比较重要的环节（part）来排优先级，并且还确定了这些环节的出场顺序。（那么，自然而然），你也就知道有哪些环节可以被 delay 啦。顺利的话（Ideally），也是能够从这些环节的出场顺序看出（reflect ） CSS 和 JavaScript 的引入顺序。如果真是酱紫的话，在今后的项目构建过程中，CSS 以及 JavaScript 的处理，将会变得容易多啦。当然，你也可以去考虑究竟啥样的视觉体验才是介于好与坏之间的“中间状态”，前提是，你的页面正处于 loaded 状态（比如，页面的 web 字体还未载入）。

规划、规划、规划，重要的事情说三遍！为啥呢？这是因为，对性能提前做好规划，可以让你（在真的遇到性能问题的时候）做到“手到擒来”（low-hanging-fruits），并且最终还可能会演变成快速制胜的法宝（strategy）。不过话说回来，在没有做好规划、（公司层面的）优化目标被设置的不合理的背景下，还能够把性能的优先级摆在第一位，其实是很难的。
### 选择正确的指标（Choose the right metrics）

[不是所有的指标都同样重要](https://speedcurve.com/blog/rendering-metrics/)。研究最影响应用的指标：通常，这与开始渲染最重要部分的速度（以及它们是什么）和提供的输入响应速度有关。这些将会给出最佳的优化目标。通过各种方式，而不是只关注于整个页面的加载时间（例如 onLoad 和 DOMContentLoaded 时间），优先加载用户能感知到的页面变化。这意味着要关注[一些不同的指标](https://docs.google.com/presentation/d/1D4foHkE0VQdhcA5_hiesl8JhEGeTDRrQR4gipfJ8z7Y/present?slide=id.g21f3ab9dd6_0_33)，事实上，选择正确的指标并不容易。

下面是一些值得考虑的指标：

- 第一次有效的渲染（ FMP ，当主要内容出现在页面上）

- [重要内容渲染时间](https://speedcurve.com/blog/web-performance-monitoring-hero-times/)（当页面的重要内容完成渲染时）

- 交互时间（ TTI 页面布局稳定时，关键字体是可见的，主线程足以处理用户输入 - 基本上是用户可以点击 UI 并与之交互的时间点）

- 输入响应（接口响应用户操作需要的时间）

- 感知速度指标（测量页面可视化内容的速度有多快；分数越低越好）

- [自定义指标](https://speedcurve.com/blog/user-timing-and-custom-metrics/)，由业务需求和用户体验来定。

[Steve Souders 对每个指标都有详细的解释](https://speedcurve.com/blog/rendering-metrics/)。 在很多情况下，TTI 和输入响应是最重要的，这取决于应用程序的上下文，有些度量可能会不同：例如，对于 Netflix 电视用户界面，[键盘输入响应，内存使用和 TTI](https://medium.com/netflix-techblog/crafting-a-high-performance-tv-user-interface-using-react-3350e5a6ad3b) 更为重要。

[![The difference between First Paint, First Contentful Paint, First Meaningful Paint, Visual Complete and Time To Interactive. Large view. Credit: @denar](https://i.vimeocdn.com/video/675362019.png?mw=1500&mh=840&q=70)](https://vimeo.com/249524245)

### 收集用户设备上的数据（Gather data on a device representative of your audience）

为了收集准确的数据，需要选择不同设备来测试。Moto G4，一个中档的三星设备和一个像 Nexus 5X 这样的中端设备会是不错的选择。如果身边没有设备，可以在受限的 CPU（降低5倍）下的节流网络（例如，150ms RTT，1.5 Mbps 以下，0.7 Mbps 以上）来进行测试，模拟电脑桌面的体验。最后切换到常规的 3G ，4G 和 Wi-Fi 网络。为了使性能影响更加明显，甚至可以引入 [2G](https://www.theverge.com/2015/10/28/9625062/facebook-2g-tuesdays-slow-internet-developing-world) 或设置 [3G 节流网络](https://twitter.com/thommaskelly/status/938127039403610112) ，以便进行更快的测试。

![Introducing the slowest day of the week. Facebook has introduced 2G Tuesdays to increase visibility and sensitivity of slow connections. (Image source)](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_800/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/dfe1a4ec-2088-4e39-8a39-9f2010380a53/tuesday-2g-opt.png)

幸运的是，有很多不错的选择可以协助自动收集数据，并根据指标衡量一段时间内网站的表现。记住，良好的性能指标是被动和主动监控工具的组合：

- 被动监控工具，可以根据请求来模拟用户交互（例如 Lighthouse ， WebPageTest 等综合测试）

- 连续记录和评估用户交互的主动监控工具（真实用户监控，例如 SpeedCurve ， New Relic - 这两种工具也提供综合测试）

前者保证了产品工作的稳定性，在开发过程中发挥重要作用。后者对于产品的长期维护非常有利，因为它有助于了解在实际访问站点时发生的性能瓶颈。通过使用导航定时，资源定时，绘图定时，长时间任务等内置的 RUM API ，被动和主动性能监视工具共同维护应用程序的性能。例如，可以使用 [PWMetrics](https://github.com/paulirish/pwmetrics) ， [Calibre](https://calibreapp.com/) ， [SpeedCurve](https://speedcurve.com/) ， [mPulse](https://www.soasta.com/performance-monitoring/) 和 [Boomerang](https://github.com/yahoo/boomerang) ， [Sitespeed.io](https://www-origin.sitespeed.io/) ，这些都是性能监控的绝佳选择。

注意：选择浏览器外部的网络级限制器比较安全，比如， DevTools 的实现方式会导致，在 HTTP / 2 推送时， DevTools 会出现问题（谢谢 Yoav ！）。

![Lighthouse, a performance auditing tool integrated into DevTools](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/d829af6f-23ff-432c-9659-bd6f3c13678f/lighthouse-shop-polymer-opt.png)

### 给你的同事安利性能优化の备忘录（Share the checklist with your colleagues）

（不过在安利之前），（首先）要确保你团队的成员都熟悉备忘录中的优化策略，这样就能避免日后出现（备忘录优化策略）使用方法上的误区。在我们需要做决策的环节，性能优化の备忘录都会有相对应的性能优化小贴士，前端开发人员通过对性能的适当沟通，可以使项目获得巨大收益，每个成员也会因此具有责任感，而不仅仅是作为前端开发人员。产品的开发设计将根据性能预算和清单中定义的优先顺序来决定。

![RAIL, a user-centric performance model](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/c91c910d-e934-4610-9dc5-369ec9071b57/rail-perf-model-opt.png)

## 制定目标须务实（Setting Realistic Goals）

### 控制响应时间在 100 ms，控制帧速在 60 帧/秒（100-millisecond response time, 60 fps）

为了使交互体验更好，界面有 100ms 响应用户的输入。否则，用户会认为该应用程序很慢。 [RAIL performance model](https://www.smashingmagazine.com/2015/10/rail-user-centric-model-performance/) 提出一些不错的性能优化指标：在用户初始化 input 输入框后，务必在 100ms 之内给出反馈。考虑到存在响应时间不足 100ms 的情况，页面最迟在 50ms 的时候，把控制权交给主线程。另外针对优化时有压力的环节（如动画），最好的做法是，在能够优化的地方，尽量优化到极致；在不能优化的地方，让性能开销降至最低。

因此，动画的每一帧都应该控制在 16ms 以内，这样就可以实现 60fps（1s/60 = 16.6ms），当然把每一帧控制在 10ms 以内更合适。考虑到浏览器绘制新的画面需要时间，要实现 60fps 的目标，代码应该在 16.6ms 之前结束运行，并合理利用剩下的时间。显然，这些性能优化指标适用于优化运行时的性能（runtime performance），而不是用于优化载入时的性能（loading performance）。

### 速度 < 1250，3G 上交互时间 < 5s，关键文件大小 < 170Kb（SpeedIndex < 1250, TTI < 5s on 3G, Critical file size budget < 170Kb）

尽管实现起来会非常困难，但最终目标仍然是第一次有效的渲染时间低于 1 秒，[速度值](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index)低于 1250。在 3G 慢速网络下，考虑到 200 美元 Android 手机（例如 Moto G4 ）的基准水平，400ms 的往返时间和 400kbps 的传输速度，[目标是互动时间低于 5s](https://www.youtube.com/watch?v=_srJ7eHS3IM&feature=youtu.be&t=6m21s) ，重复访问下低于 2s。

值得注意的是，对于“交互时间”，最好区分[第一互动和一致互动](https://calendar.perfplanet.com/2017/time-to-interactive-measuring-more-of-the-user-experience/)，以避免误解。前者是主要内容呈现之后的最早节点（其中至少有 5 秒钟的页面响应时间）。后者是页面可以持续对输入作出响应的时间节点。

HTML的第一个 14〜15Kb 是最关键的有效数据块，也是第一次往返传递的部分。更简单地说，为了实现上述目标，需要给关键文件最大的[文件预算](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/)。170Kb 的压缩（ 0.8-1MB 解压缩）平均需要长达 1 秒（取决于资源类型）的解析和编译。稍高一点是可以的，但会降低其价值。

尽管如此，还是可以提高绑定的规模预算。比如，开始渲染之前，或者[追踪前端CPU](https://calendar.perfplanet.com/2017/tracking-cpu-with-long-tasks-api/)时，在浏览器主线程上设置性能。 [Calibre](https://calibreapp.com/) ，[SpeedCurve](https://speedcurve.com/) 和 [Bundlesize](https://github.com/siddharthkp/bundlesize) 等工具有助于控制预算，并将其集成到构建过程中。

![From Fast By Default: Modern loading best practices by Addy Osmani (Slide 19)](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/3bb4ab9e-978a-4db0-83c3-57a93d70516d/file-size-budget-fast-default-addy-osmani-opt.png)

## 环境搭建（Defining The Environment）

### 做好构建工具的选型以及搭建好（适合自己的）构建工具（Choose and set up your build tools）

[不要过分关注那些在现在看来很炫酷的技术栈](https://24ways.org/2017/all-that-glisters/)，根据自己项目环境来选择构建工具（比如使用 Grunt、Gulp、Webpack、Parcel 或者结合使用上述工具）。另外只要你的构建工具选型有更快的产出，并且在项目构建过程中没有遇到问题，（就可以说）你的构建工具选型就是好的。

### 渐进式提升（Progressive enhancement）

把（如何）[渐进式提升性能](https://www.aaron-gustafson.com/notebook/insert-clickbait-headline-about-progressive-enhancement-here/)作为前端架构以及项目部署的指导思想，这不失为一种安全的做法（a safe bet）。首先要设计出具有核心（用户）体验的产品，然后在（支持新特性的）浏览器使用新特性来提升用户体验，从而营造出极具[弹性 Web 设计（resilient）](https://resilientwebdesign.com/introduction/)的用户体验。假设你的网站都能在这些糟糕的环境下（烂屏幕、烂浏览器、渣网络以及运行超慢的电脑）跑的很快，那么在更好的环境下（好浏览器、好网络以及运行超快的电脑），你的网站只会跑的更快。

### 选择一个强大的性能基准（Choose a strong performance baseline）

有那么多的未知因素会影响加载——网络、热节流（译者注:温度过高时自动降低频率的系数）、缓存逐出、第三方脚本、解析器阻止模式、磁盘I/O、IPC(过程间通信)掉帧、安装拓展、CPU、硬件和内存限制、L2/L3缓存 差异、rtts、图像、网页字体加载行为——JavaScript是最影响体验的环节，其次是web字体和图像的渲染往往也消耗太多的内存。随着性能瓶颈从服务器移到客户端，作为开发者，我们必须更详细地考虑所有可能发生的情况。

​在一个已经包含关键路径html / css / javascrip、路由器、状态管理、实用程序、框架和应用程序逻辑的170kb的预算中，我们必须仔细研究网络传输成本、解析/编译的时间和运行的时间成本来选择我们的框架。

![From Fast By Default: Modern Loading Best Practices by Addy Osmani (Slides 18, 19)](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/39c247a9-223f-4a6c-ae3d-db54a696ffcb/tti-budget-opt.png)

​正如Seb Markbåge所指出的那样，衡量框架启动成本的一个好方法是首先渲染一个视图，然后删除它，然后再次渲染，因为它可以告诉你框架是如何扩展的。

第一次渲染倾向于对一堆懒惰编译的代码进行预热，当它扩展时，更大的分支可以从中受益。第二次渲染基本上是仿效页面上的代码重用是如何随着页面复杂度的增长来影响性能特征。

不是每一个项目都需要一个框架。事实上，有些项目完全不需要框架就可以完成。一旦选择了一种框架，你可能至少几年内都不会再换。所以，如果你真的需要使用，请认真考虑过后再确定。最好在选择一个框架之前，先去考虑它的大小和初始解析时间的总成本，像[Preact](https://github.com/developit/preact)，[Inferno](https://github.com/infernojs/inferno), [Vue](https://vuejs.org/), [Svelte](https://svelte.technology/) 或者 [Polymer](https://github.com/Polymer/polymer)等轻量级的框架就可以让工作顺利完成了。框架的大小基线将为你的应用程序代码定义约束条件。

![JavaScript parsing costs can differ significantly. From Fast By Default: Modern Loading Best Practices by Addy Osmani (Slide 10)](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/8a36eef0-083f-4652-9814-95ffe7848982/parse-costs-opt.png)

请记住，对比台式机，在移动设备上，项目的加载速度你应该预计减速4倍-5倍。移动设备有不同的GPUs、CPU，不同的内存，不同的电池特性。手机上的解析时间成本比PC的要高出36％。所以，请始终在平均设备上进行测试——这是最能代表受众群体的设备。

​不同的框架将会对性能产生不同的影响，并且需要不同的优化策略，所以你必须清楚地了解你所依赖的框架的所有细节。在构建Web应用程序时，请查看[PRPL ](https://developers.google.com/web/fundamentals/performance/prpl-pattern/)模式和应用程序外壳架构。这个想法很简单：推动初始路由交互所需的最少代码快速渲染，然后使用服务进行缓存和预缓存资源，然后延迟加载您需要的路由，这是异步的。

![PRPL stands for Pushing critical resource, Rendering initial route, Pre-caching remaining routes and Lazy-loading remaining routes on demand](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/bb4716e5-d25b-4b80-b468-f28d07bae685/app-build-components-dibweb-c-scalew-879-opt.png)

![An application shell is the minimal HTML, CSS, and JavaScript powering a user interface](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/6423db84-4717-4aeb-9174-7ae96bf4f3aa/appshell-1-o0t8qd-c-scalew-799-opt.jpg)

### 使用 AMP 技术或者 Instant Articles 技术？（Will you be using AMP or Instant Articles？）

（译者注:AMP即为加速移动网页，**是**一种制作可快速加载的轻量型网页的方法，特别适合于移动设备）

根据你团队的优先事项和策略，你可能会考虑使用谷歌的 [AMP](https://www.ampproject.org/) 或者FaceBook的 [Instant Articles](https://instantarticles.fb.com/)或者苹果的 [Apple News](https://www.apple.com/news/)。虽然没有它们也可以，但是AMP确实提供了一个可靠的性能框架和一个免费的内容分发网络(CDN)，而 Instant Articles将提高你在Facebook上的知名度和性能。

这些技术对用户的主要好处是保证了性能，所以有时他们可能更喜欢AMP- / Apple 新闻/即时页面链接，而不是“普通”页面和可能臃肿的页面。对于倾向处理大量第三方内容的网站，这些选择可能会大大加速渲染时间。

网站所有者的利益是显而易见的：这些格式在各自的平台上的可发现性以及在搜索引擎中的可见度提高。你也可以建立一个渐进的网络AMPs，通过用amps作为你的PWA的数据源。缺点？显而易见，在有围墙的花园中开发人员可以制作和维护其内容的单独版本，并在Instant Articles 和Apple News没有实际的网址的情况下。

### 合理利用 CDN ？（Choose your CDN wisely？）

基于你动态数据的规模，你可能会把一部份内容“外包”（outsource）给用[静态站点生成工具](https://www.smashingmagazine.com/2015/11/static-website-generators-jekyll-middleman-roots-hugo-review/)如 Jekyll 生成的静态文件，接着把静态文件推送到 CDN 上，最后 CDN 只是提供静态文件的静态版本，所以这样就可以避免发起对数据库的读写请求。甚至你可以搞一个基于 CDN 的[静态文件托管平台（static-hosting platform）](https://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/)，(这样就可以)通过给页面添加可交互组件的方式来丰富你的页面，并以此作为性能提升的标志（[JAMStack](https://jamstack.org/)）。

请注意，CDN 也是可以托管并卸载（offload）动态内容的，所以咱们没有必要把CDN的服务范围限定在静态资源。（另外需要你记住的是），不管你的 CDN 是否执行内容压缩（GZip）、内容转换、HTTP/2 传输以及 ESI（一种标记语言，可以用它把网页划分为单独的可缓存的实体）等操作，我们还是需要复核上述操作的，这是因为上述操作不仅会在 CDN 的 edge 处（服务器最接近用户的地方）聚合页面中的静态以及动态内容，也还会执行其它任务。

## 优化构建（Build Optimizations）

### 如实（straight）设置优先级（Set your priorities straight）

知道自己第一步要干啥，（对自己来说）是件好事。例如，首先在页面中写好并运行请求（某目录下所有）资源（JS、图片、字体、第三方js脚本以及开销很大的模块，比如轮播图、复杂的信息图（infographics）以及多媒体内容）的代码，然后（使用浏览器的开发者工具 Network 选项卡）将这些资源进行分组。

先搞清楚资源（assets）可以分为几类（set up a spreadsheet），大致可以分为：

- 针对传统浏览器定义的核心（core）型资源（比如可以完整访问的核心型内容）

- 针对现代浏览器定义的增强（enhanced）型资源（比如增强型内容）

- 其它（extras）型资源（指的是该资源可选且支持懒加载，如 web 字体、多余的样式、轮播脚本、媒体播放器、社交媒体（social media）按钮以及大图）。

我们发表了一篇关于“[提升Smashing Magazine的性能](https://www.smashingmagazine.com/2014/09/improving-smashing-magazine-performance-case-study/)”的博文，这里面会详细讲述有关于上述问题的解决办法。

### 考虑使用 “cutting-the-mustard” 技术（Consider using the "cutting-the-mustard" pattern）

尽管相当古老，但我们仍然可以使用[cutting-the-mustard 技术](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard) 来实现不同类型的浏览器载入不同类型的资源(传统浏览器载入核心型资源，现代浏览器载入增强型资源)。加载资源应严格遵守规则：页面加载时应首先载入主要资源，然后在DomContentLoaded事件触发时载入Enhancement资源，最后在Load事件触发时载入其他资源。请注意，该技术从(安装在设备中的)浏览器来推断出设备性能的好坏，这已经不是必要做的事了。

举个例子，在发展中国家的低配版Android手机在它们有限的内存和CPU的能力下仍然会内置chrome浏览器，而且都能达到(技术标准所提出的)要求，这就是PRPL模式可以作为一个很好的选择的地方。最终，使用[设备内存客户端提示头](https://github.com/w3c/device-memory)，我们将能够更好地去定位低端设备。截止到本文编写的时候，提示头仅支持闪烁（一般用于客户端提示）。由于设备内存还具有一个已在Chrome中可用的JavaScript API，因此可以是基于API来检测特征，并仅在不支持它时回退到“cutting-the-mustard”技术。

### 减少解析Javascript的成本（Parsing JavaScript is expensive, so keep it small）

在处理单页面应用程序时，可能需要一些时间来初始化应用程序，然后才能呈现页面。寻找模块和技术来优化最初的渲染时间（例如，这里是如何调试反应性能，以及如何提高角度性能），因为大多数性能问题来自于初始解析时间来引导应用程序。

JavaScript有成本，但并不一定是指性能上消耗的文件大小。解析和执行时间因为设备硬件的差异而有着显著的区别。在一般的手机（moto g4）上，1MB（未压缩）的javascript的解析时间将在1.3-1.4s左右，15-20％的时间都在手机上进行解析。根据编译的执行过程，JavaScript的单纯加载平均需要4秒，大约11秒之前，会有第一次有意义的移动绘画。前提：在低端移动设备上，解析和执行时间可以轻松地提高2-5倍。

避免解析成本的一个有趣的方法是使用最近引入实验的 [binary templates](https://emberjs.com/blog/2017/10/10/glimmer-progress-report.html#toc_binary-templates) ，这些模板不需要被解析。

这就是为什么检查每个单独的JavaScript依赖项是很重要的原因，像 [webpack-bundle-analyzer](https://www.npmjs.com/package/webpack-bundle-analyzer)，[Source Map Explorer](https://github.com/danvk/source-map-explorer) 和 [Bundle Buddy](https://github.com/samccone/bundle-buddy) 这样的工具都可以帮助你实现这一点。去计算JavaScript解析和编译时间吧，推荐Etsy的一个小工具[DeviceTiming](https://github.com/danielmendel/DeviceTiming)，它可以帮助计算你的JavaScript在任何设备或浏览器上的解析和执行时间。但是：虽然规模很重要，但这不是一切。当脚本大小增加时，解析和编译时间不一定会线性增加。
[![Webpack Bundle Analyzer visualizes JavaScript dependencies](https://i.vimeocdn.com/video/675364033.png?mw=1200&mh=722&q=70)](https://vimeo.com/249525818)

### 你使用的是ahead-of-time的编译器吗？（Are you using an ahead-of-time compiler?）

使用一个[ahead-of-time compiler](https://www.lucidchart.com/techblog/2016/09/26/improving-angular-2-load-times/)(编译器)，将一些客户端呈现转移到服务器，从而快速输出可用的结果。最后，考虑使用[Optimize.js](https://github.com/nolanlawson/optimize-js) 来优化，通过包装可快速调用的函数来实现(在app初始时能够)快速载入（但可能并不需要了）。

### 你在使用tree-shaking，规模提升(scope hoisting )和代码分割吗？（Are you using tree-shaking, scope hoisting and code-splitting?）

[Tree-shaking](https://medium.com/@roman01la/dead-code-elimination-and-tree-shaking-in-javascript-build-systems-fb8512c86edf)是一种可以过滤webpack中未使用的输出，只包含生产中实际使用的代码构建的方法。使用webpack 3和Rollup的话，我们也有 [scope hoisting](https://medium.com/webpack/brief-introduction-to-scope-hoisting-in-webpack-8435084c171f)，可以让这两个工具检测导入链接在哪里可以被平整并转换成一个内联函数而不影响其他代码的使用。如果是webpack 4，你还可以使用 [JSON Tree Shaking](https://react-etc.net/entry/json-tree-shaking-lands-in-webpack-4-0)。 [UnCSS](https://github.com/giakki/uncss)或 [Helium](https://github.com/geuis/helium-css)可以帮助你从CSS中删除未使用的样式。

另外，你可能要考虑学习如何编写高效的CSS选择器，以及如何避免膨胀和耗性能的样式。你还可以做更多的事，比如你也可以使用Webpack来缩短CSS类名，并通过作用域隔离在编译时动态地重命名CSS类名。

代码拆分是另一个Webpack功能，将你的代码分解为按需加载的“块”。不是所有的JavaScript都必须立即下载，解析和编译。一旦在代码中定义了分割点，Webpack就可以处理依赖和输出文件，它可以减少最初的加载量并使应用程序按需请求代码。另外，考虑使用 [preload-webpack-plugin](https://github.com/GoogleChromeLabs/preload-webpack-plugin) ，它根据代码分解的路由，然后用`<link rel="preload">`或者`<link rel="prefetch">`来提示浏览器去使用和预加载。

那在哪里定义(代码)拆分点：通过追踪哪些CSS/JavaScrip被使用，哪些不被使用。Umar Hansa 解释了如何使用 Devtools的代码覆盖率来实现它。

如果你不使用Webpack，那 [Rollup](http://rollupjs.org/) 会比 Browserify exports更好。虽然这么说，但你可能需要查看 [Rollupify](https://github.com/nolanlawson/rollupify)，它将ECMAScript 2015模块转换为一个大的 CommonJS 模块 - 因为根据你对bundler和模块系统的选择，小模块可以有惊人的高性能表现。

最后， ES2015在现代浏览器中得到了非常好的支持，可以考虑使用[babel-preset-env](http://2ality.com/2017/02/babel-preset-env.html) 来转译浏览器不支持的 ES2015+的功能。创建两个版本，ES6一个，ES5一个。我们可以使用`script type =“module”`来让带有ES模块的浏览器支持加载文件，而低版本的浏览器则可以使用script nomodule加载遗留的构建。

对于lodash来说，使用[babel-plugin-lodash](https://github.com/lodash/babel-plugin-lodash) 将只加载你在源代码中使用的模块，这样会为你节省相当多的JavaScript负载。
![From Fast By Default: Modern Loading Best Practices by the one-and-only Addy Osmani. Slide 76](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/31237c37-d7db-4faa-9849-51657e122331/babel-preset-opt.png)

### 利用你使用的 JavaScript 引擎进行优化（Take advantage of optimizations for your target JavaScript engine）

根据你的用户群研究占主导地位的JavaScript引擎，然后探索优化它们的方法。例如，当在用于Blink引擎的浏览器、Node.js 运行和Electron的v8优化时，对单脚本使用脚本流。它允许async或者defer scripts 在下载开始后在单独的后台线程上解析，因此在某些情况下可以提高多达10％的页面加载时间。实际上，`<head>`中使用`<script defer>`，以便浏览器可以及早发现资源，然后在后台线程上解析它。

警告：Opera Mini浏览器不支持延迟加载，所以如果你正在开发印度或非洲的项目时，延迟将被忽略，导致阻塞渲染，直到脚本已被评估。
![Progressive booting means using server-side rendering to get a quick first meaningful paint, but also include some minimal JavaScript to keep the time-to-interactive close to the first meaningful paint](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/ab06acd3-833a-4634-abf9-fc8d91939250/fmp-and-tti-opt.jpeg)

### 选择客户端渲染或服务器端渲染？(Client-side rendering or server-side rendering?)

在这两种情况下，我们的目标应该是设置逐步引导：使用服务器端渲染来获得快速的第一个有意义的绘制，但也包括一些最小必要的JavaScript，以保持时间与交互都接近它。如果JavaScript在第一次有意义的绘制之后出现得太晚，那么浏览器可能会锁定主线程，而解析、编译和执行后期发现的JavaScript，从而对站点或应用程序的交互性造成限制。

为了避免这个问题，需要将函数的执行分解为单独的异步任务，并在可能的情况下使用requestidlecallback。可以使用WebPack的动态`import（）`支持延迟加载ui，直到用户确实需要前避免加载，解析和编译的成本。

在本质上， Time to Interactive  (TTI)可以告诉我们navigation和Interactive之间的时间长度。在呈现初始内容之后，通过查看第一个5秒窗口(window )来定义度量标准，其中是否有JavaScript任务花费的时间超过50毫秒，如果发生超过50ms的任务，则重新开始搜索5秒钟的窗口。因此，浏览器会首先假定它达到了Interactive，只是为了切换到Frozen的状态，才最终切换回到Interactive。

一旦我们达到Interactive，我们可以随时随地启动应用程序的次要部分。不幸的是，正如 [Paul Lewis](https://aerotwist.com/blog/when-everything-is-important-nothing-is/#which-to-use-progressive-booting) 注意到的那样，框架通常没有优先级的概念可以提供给开发人员，因此大多数的库和框架都难以实现渐进式引导。如果你有时间和资源，就用这个策略来最终提升性能。

### 你是否限制了第三方脚本的影响？(Do you constrain the impact of third-party scripts?)

在所有性能优化的情况下，我们经常无法控制来自业务需求的第三方脚本。第三方脚本指标不受终端用户体验的影响，所以通常只要有一个脚本调用讨厌的第三方脚本，就可以最终破坏性能测试的专一性。为了控制和缓解这些脚本带来的性能损失，只是将它们异步加载（可能通过延迟），并通过资源提示（如dns-prefetch或preconnect）加速它们是不够的。

正如Yoav Weiss 在第三方脚本的重要讨论中所解释（[must-watch talk on third-party scripts](http://conffab.com/video/taking-back-control-over-third-party-content/)），在许多情况下，这些脚本会下载动态资源。资源会在页面加载中发生变化，所以我们不一定知道哪些主机将从中下载资源，以及它们会是什么资源。

那我们有什么选择呢？考虑使用服务暂停资源下载（**using service workers by racing the resource download with a timeout**），如果资源在一定超时内没有响应，则返回一个空的响应告诉浏览器继续进行解析页面。您还可以记录或阻止不成功或不符合特定条件的第三方请求。另一种选择是建立内容安全策(CSP) 以限制第三方脚本的影响，例如，不允许下载音频或视频。最好的选择是通过`<iframe>`嵌入脚本，使脚本在iframe中运行，因此不能访问页面的dom，也不能在你的域上运行任意代码。可以使用`sandbox`属性对iframe进行限制，比如可以禁用iframe可以执行的任何功能，例如，阻止脚本运行，防止警报，表单提交，插件，访问顶部导航等等。

例如，可能需要允许脚本使用`<iframe sandbox =“allow-scripts”>`来运行。每个限制都可以通过sandbox属性上的各种允许值来提高（几乎在任何地方都支持），因此可以将它们限制在被允许的范围内。考虑使用[Safeframe](https://github.com/InteractiveAdvertisingBureau/safeframe) 和Intersection Observer;that would enable ads to be iframed while still dispatching events or getting the information that they need from the DOM (e.g. ad visibility). 注意新的策略，例如功能策略，资源大小限制和CPU /带宽优先级，来限制会使浏览器速度变慢的有害Web功能和脚本。同步脚本，同步xhr请求，document.write和落后的实现方法。

对第三方进行压力测试，在DevTools的性能配置文件页面中从头到尾检查，测试如果请求被阻止或超时，会发生什么情况 - 对于后者，可以使用WebPageTest's Blackhole服务器`72.66.115.13`,可以指定特定的域到你的主机文件。最好是自托管主机(self-host)，并使用单个主机名，但也会生成一个请求映射，显示第四方调用并检测脚本何时更改。
![Image credit: Harry Roberts](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/b1e12dad-ea64-430e-b3db-b67fb76029d8/block-request-url-image-opt.png)

### http缓存头设置是否正确?（Are HTTP cache headers set properly?）

(设置HTTP cache header的正确做法)，其实是需要对已经正确设置的`expires`、`cache-control`、`max-age`以及其它HTTP缓存响应头(HTTP cache header)进行二次检查。一般而言，资源都应该被缓存下来，(除非资源在短时间内可能会变或者资源属于没有时间限制的(indefinitely)静态资源，你才可以对该资源不进行缓存处理)，这样一来，在需要资源的时候，你只需要改变资源在URL中版本号。禁用最后修改的标题，因为任何资产都将导致带有if-modified-since-header的条件请求，即使资源处于高速缓存中也是如此。与Etag一样，即使它有它的作用。

使用`Cache-control: immutable`：(其实是为了解决fingerprinted静态资源的缓存问题而被设计出来的，解决了客户端revalidation问题（截至2017年12月，[仅Firefox、Edge和Safari支持](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control);在Firefox也只支持https：。当然你也可以参考这些[Heroku’s primer on HTTP caching headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)、Jake Archibald的[缓存之最佳实践](https://jakearchibald.com/2016/caching-best-practices/)以及Grigorik的[HTTP caching primer](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=en)。另外，要注意不同的头文件，特别是与[CDNs](https://www.fastly.com/blog/getting-most-out-vary-fastly)相关的文件，并且要注意关键头文件([Key header](https://www.greenbytes.de/tech/webdav/draft-ietf-httpbis-key-latest.html) )，这有助于避免每当新的请求与先前的请求略有不同时多进行验证的额外交互

## 优化资源（Assets Optimizations）

### 是否使用了Brotli或Zopfli压缩文本？（Is Brotli or Zopfli plain text compression in use?）

2015年，Google推出了新的开源无损数据格式Brotli，现在所有的浏览器都支持这种格式。 实际上，Brotli似乎比Gzip和Deflate更有效。 压缩速度可能非常缓慢，具体取决于设置，但较慢的压缩可获得较高的压缩率。 不过，它解压的速度很快。只有当用户通过HTTPS访问网站时，浏览器才会接受它。 那有什么不足呢？ Brotli今天仍然没有在某些服务器上预装，而且如果不自己编译NGINX或者Ubuntu，也不是简单的。 不过，这并非那么困难。 事实上，有些CDN支持它，甚至可以在尚不支持Brotli的CDN上启用它（使用服务工作者）。在最高的压缩级别下，Brotli速度非常慢，以至于服务器在等待动态压缩资源时开始发送响应所花费的时间可能会使文件大小的任何潜在收益都无效。 然而，对于静态压缩，更高的压缩设置是首选。或者，您可以考虑使用Zopfli的压缩算法，该算法将数据编码为Deflate，Gzip和Zlib格式。 任何常规的Gzip压缩资源都将受益于Zopfli改进的Deflate编码，因为这些文件比Zlib的最大压缩率要小3％到8％。 问题在于文件的压缩时间会增加大约80倍。对于一些不常改变的资源，以及那些压缩一次然后下载多次的文件，使用Zopfli是一个好主意。如果您可以忽略动态压缩静态资产的成本，这是值得的。 Brotli和Zopfli都可以用于任何明文有效载荷 - HTML，CSS，SVG，JavaScript等等。这个策略？ 在最高级别使用Brotli + Gzip预压缩静态资源，并使用Brotli在1-4级即时压缩（动态）HTML。 另外，检查CDN是否支持Brotli（例如KeyCDN，CDN77，Fastly）。 确保服务器正确处理Brotli或gzip的内容协商。 如果您不能在服务器上安装/维护Brotli，请使用Zopfli。

### 你会合理优化图片吗？（Are images properly optimized?）

尽可能使用具有srcset，sizes和`<picture>`元素的响应式图片。 您可以通过为WebP图像提供`<picture>`元素和JPEG回退（请参阅Andreas Bovens的代码片段），或者通过内容协商（使用Accept头），来使用WebP格式（即将在Chrome，Opera，Firefox中支持）。Sketch本身支持WebP，并且可以使用Photoshop的WebP插件从Photoshop导出WebP图像。其他选项也可用。如果您使用的是WordPress或Joomla，可以使用扩展来帮助您轻松实现对WebP的支持，例如WordPress的Optimus和Cache Enabler以及Joomla自身已支持的扩展（通过Cody Arsenault）。您也可以使用客户端提示，但仍需要获得一些浏览器支持。 在复杂的标记下没有足够的资源去实现响应式图像？ 使用响应式图像断点生成器或Cloudinary等服务自动进行图像优化。 而且，在许多情况下，单独使用srcset和size将会获得显着的好处。在Smashing杂志上，我们使用postfix -opt作为图像名称，例如brotli-compression-opt.png; 每当图像包含该后缀时，团队中的每个人都知道图像已经被优化。

![The Responsive Image Breakpoints Generator automates images and markup generation](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/db62c469-bbfc-4959-839d-590abb41b64e/responsive-breakpoints-opt.png)

### 图片优化进阶（Take image optimization to the next level）

当你在开发一个页面，上面有一个特定图像的加载速度非常关键，确保JPEG是渐进式的，并且使用Adept，mozJPEG（通过操纵扫描级来改善开始渲染时间）或者Guetzli 去压缩。Guetzli是Google新推出的着重于感知性能的开源编码器，并参考了Zopfli和WebP。 唯一的缺点是处理速度慢（每百万像素消耗CPU一分钟）。 对于PNG，我们可以使用Pingo，而SVGO或SVGOMG用于SVG。每一个图像优化的文章都会提到这点，但保持矢量资源干净和紧密总是值得提醒。 确保清理未使用的资源，删除不必要的元数据，并减少图稿中的路径点数量（从而减少SVG代码）。到目前为止，这些优化只涵盖了基础知识。 Addy Osmani已经发布了非常详细的关于基本图像优化的指南，深入到图像压缩和颜色管理的细节。例如，您可以模糊图像中不必要的部分（通过对其应用高斯模糊滤镜）以减小文件大小，最终甚至可以开始移除颜色或将图像变成黑白色，以进一步缩小图像尺寸。对于背景图像，从Photoshop导出的照片质量为0到10％也是绝对可以接受的。GIF呢？那么，我们可以使用循环的HTML5视频，而不是影响渲染性能和带宽的笨重的GIF动画，虽然浏览器使用`<video>`时的性能很慢，但它与图像不同，浏览器不会预加载`<video>`内容。至少我们可以使用Lossy GIF，gifsicleor 或giflossy为GIF添加有损压缩。
好消息：希望很快我们可以使用`<img src =“.mp4”>`加载视频，早期的测试表明，img标签显示的速度比原来快了20倍，解码速度比GIF相当快了7倍，除了是文件大小的一小部分。还不够好？那么，你也可以使用多个背景图像技术提高图像的感知性能。请记住，利用对比度和模糊不必要的细节（或删除颜色）也可以减小文件的大小。啊，你需要放大一个小照片同时不丢失质量？考虑一下使用Letsenhance.io。

![Zach Leatherman's Comprehensive Guide to Font-Loading Strategies provides a dozen of options for better web font delivery](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/eb634666-55ab-4db3-aa40-4b146a859041/font-loading-strategies-opt.png)

### 网页字体是否已经优化？（Are web fonts optimized?）

第一个问题值得问一问，你是否可以摆脱使用UI系统字体。如果情况并非如此，那么很有可能您所使用的网络字体包括字形和额外的功能以及粗体都是未被使用的。您可以向字体设计公司获取网络字体子集或子集，如果您使用的是开源字体（例如仅包含带有某些特殊的重音字形的拉丁语）还可以自己制作子集来最小化其文件大小。
WOFF2的支持非常好，对于不支持WOFF2的浏览器，你可以使用WOFF和OTF作为后备。 另外，从Zach Leatherman的“字体加载策略综合指南”（也可以找到Web字体加载方法的代码片段）中选择一个策略，并使用服务工作者缓存持久地缓存字体。 需要一个快速的胜利？ Pixel Ambacht有一个快速教程和案例研究，把您的字体整理好。如果您无法从服务器获取字体并依赖于第三方主机，请确保使用字体加载事件（或对不支持它的浏览器使用Web字体加载器）。 FOUT比FOIT好;在回退之后立即开始渲染文本，并异步加载字体 - 也可以使用loadCSS。你也许能够摆脱本地安装的操作系统字体，也可以使用可变的字体。怎么设计一个防弹字体加载策略？从font-display开始，然后回到Font Loading API，然后回到Bram Stein的Font Face Observer。 （感谢Jeremy！）如果您有兴趣从用户的角度衡量字体加载的性能，Andreas Marschke将研究Font API和UserTiming API进行性能跟踪。此外，不要忘记包含font-display：可选的描述符，用于弹性和快速字体回退，unicode范围将大字体分解为更小的特定于语言的字体，由于两种字体之间有尺寸差异，可以使用Monica Dinculescu的字体样式匹配器来减少布局上的移位。

## HTTP2

### 做好（网站）HTTPS 的迁移工作，然后对其开启 HTTP/2（Migrate to HTTPS, then turn on HTTP/2）

在 Google [倡导网站 HTTPS 化](https://security.googleblog.com/2016/09/moving-towards-more-secure-web.html)以及 Chrome 会把（所有使用 HTTP 的）网页认定为“不安全”的大环境下，让网站拥抱 [HTTP/2](https://http2.github.io/faq/)，是顺应时代的潮流，不可避免（a switch to HTTP/2 environment is unavoidable）。而且从目前来看，[主流浏览器对 HTTP/2 支持度还是不错的](https://caniuse.com/#search=http2)；（需要注意的是），现在的 HTTP/2 还不能够适用于所有的场景（it isn't going anywhere）；不过，在某些场景下，使用 HTTP/2 会让你大力出奇迹（you're better off with it）。一旦你网站上了 HTTPS，那么，你就可以利用 service worker 以及 HTTP/2 的服务器推送功能来获取[更显著的性能提升](https://www.youtube.com/watch?v=RWLzUnESylc&t=1s&list=PLNYkxOF6rcIBTs2KPy1E6tIYaWoFcG3uj&index=25)（至少从长远看）。

![Eventually, Google plans to label all HTTP pages as non-secure, and change the HTTP security indicator to the red triangle that Chrome uses for broken HTTPS](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/30dd1821-9800-4f01-91a8-1375d4812144/http-pages-chrome-opt.png)

（在这个过程中，你会发现），耗时最长的任务其实是 [HTTPS 的迁移](https://https.cio.gov/faq/)，（而且你还会发现），耗时的长短取决于 HTTP/1.1 项目的用户（指的是那些还在使用低版本操作系统或者使用低版本浏览器的用户）基数。（出于低版本浏览器性能优化的考虑），所以你不但需要打包出符合（该款浏览器）性能优化的资源，而且还需要把打包后的资源推给浏览器。所以这也需要你去适应[上述构建流程](https://rmurphey.com/blog/2015/11/25/building-for-http2)。需要注意的是，在进行 HTTPS 迁移操作的同时，进行资源构建的操作，可能会让上述操作变得复杂（tricky）、耗时。（另外我想说的是），接下来所讲的内容，都是针对之前切过 HTTP/2 环境或者现在正准备切 HTTP/2 环境（的读者）来展开的。

### 正确部署 HTTP/2（Properly deploy HTTP/2）

重要的事情说两遍，在你开始使用 HTTP/2 请求资源之前，需要搞清楚你以前是如何请求资源的。另外需要你在载入大模块以及并行载入小模块之间找到一个平衡点。 一方面，你可能不愿意把所有资源合并到一起，而是希望把所有视图都分散到（小型）模块里面，然后在项目构建的过程中完成对（小型）模块的压缩，最后通过[ “scount” approach](https://rmurphey.com/blog/2015/11/25/building-for-http2) 以及异步的方式来分别实现对模块的引用及载入。这样的话，就不存在文件改变需要（重新）引入整个样式文件或者重新下载 JavaScript 脚本的情况。 另一方面，HTTP/2 环境下打包 JS 文件时存在问题，这是因为向浏览器发送很多小型的 JS 文件的过程中会存在很多问题。 首先，文件压缩的优势被破坏。在压缩大文件的过程中，借助目录重用的特点，达到优化性能的目的，然而单个文件就不能。虽说有方法可以解决上述问题，但是这些方法在现在看来有点激进。 其次，浏览器不能针对一些工作流进行优化。例如，在 Chrome 浏览器，[IPCs](https://www.chromium.org/developers/design-documents/inter-process-communication) 与资源数成线性关系，因此（向浏览器发送）数以百计的文件资源会导致浏览器产生运行时开销。

你仍然可以尝试通过[渐进的方式载入 CSS](https://jakearchibald.com/2016/link-in-body/)。很显然，这种做法不利于 HTTP/1.1 的用户，因此在部署 HTTP/2 的过程中，你可能需要针对不同的浏览器创建并发送（该浏览器支持）HTTP 协议的报头，这（还只）是部署过程中稍微复杂的地方。不过你可以通过 [HTTP/2 connection coalescing](https://daniel.haxx.se/blog/2016/08/18/http2-connection-coalescing/)（可以让你在 HTTP/2 环境使用域名共享）来避开这些，不过在现实中很难实现。
肿么办？如果你简单了解过 HTTP/2，（就应该知道），（一次请求）发送10个数据包似乎是不错的折中做法（上述做法即使是放到以前的浏览器身上，效果也不会太差）。（话虽如此），不过我们还是需要通过实验、测试等手段来帮网站在载入大模块以及并行载入小模块之间寻求合适的平衡点。

![To achieve best results with HTTP/2, consider to load CSS progressively, as suggested by Chrome's Jake Archibald](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/24d7fcb0-40c3-4ada-abb3-22b8524f9b2d/progressive-css-loading-opt.png)

### 服务器以及部署的CDN都支持HTTP/2吗？（Do your servers and CDNs support HTTP/2？）

（一般来说），不同的服务器、CDN 对 HTTP/2 的支持度也是不一样的。（幸运的是），你是可以使用[TLS工具](https://istlsfastyet.com/)的。（那么问题来了，TLS工具究竟能干啥）？功能如下：

- 验证服务器的 HTTP/2 等相关配置是否正确

- 提供一些列表，并且能够对一些特性（指的是跟性能相关的特性）进行高亮

这样一来，你不但可以快速知道服务器的性能到底咋样，而且还可以知道该服务器所支持的新特性到底有哪些。

![Is TLS Fast Yet? allows you to check your options for servers and CDNs when switching to HTTP/2.](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/f12ff2f5-9349-46a1-9c51-7a05dc906322/istlsfastyet-opt.png)

### 你开启 OCSP stapling了吗？（Is OCSP stapling enabled？）

只要你[在服务器上开启 OCSP stapling](https://www.digicert.com/enabling-ocsp-stapling.htm)，然后你就可以加速 TLS 握手过程。OCSP 协议其实是作为 CRL 协议的另一种实现而被创建出来的。OCSP 协议以及 CRL 协议在过去常被用于检查 SSL 认证证书是否被吊销（revoked）等事情。不管怎么样，OCSP 协议是不需要浏览器在下载以及搜索认证信息方面花太多的时间，因此开启 OCSP stapling，确实是可以缩短TLS握手（所需的）时间。

### 你上 IPv6 了吗？（Have you adopted IPv6 yet？）

在 [IPv4 地址空间被分配的所剩无几](https://en.wikipedia.org/wiki/IPv4_address_exhaustion)，以及各大移动运营商积极（rapidly）进行 IPv6 改造（在美国，IPv6 覆盖率已经[达到](https://www.google.com/intl/en/ipv6/statistics.html#tab=ipv6-adoption&tab=ipv6-adoption)50%）的大背景下，通过更新 [DNS 解析 IPv6](https://www.paessler.com/blog/ask-the-expert-current-status-on-ipv6) 规则的方式来确保将来不出错，这不失为一种靠谱的做法。这样一来，你就只需要保证，在进行网络环境切换的时候，能够提供 dualstack（允许 IPv4 服务、IPv6 服务并行）。（为啥呢？这是因为），毕竟在设计 IPv6 的时候，人家就没有考虑向下兼容。另外[研究也发现](https://www.cloudflare.com/ipv6/)：也是正因为 IPv6 自带 NDP 以及路由优化（的 buffer），所以才能够让网站的载入速度提升10%到15%。

### （针对 HTTP 响应头压缩的）HPACK 压缩算法，你实践了吗？（Is HPACK compression in use？）

如果你在用 HTTP/2，那么请你去复核一些细节（指的是为了减少 HTTP 响应头开销，服务器所实现 的 [HPACK 算法](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/)细节）吧。另外，考虑到出现服务器支持 HTTP/2 的时机有点晚以及（上述服务器）对 HTTP/2 规范的支持度不够等因素，所以大家还是应该以 example 的心态来看待 HPACK 实践。（至于服务器 HTTP/2 环境的检测），我们强力推荐 [H2spec](https://github.com/summerwind/h2spec)，而且从技术细节上来讲，这个工具还是蛮不错的。[服务器成功开启 HPACK ，画面是酱紫的](https://www.keycdn.com/blog/http2-hpack-compression/)。

![H2spec](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/efc02119-9155-4126-b7b9-bc83c4b16436/h2spec-example-750w-opt.png)

### 务必保证服务器的安全性（Make sure the security on your server is bulletproof）

在所有浏览器 HTTP2 协议的实现都是基于 TLS 安全协议的大背景下，假设你遇到以下问题：

- 浏览器出现安全警告，不过我想屏蔽这些安全警告，肿么办？

- 访问网页发现某些组件已失效，肿么办？

办法还是有的，且方法如下：

- [仔细检查那些与安全相关的 HTTP 头部，看是否设置正确](https://securityheaders.io/)

- [使用一些工具来排除已知漏洞](https://www.smashingmagazine.com/2016/01/eliminating-known-security-vulnerabilities-with-snyk/)

- [使用一些网站来检查证书是否失效](https://www.ssllabs.com/ssltest/)

- 确保所有（从外部引入的）插件以及埋点（js）脚本的载入都是走 HTTPS 协议的，不过这点对于跨站脚本来说，这几乎是不可能的

- 正确设置 [strict-transport-security](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) 以及 [content-security-policy](https://content-security-policy.com/) HTTP 请求头

### 用 service worker 来处理缓存问题或者将 service worker 作为网络（出现问题时）的应急方案，合适吗？（Are service workers used for caching and network fallbacks？）

你要知道，(在有网以及服务器未被优化的情况下)，从服务端拿数据的速度是不可能快于用户从本地拿缓存的速度。假设你的网站已经切到 HTTPS，我推荐你去看[Pragmatist's Guide to Service Workers](https://github.com/lyzadanger/pragmatist-service-worker)，这个指南，不但会教你如何利用 service worker cache 来缓存静态资源，从而达到存储离线资源（甚至离线页面）的目的，而且还会教你如何从用户的设备里面拿数据，也就是说，你现在是不需要通过网络的方式去请求之前的数据。当然，你也可以去看 Jake 的 [Offline Cookbook](https://jakearchibald.com/2014/offline-cookbook/) 或者去看 Udacity 开设的免费公开课[“离线web应用”](https://www.udacity.com/course/offline-web-applications--ud899)。那么问题来了，浏览器是否支持Service Workers 呢？如上所述，[service worker 已经得到浏览器厂商的广泛支持](https://caniuse.com/#search=serviceworker)（Chrome，Firefox，Safari TP， Samsung，Edge 17+），（可以这样说吧，不管浏览器对 service worker 的支持咋样），性能优化的备用方案仍然是会基于网络。（最后，大家可能会问），Service Workers 到底能不能提升性能？[答案是肯定的，Service Workers确实会提升性能](https://developers.google.com/web/showcase/2016/service-worker-perf)。

## 在做好测试的同时部署好监控

### 在浏览器开代理或者浏览器的版本比较低的情况下，你测试过吗？（Have you tested in proxy browsers and legacy browsers？）

只在 Chrome 以及 Firefox 浏览器环境下测试，是远远不够的。所以你应该去了解，在浏览器开代理或者浏览器的版本比较低的环境下，一个网站背后的运行机制。UC 浏览器以及 Opera mini 在[亚洲都占有较大的市场份额](http://gs.statcounter.com/#mobile_browser-as-monthly-201511-201611)（在亚洲的市场份额已经达到35%）。为了避免自己以后被网络坑哭，我还是建议你去[了解一下自己（所感兴趣）国家的平均网速](https://www.webworldwide.io/)。对于需要进行网络节流测试或者需要对高 DPI 设备进行仿真测试的用户来说，BrowserStack 是个不错的选择，因为[BrowserStack](https://www.browserstack.com/)不仅可以完成上述操作，而且也可以完成真机上的测试。

![k6 allows you write unit tests-alike performance tests](https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/96fa3207-4fff-4b7b-bfa0-c115062d826a/demo-unit-perf-tests.gif)

### (对性能开销大的环节),是否可以做到持续监控？（Is continuous monitoring set up？）

一般来说，[WebPageTest](http://www.webpagetest.org/) 测试环境的部署，有助于快速构建出多个测试用例。然而，对于工具（指的是能够持续监控、自动报警的工具）来说，能够带给你的，将会是更多有关于性能细节的画面。（所以你需要做的事是），在需要对（与业务相关的）指标进行监控的情况下，设置自定义的标记。当然，你也可以考虑添加[性能回归自动报警（automated performance regression alerts）](https://calendar.perfplanet.com/2017/automating-web-performance-regression-alerts/)机制（能够帮助监测指标的变化情况）。

（调研之后，你会发现）：其实是可以使用 [RUM](https://smartbear.com/learn/performance-monitoring/what-is-real-user-monitoring/) 的方式来帮忙监测性能的变化情况。至于有哪些测试工具可以被用于自动化单元测试，推荐你去使用 [k6](https://github.com/loadimpact/k6)（配合它的 API 使用，效果更佳）。当然你也可以去体验 [SpeedTracker](https://speedtracker.org/)、[Lighthouse](https://github.com/GoogleChrome/lighthouse) 以及 [Calibre](https://calibreapp.com/) 工具



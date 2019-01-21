# 前端性能优化の备忘录 2019 版

我们先来快速过一下今年的前端性能优化の备忘录：

* 愿景：让网站在 2019 年跑得更快！
* 内容：涵盖了目前你所需要知道的关于打造极速体验的所有内容
* 性质：一年发布一次，从 2016 年起开始更新

**web 性能这件事，处理起来非常棘手，不是吗？我们是否真的了解 web 性能的目前现状**以及是否真的知道啥是性能瓶颈呢？JavaScript 文件的体积过大、web 字体的下载速度过慢、图片的尺寸过大、渲染的速度过慢等因素，是不是诱发性能瓶颈的原因呢？这些问题是否值得你去实践 [tree-shaking](https://doc.webpack-china.org/guides/tree-shaking/)、[scope hoisting](https://zhuanlan.zhihu.com/p/27980441)、[code-splitting](https://doc.webpack-china.org/guides/code-splitting/) 以及其它那些比较好的载入技巧呢？比如 [intersection observer](https://www.w3.org/TR/intersection-observer/)、[server push](https://tools.ietf.org/html/rfc7540#section-8.2)、[clients hints](https://github.com/igrigorik/http-client-hints)、[HTTP/2](https://http2.github.io/)、[service workers](https://www.w3.org/TR/service-workers-1/) 以及其它不可思议的 workers。（抛开上面提到的一些问题，我觉得）最为重要的一点，**大家是否知道性能优化从哪里开始**以及是否知道如何树立长期的性能优化意识？

在以前，大家对性能优化总是摆出一副“秋后算账“的样子。（举个栗子），就算它的工作内容只是对资源进行压缩、合并、优化或者对服务器`配置`文件（的配置项）做一些微调，（万万没想到的是），大家还是习惯性地把（针对性能优化的）排期给拖到项目末尾。现在我们再回过头看，性能优化似乎变得更加重要啦。

对性能进行优化，不仅仅只是出于技术层面的忧虑：真的是因为性能优化非常重要，尤其是在（我们需要）把性能优化环节给集成到工作流，且需要根据性能优化环节所给出的建议，来提示（我们）进行决策的情况下，性能优化就会显得尤为必要。**另外性能必须是可以不断被量化、被监控以及被优化的**。现在随着 web 应用的复杂性日益增加，（自然而然）就会给性能指标的分析带来新的挑战，为啥呢？这是因为性能指标之间的差异性非常大。不过这也取决于使用的设备、浏览器、协议、网络类型以及其它能够对性能产生影响的潜在因素，包括 CDN、ISP、缓存、代理、防火墙、负载均衡以及服务器。

所以，在（网站）项目刚启动时，我们就应该（对网站）采取（一些有助于网站）性能提升的手段，并且该过程一直要持续到网站的最终版本发布上线。在这段时间里，假设我们需要牢记这些优化手段，并且希望针对这些优化手段做一下总结，内心 OS：天啦噜，这总结应该长啥样呀？往下阅读，你就会发现一篇非常客观的年度总结：前端性能优化の备忘录 2019 版，今年这一版的内容讲的是对一些问题的后续跟进。比如，如何让网站的响应速度更快、交互体验更顺畅以及如何做才不会让网站耗尽用户的带宽。

## 目录

* 准备工作：设立目标以及确立指标
* 设立合理的目标
* 环境搭建
* 资源优化
* 构建优化
* Delivery 优化
* HTTP/2
* 测试和监控
* 快速制胜
* 下载前端性能优化の备忘录（PDF 版本、Apple Pages 版本，Word 版本）
* Off We Go！

（当然，你也可以只[下载前端性能优化の备忘录 PDF 版本](https://www.dropbox.com/s/8h9lo8ee65oo9y1/front-end-performance-checklist-2018.pdf?dl=0)（166 KB）或者[去 Apple Pages 下载前端性能优化の备忘录](https://www.dropbox.com/s/yjedzbyj32gzd9g/performance-checklist-1.1.pages?dl=0)（275 KB）或者下载 [.docx 文件](https://www.dropbox.com/s/76b3yzexqdwsg65/performance-checklist-1.2.docx?dl=0)。让我们一起愉快的优化前端性能吧！）

## 准备工作：设立目标以及确立指标（Getting Ready: Planning And Metrics）

虽说微优化（Micro-optimization）对稳定性能很有帮助，不过（我觉得大家）还是应该在思想层面上设立一个明确的目标，这是因为目标一旦被量化，你在项目期间作出的所有决定都会受到影响，所以说设立明确的目标才至关重要。（至于如何解决前端性能问题），这里有很多不同的优化模型（model），并且我们都是按照自己的理解来编排下面的内容，所以（在使用上述优化模型时），你自己只需要确定性能优化手段的优先级。

### 树立性能优化意识文化（Establish a performance culture）

对于很多团队来说，（他们团队里面的前端工程师）不但知道哪些问题可能会导致潜在的性能问题，而且还知道应该使用哪种载入技巧来解决。不过（这并没什么卵用，为啥这样说呢？这是因为），只要有人对这种意识文化不认同，那么你做的每一个决策，都会变成部门撕逼的导火索，最后导致团队四分五裂、各自为政。所以，你需要把业务方拉进来，并且得到他们的支持。为了得到他们的支持，你需要设立一个研究课题，研究他们所关心的速度收益指标（speed benefits metrics）以及关键绩效指标（KPI）。

（在性能优化的这件事上），只要开发或者设计那边有一方跟业务或者营销团队意见不合，那么性能优化这条路将不会走的太远。所以说，分析客服系统（所收集到的）常见投诉，并且去思考如何提升性能，还是会在（一定程度上）帮你缓解一些常见的性能问题。

现在不管你用的是手机还是台式机，这两种设备都是可以跑性能测试的，而且（性能测试工具）还能够针对这两种设备定量的给出测试结果。在存在真实数据的情况下，这些（指的是测试结果）都将会有助于你建立起以公司为样本数据的研究课题。另外，利用好 [WPO Stats](https://wpostats.com/) 上的数据，包括实验得出的数据以及案例给出的数据。（为啥这样说呢？这是因为）这些数据能够在（一定程度上）提升你对业务的熟悉程度，这其中也包括了帮助你去理解性能（为啥如此）重要的背后原因以及性能对用户体验、业务指标的影响。光嘴上说性能优化很重要，这是远远不够的。还是需要你去制定一些易量化、易追踪的目标，并且一直执行下去。

至于如何做？Allison McKnight 在她的演讲（[树立长期的性能优化意识](https://vimeo.com/album/4970467/video/254947097)）中，分享了一个比较全面的案例，内容是关于她如何帮助 Etsy 树立起性能优化意识文化（[幻灯片](https://speakerdeck.com/aemcknig/building-performance-for-the-long-term)）。

[![brad-perf-budget-builder](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/7191d628-f0a1-490c-afca-c8abcdfd4823/brad-perf-budget-builder.png)](http://bradfrost.com/blog/post/performance-budget-builder/)

### 目标：网站总是要快人一步（Goal: Be at least 20% faster than your fastest competitor）

[心理学研究](https://www.smashingmagazine.com/2015/09/why-performance-matters-the-perception-of-time/#the-need-for-performance-optimization-the-20-rule)表明，如果你想让用户感觉你网站（的载入速度）就是比其它网站快，那么你网站（的载入速度）至少要比其它网站快 20% 才行。另外，研究主要的竞争对手，比如收集他们在手机以及台式机上的一些表现以及他们设置的一些阈值，（在一定程度上）能够帮你超越竞争对手。为了让上述结果更加精确，（你需要做的事是），首先去查看分析报告，得出用户的用户行为。对啦，如果你的分析报告只是用于测试，那么你也可以只模拟出 90% 的场景。

为了更好地了解竞争对手的表现，你可以[使用 Chrome UX Report](https://web.dev/fast/chrome-ux-report)（CrUX，提供现成的 RUM 数据集，[视频的介绍](https://vimeo.com/254834890)可以去看 Ilya Grigorik 的视频）、[Speed Scorecard](https://www.thinkwithgoogle.com/feature/mobile/)（还能对网站的收益情况进行评估）、[真实环境下网站用户体验的比较结果](https://ruxt.dexecure.com/compare) 或者 [SiteSpeed CI](https://www.sitespeed.io/)（基于 [synthetic testing](https://raygun.com/blog/synthetic-testing/)）。

**注意**：倘若你有在用 [Page Speed Insights](https://developers.google.com/speed/pagespeed/insights/) （插一句，该工具并没有过时）的话，你就可以针对特定的页面来获取 CrUX 与性能有关的数据，而不仅仅是对数据的聚合。另外，这些数据对资源（如 “登录页” 或者 “产品列表” ）性能指标的设立会很有帮助。并且如果你准备用 CI 来测试这些资源的性能开销，你需要确保你的测试环境能够满足 CrUX，前提是你有用 CrUX 来设定目标（感谢 Patrick Meenan ！）。

然后去收集数据，并将收集的数据做成[表格（spreadsheet）](http://v3.danielmall.com/articles/how-to-make-a-performance-budget/)，（并且根据自身的情况），剔除部分（20%）数据，这样你就可以设立自己的小目标啦，例如完成一篇关于[性能预算（performance budgets）](http://bradfrost.com/blog/post/performance-budget-builder/)的分析报告。现在，你就可以开启你的性能测试之旅啦。假设你记住了（测试过程中所花费的）性能开销，然后想通过尝试脚本最小化的方法来让交互时间的数据更好看，接下来你会发现你已经踏上了性能优化的“不归路”（a reasonable path）。

需要给出一些资料（resources）才能开始？

* Addy Osmani 之前曾写过一篇文章，里面详细介绍了[如何开始性能预算](https://medium.com/@addyosmani/start-performance-budgeting-dabde04cf6a3)、如何量化新特性所带来的影响，以及在性能开销超出预算之后，（后续削减性能开销的操作）从哪里开始
* Lara Hogan 写的那篇关于[如何处理设计与性能预算之间的关系](http://designingforperformance.com/weighing-aesthetics-and-performance/#approach-new-designs-with-a-performance-budget) 的博文，给了设计师很多非常有用的建议
* Jonathan Fielding 的 [Performance Budget Calculator](http://www.performancebudget.io/)、Brad Frost 的 [Performance Budget Builder](https://codepen.io/bradfrost/full/EPQVBp/) 以及 [Browser Calories](https://browserdiet.com/calories/) 都能够有助于静态资源性能预算的统计（感谢 [Karolina Szczur](https://medium.com/@fox/talk-the-state-of-the-web-3e12f8e413b3) 的提醒）
* 此外，可以通过使用仪表盘（能够以图表的形式来呈现资源经构建之后的体积）的方式，来实现对网站的性能开销以及当前网站性能的可见。有很多工具可以帮你做到这一点：[SiteSpeed.io dashboard](https://www.peterhedenskog.com/blog/2015/04/open-source-performance-dashboard/)（开源）、[SpeedCurve](http://speedcurve.com/) 以及 [Calibre](https://calibreapp.com/) 只是其中的很小一部分。另外，你可以在 [perf.rocks](http://perf.rocks/tools/) 这个网站里面找到更多工具。

一旦你做好了网站性能的预算，就可以把这些预算连同 [Webpack Performance Hints 以及 Bundle 体积](https://web.dev/fast/incorporate-performance-budgets-into-your-build-tools)、[Lighthouse CI](https://web.dev/fast/using-lighthouse-ci-to-set-a-performance-budget)、[PWMetrics](https://github.com/paulirish/pwmetrics) 或者 [Sitespeed CI](https://www.sitespeed.io/) 都整合进自己的构建流程。以便于在你提 pull request 的同时强制执行“预算”操作，并且还会在 PR 所提供的评论处给出历史得分记录。如果你需要一些定制化的东西，可以使用 [webpagetest-charts-api](https://github.com/trulia/webpagetest-charts-api)（属于 endpoints API 范畴，根据 WebPagetest 的测试结果来绘制图表）。

例如，就像 [Pinterest](https://medium.com/@Pinterest_Engineering/a-one-year-pwa-retrospective-f4a2f4129e05) 一样，你自己可以创建一个自定义的 `eslint` 规则 — 禁止导入那些自身所依赖的东西过多（dependency-heavy）的文件夹以及文件，因为如果不那么做的话，bundle 文件会变得很臃肿。另外，可以设立一个能够共享给整个团队的“安全” package 列表。

除了性能预算（performance budgets）这个因素要考虑之外，还需要去思考用户提的一些需求（critical customer tasks），为啥呢？这是因为，这些需求对业务来说，最为有利。对啦，**针对一些关键性的操作**，你还要去设置并且讨论出合理的**时间阈值**，还有就是，（针对页面啥时候处于）“UX ready 状态”，可以在你统计的时候，使用一些大家都认可的标识 - user timing marks。另外在很多情况下，用户的一些操作行为会涉及多个部门。所以，就（用户操作所带来的）时间开销是否合理这个问题来说，各个部门如果能抱成一团的话，那么将会有助于避免一些问题的发生，比如，在将来的某个时候，各个部门为了性能问题来“撕逼”（discussions）。最后，在你引入资源以及新增功能的时候，不但要确保上述操作所带来的开销，是可以看得见摸得着的，而且还要确保大家都能够知道这些开销是怎么来的。

另外，正如 Patrick Meenan 所建议的那样，在设计（资源载入环节）的过程中，**知道如何规划好（plan out）资源之间的载入顺序以及知道这些顺序之间的权衡利弊（trade-offs），真的是很有必要**。（为啥呢？试想一下），假设在项目开始之前，你就针对一些比较重要的环节（part）来排优先级，并且还确定了这些环节的出场顺序。（那么，自然而然），你也就知道有哪些环节可以被 delay 啦。顺利的话（Ideally），也是能够从这些环节的出场顺序看出（reflect ） CSS 和 JavaScript 的引入顺序。如果真是酱紫的话，在今后的项目构建过程中，CSS 以及 JavaScript 的处理，将会变得容易多啦。当然，你也可以去考虑究竟啥样的视觉体验才是介于好与坏之间的“中间状态”，前提是，你的页面正处于 loaded 状态（比如，页面的 web 字体还未载入）。

规划、规划、规划，重要的事情说三遍！为啥呢？这是因为，对性能提前做好规划，可以让你（在真的遇到性能问题的时候）做到“手到擒来”，并且最终还可能会演变成快速制胜的法宝。不过话说回来，在没有做好规划、（公司层面的）优化目标被设置的不合理的背景下，还能够把性能的优先级摆在第一位，其实是很难的。

[![The difference between First Paint, First Contentful Paint, First Meaningful Paint, Visual Complete and Time To Interactive](https://i.vimeocdn.com/video/675362019.jpg)](https://player.vimeo.com/video/249524245)

### 选择正确的指标（Choose the right metrics）

[并不是所有指标的重要性都一样](https://speedcurve.com/blog/rendering-metrics/)。（那么问题来了，究竟）研究哪些指标，对你的应用程序来说最为重要：通常，指标的选择与开始渲染产品最关键像素（most important pixels）的速度以及为这些渲染像素提供输入响应（input responsiveness）的速度有关。这些知识点将会助力你设立最佳的优化目标。

不管怎样，与其关注整个页面的加载时间（例如 onLoad 时间和 DOMContentLoaded 时间），倒不如优先考虑加载那些用户所能够感知到的内容。这就意味着，需要去关注一些其它的指标。实际上，在选择正确指标的过程里面，输赢并不会那么明显。

基于 Tim Kadlec 的研究以及 Marcos Iglesias [演讲](https://docs.google.com/presentation/d/e/2PACX-1vTk8geAszRTDisSIplT02CacJybNtrr6kIYUCjW3-Y_7U9kYSjn_6TbabEQDnk9Ao8DX9IttL-RD_p7/pub?start=false&loop=false&delayms=10000&slide=id.g3ccc19d32d_0_98)中的笔记来看，之前的指标可以被分为几个组。通常，我们需要使用所有这些指标来全方位了解性能，并且在一些特殊的情况下，一些指标会比其它指标更重要。

* Quantity-based metrics 衡量的是请求数、权重以及性能得分。适合用来告警（raising alarms）以及监控一段时间内的变化情况，但不适合用来理解（understanding）用户体验。
* Milestone metrics 会在页面加载过程所出现的生命周期里面使用一些状态（states），例如 Time To First Byte 以及 Time To Interactive。适合用来描述（describing）用户体验以及监控，但不适合用来了解 milestone 之间所发生的事情
* Rendering metrics 能够估算出内容呈现的速度（例如，Start Render time、Speed Index）。适合用来衡量以及调整渲染性能，但不适合用来确定重要内容所出现的时机以及与之交互的时机
* Custom metrics 为用户提供特定的自定义事件，例如，Twitter 的 [Time To First Tweet](https://blog.alexmaccaw.com/time-to-first-tweet) 以及 [PinnerWaitTime](https://medium.com/@Pinterest_Engineering/driving-user-growth-with-performance-improvements-cfc50dafadd7)。适合用来精确地描述（describing）用户体验，但不适合用来扩展指标以及与竞争对手的比较

为了完成下面这幅图（To complete the picture），我们通常会在所有这些分组里面寻找有用的指标。通常，是那些最与之相关的指标：

* [First Meaningful Paint](https://developers.google.com/web/tools/lighthouse/audits/first-meaningful-paint)（FMP）<br/>描述的是，页面从开始载入到基本内容出现的那一段时间，从而了解服务器吐数据的速度。出现 FMP 时间较长的情况，通常表示 JavaScript 阻塞了主线程，但也可能与后端/服务器问题有关
* [Time to Interactive](https://calibreapp.com/blog/time-to-interactive/)（TTI）<br/>到了这个节点，说明布局已经处于稳定状态、关键字体已处于可见状态以及主线程已经充分能够处理用户输入 — 基本上，这个时间标志着用户可以和 UI 进行交互。是了解用户在没有延迟的情况下需要等待多长时间才能够使用网站的关键指标
* [First Input Delay](https://calibreapp.com/blog/time-to-interactive/)（FID）或者 Input responsiveness <br/>描述的是，从用户第一次开始与你的网站进行交互到浏览器真的能够对该交互做出响应的那一段时间。TTI 所补充的内容就非常好，因为它填补（describes）了下面这幅图所缺失的内容：用户与网站进行交互时，这里面会发生什么。只用作 RUM（Real User Monitoring）指标。需要在浏览器里面测量 FID，这里有一个 [JavaScript 库](https://github.com/GoogleChromeLabs/first-input-delay)
* [Speed Index](https://dev.to/borisschapira/web-performance-fundamentals-what-is-the-speed-index-2m5i) <br/>描述的是，填充（populated）页面内容的这个过程，在视觉上给人所呈现出的速度；分数越低越好。Speed Index 分数是根据完成填充页面进度的速度来计算的，（你要记住），它不过是个通过计算手段所得到的数值，而且这个值容易受视窗大小的影响，所以你需要针对你的那些目标受众群体，来制定一系列与之相匹配的测试配置（感谢 [Boris](https://twitter.com/borisschapira) ！）
* CPU time spent <br/>用于表明主线程在处理负载时的繁忙程度，它为我们展示了主线程被阻塞的频率以及时间，这里面包括绘制（painting）、渲染（rendering）、脚本的编写（scripting）以及脚本的载入（loading）。主线程被长时间阻塞（High CPU time），表明页面出现了卡顿，比如当用户的操作与响应之间存在明显的延迟。有了 WebPageTest，你就可以选择 [Chrome 选项卡上的 "Capture Dev Tools Timeline"](https://deanhume.com/ten-things-you-didnt-know-about-webpagetest-org/)，来揭露主线程所出现的故障，因为 “Capture Dev Tools Timeline” 会在所有的设备上使用 WebPageTest
* [Ad Weight Impact](https://calendar.perfplanet.com/2017/measuring-adweight/)<br/>如果你的网站是依靠广告来盈利的话，那么沿途记下（track）那些与广告相关的代码权重，这非常有用。这里推荐 Paddy Ganti 所写的[脚本](https://calendar.perfplanet.com/2017/measuring-adweight/)，该脚本会首先给出两个 URL（一个正常，一个会屏蔽广告），然后会弹窗提示有视频内容生成，该视频内容由 WebPageTest 生成，用于对比两个 URL 所获取的内容。最后会把这些有差异的内容（delta）做成报告
* Deviation metrics <br/>正如 [Wikipedia 工程师所指出的那样](https://phabricator.wikimedia.org/phame/live/7/post/117/performance_testing_in_a_controlled_lab_environment_-_the_metrics/)，根据你最后结果存在差异化数据的多少，可以得出你工具的可靠性怎么样以及你后面需要把多少精力放在异常数据上。出现较大的数据差异，则表明设置需要进行调整。另外它还能够帮助你理解为啥有些页面就很难被测量准确，比如由于第三方脚本导致的显著差异。在浏览器推出新版本时，沿途记下（track）浏览器版本的这种做法，不失为一种好的做法，因为这样就可以在浏览器推出新版本时了解性能突变的原因
* [Custom metrics](https://speedcurve.com/blog/user-timing-and-custom-metrics/) <br/>是根据你的业务需求以及用户体验（customer experience）来定义。它需要你自身能够分辨出关键像素（important pixels）、关键脚本（critical scripts）、必要的 CSS（necessary CSS）以及相关资源，并且能够衡量出这些内容在被交付（delivered）给用户时的速度。关于这一点，你可以监控 [Hero Rendering Times](https://speedcurve.com/blog/web-performance-monitoring-hero-times/) 或者使用 [Performance API](https://css-tricks.com/breaking-performance-api/)，从而为你重要的业务事件标记特定的时间戳。另外，你可以在测试结束的时候，通过执行任意 JavaScript 代码的方式来[获取 WebPagetest 所给出的 custom metrics](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/custom-metrics)。

（至于每一个指标都讲的是啥），Steve Souders 对此有[详细的介绍](https://speedcurve.com/blog/rendering-metrics/)。另外，需要着重注意的是，虽说 Time-To-Interactive 指标的测量工作，可以在所谓的实验室环境中，通过运行自动统计（automated audits）的方式来进行，但是 First Input Delay 指标体现的是真实用户体验 — 真实用户所感受到的明显延迟。一般来说，经常测量并沿途记下（track）这两指标，不失为一个两全其美的办法。

根据应用程序所使用的场景（context），在首选指标的选择上，结果可能会有所不同：比如，对于 Netflix 的视频内容界面（TV UI）来说，[重要键的响应输入、内存的使用情况以及 TTI](https://medium.com/netflix-techblog/crafting-a-high-performance-tv-user-interface-using-react-3350e5a6ad3b)更重要，而对于 Wikipedia 来说，呈现[第一次或者最后一次视觉的变化以及 CPU time spent 指标](https://phabricator.wikimedia.org/phame/live/7/post/117/performance_testing_in_a_controlled_lab_environment_-_the_metrics/)更重要。

**注意**：FID 指标以及 TTI 指标，这两者指标都不会考虑滚动这种交互行为；另外，滚动这种交互行为可以独立进行 — 因为它不在主线程所管辖的范围里面，对于很多主打内容消费的网站来说，这些指标就显得不那么重要（感谢，Patrick！）

[![User-centric performance metrics provide a better insight into the actual user experience. First Input Delay (FID) is a new metric that tries to achieve just that](https://res.cloudinary.com/indysigner/image/fetch/f_auto,q_auto/w_2000/https://cloud.netlifyusercontent.com/assets/344dbf88-fdf9-42bb-adb4-46f01eedd629/5d80f91c-9807-4565-b616-a4735fcd4949/network-requests-first-input-delay.png)](https://twitter.com/__treo/status/1068163152783835136)

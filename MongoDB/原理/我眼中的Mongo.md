**MongoDB“牛”吗？**

显然论交易型处理它难以比肩传统关系型数据库，谈缓存处理未必完胜Redis，看大并发写目测难以媲美Cassendra … …
但是，我却说MongoDB“牛”!
且不论上述几点在最新版本中是否依然如此（在MongoDB多引擎下有太多变数），MongoDB背后代表了一种“化繁为简”的业务驱动开发模式，让开发能够更加易于快速适应业务变化，让思考模式在多次迭代中进化，就这一点，即可在众多业务场景中立于不败之地。从商业模式的角度来看，现代企业的应用随着业务模式变更必须快速调整，这本身就是一个多次快速迭代的过程，与MongoDB设计思想是不谋而合的。所以业务驱动，迭代设计思路才是MongoDB精华之一。

记得初来MongoDB，来到一家互联网公司做个介绍，一位英国留学归来的小伙子貌似对于MongoDB颇有微词，指着我大声质问：“我不觉得MongoDB有怎么好，你能告诉我有哪个应用只能用MongoDB吗？”坦率的说，除了mainframe之外，我自认没有开放平台上没有哪个应用只能用哪个数据库，只有根据业务的需求来定夺更合适的架构。不得不承认，在某些架构“不会发生变化”，结构简单的场景中，MongoDB未必是最佳的选择。但是大千世界中更多的是一个“变”字，所以想来这也就是为何MongoDB这么一个后来者会如此火爆的占据db engine排名第四的重要原因。事实上即使是开发者也需要把眼光放的远些，多多换位思考，不一味拘泥以某一技术，这样才能站得更高，看的更远。毕竟在国内开发很难像国外一样，有了3个小孙子白了头发还能一个萝卜一个坑继续搞自己的代码的 … …

当然，MongoDB第二个精华显然是路人皆知的架构 —— 复制集+分片，很多早年初用MongoDB的忠实粉丝就是冲着这点而去。只是技术的差距不同于设计思路的差距，是很容易被模仿并改进的，所以在我理解中从大的框架角度来看可谓优势仍在，但后来者甚多。

如果说前两点从架构设计的角度来说都已经安身立命了，那么第三点精华可谓前途无限，那就是MongoDB的插拔式引擎，在我的眼里，这是MongoDB的大蓝图之一，也是能够真正贯彻执行第一点精华的重要手段。
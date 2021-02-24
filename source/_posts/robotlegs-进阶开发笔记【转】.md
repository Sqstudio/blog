---
title: Robotlegs 进阶开发笔记【转】
tags: []
id: '646'
categories:
  - - Rebotlegs
date: 2012-04-25 16:19:18
---

【本文属于转载，阅读原文：[http://www.wersling.com/?p=675](http://www.wersling.com/?p=675)】 [Robotlegs](http://robotlegs.org/)是一个很独特的MVCS框架，2年前我开始使用它，并用于一些项目中，那个时候Robotlegs还是bate版本，现在已经更新到1.4，更多的人已经开始学习它并用到项目中。在学习和使用过程中，我整理和体会到一些有用的知识点，希望和正在学习和使用Robotlegs的同学分享。 因为是进阶内容，因此，建议对Robotlegs还在入门阶段或没有使用它到项目中的同学先看看这些内容，会对你更好的理解后面的内容有帮助。当然，如果你对Robotlegs已经熟悉，可以跳过。

*   入门的同学建议通读[eidiot](https://github.com/eidiot)翻译的[Robotlegs最佳实践](https://github.com/robotlegs/robotlegs-documentation/blob/master/best-practices-zh-cn.textile)
*   [了解IOC](https://github.com/tschneidereit/SwiftSuspenders)（控制反转）在Robotlegs中是如何使用的，可以学习william的[SwiftSuspenders 1.6 浅出深入](http://sswilliam.blog.163.com/blog/static/189696383201175114948372/)
*   看一些[Robotlegs Demo](https://github.com/robotlegs/robotlegs-demos-Bundle)，看看它是如何工作的

恩，如果你有所了解了，我们开始后面的内容。 **Robotlegs代码包结构组织：**

> 1.  \[domain.lib\]
> 2.      various utilities
> 3.  \[domain.project\]
> 4.      \[projectmodule\]
> 5.          \[model\]
> 6.              \[events\]
> 7.              \[vo\]
> 8.              ProjectModuleStateModel
> 9.          \[view\]
> 10.              \[events\]
> 11.              \[renderers\]
> 12.              \[skins\]
> 13.              MyProjectModuleView
> 14.              MyProjectModuleViewMediator
> 15.          \[controller\]
> 16.              \[startup\]
> 17.              MyProjectModuleActionCommand
> 18.          \[service\]
> 19.              \[helpers\]
> 20.              MyProjectModuleService
> 21.              IProjectModuleService
> 22.          \[signals\]
> 23.      \[projectmodule\]
> 24.          \[model\]
> 25.              \[events\]
> 26.              \[vo\]
> 27.              ProjectModuleStateModel
> 28.          \[view\]
> 29.              \[events\]
> 30.              \[renderers\]
> 31.              \[skins\]
> 32.              MyProjectModuleView
> 33.              MyProjectModuleViewMediator
> 34.          \[controller\]
> 35.              \[startup\]
> 36.              MyProjectModuleActionCommand
> 37.          \[service\]
> 38.              \[helpers\]
> 39.              MyProjectModuleService
> 40.              IProjectModuleService
> 41.          \[signals\]
> 42.      ...

  这是[Joel Hooks](http://knowledge.robotlegs.org/users/117676)建议的包结构，我作一些修改和说明：

*   这是一个多模块情况的包结构，如果是单模块，可以少去projectmodule这一层。
*   \[signals\]包不是必须的，除非你用到了[Signals](https://github.com/robertpenner/as3-signals)。
*   把每个模块要都用到的公共代码放到\[domain.lib\]下。模块和模块内代码保证独立，不会相互使用（引用，实例化）。
*   StartupCommand 内代码建议以MVC结构分离。如果一个模块中StartupCommnad内有过多代码，建议把它拆分成 ModelStartupCommand，ControllerStartupCommand和ViewStartupCommand，这样职责更清晰。
*   \[controller\]和\[view\]中，如果有很多类，建议按照功能建立子目录
*   这些目录看上去如此之多，我可不想一个一个创建，因此，我写了一个[Ruby小脚本](https://github.com/wersling/manaca/tree/master/tools/script)，用于创建这些目录和基础代码，有兴趣的可以看看。

**边界与约定：**

*   避免在Proxy中注入其他Proxy和Service，尤其不要侦听他们的事件。
*   避免Proxy采用单例模式，而是采用Robotlegs的injector.mapSingleton方法，单例意味着任何类成员都可以访问和修改，这会增加代码的复杂度和维护成本。
*   非必要，避免将View或Mediator存入Proxy。
*   建议在Mediator中只获取Proxy的数据，而不设置或管理数据。
*   PopupWindow（如果它被添加到stage）建议在Mediator中实例化，这有利于手动构造Mediator和侦听事件。
*   非必要，避免使用viewMap。它会使View绕过Mediator而直接与数据层交互。
*   避免在Command中注入一个View或Mediator。
*   Service的方法建议在Command中调用，而不是在Mediator中，这样可以更好的复用。当然，一个简单的方法是可以接受的。
*   最后一条：Service只是负责将加载的数据通知（dispatch event）出去，自己并不存储数据。

**多模块：** Robotlegs提供一个多模块工具：[robotlegs-utilities-Modular](https://github.com/wersling/robotlegs-utilities-Modular)。可以方便的进行模块和模块之间的通信。它看上去十分优雅，用起来也十分方便。下面是几点要注意的地方：

*   尽量将模块内事件和模块间的事件分开，不要混杂着用，我定义了一个[IModuleEvent](https://github.com/wersling/robotlegs-utilities-Modular/blob/master/src/org/robotlegs/utilities/modular/base/IModuleEvent.as)接口，用于区分它们。
*   所有需要公用的对象或类（VO，Proxy，Service）都在主模块的Startup时注入，这样子模块都可以很方便的用到。

**开发效率：** Robotlegs的底耦合和清晰的职责被人称赞，实现一个功能需要的类文件之多也使其他MVC框架望其项背。^\_^’ 因此在开发过程中，也许你会烦躁那些重复的操作——创建众多的类文件。

*   采用[Signals](https://github.com/robertpenner/as3-signals)可以有效减少View-Mediator的事件对象，其效率高于事件机制。[CommandSignals](https://github.com/robotlegs/signals-extensions-CommandSignal)也可以用Signals驱动，但是不会节省你的类文件数量。
*   采用代码模板功能快速生成代码。如果你连这个模板都不想写，有热心人已经帮你创建了不少：[Flash Builder](http://knowledge.robotlegs.org/discussions/solutions/8-flashbuilder-burrito-code-templates-inject)，[FTD](http://doesflash.com/2010/11/robotlegs-fdt-templates/)，[FlashDevelop](http://www.zedia.net/2010/robotlegs-templates-for-flashdevelop/)。
*   Robotlegs的[状态机（StateMachine）](https://github.com/robotlegs/robotlegs-utilities-StateMachine)，和PureMVC的状态机类似。在处理带有状态或流程的操作中很有用，比如与服务器连接状态、多步骤的表单提交、带条件判断的问卷调查等等。
*   不一定在项目过程中完全贯彻使用MVC框架，尤其是在WebGame项目中，可以把功能单一的部分做成一个组件，对外提供接口就可以。比如场景渲染引擎、寻路算法、播放器等。看上去就像在使用一个第三分工具库一样。
*   由于Robotlegs职责分明，因此可以根据MVCS的来分工：
    *   由团队中一部分人来维护服务层（Service）代码，专门处理数据加载和与服务器通信代码编写。
    *   一部分人处理公共数据层代码（Proxy）
    *   一部分人处理界面层代码（View）
    *   一部分人处理业务逻辑，连接数据与界面（Mediator，Command）。这样带来的好处在于一个人只要专注一个部分，因此有更多尽力关注稳定性和性能；坏处是有时会觉得比较无聊，因此时不时要切换着来做不同的部分。小项目中，也许就一个人，是没有办法这样实施的，但是项目大了，完全有必要尝试这样的方式。最后还啰嗦下，很多新人刚刚进入项目，如果对Robotlegs不了解，让他从View层开始开发，有利于学习和确保他更快融入团队。

**内存管理（GC）：** Robotlegs的低耦合性带来的就是优雅的GC，只要是遵循以下原则，内存泄露可以很好的解决，哪怕在复杂的项目也可以：

*   严格遵守前面提到的【边界与约定】。
*   为每一个View实现一个dispose方法（确保这个方法真的可以GC所有View内实例和引用），并在Mediator的onRemove方法中调用。
*   每个模块实现ShutdownCommand，进行与StartupCommand相反的操作。
*   为每个Actor（Proxy和Service）实现dispose方法，并在ShutdownCommand时调用。

**性能：** 很多框架用到后面，都会带来或多或少的性能问题，Robotlegs也不例外。建议试试我提出的几个方法：

*   只要执行一次的Command设置oneshot = true。示例：
    
    1.  commandMap.mapEvent("eventType", MyCommand, EventClass, true);
    
    _复制代码_
    
*   在添加大量Sprite到舞台前，可以设置：mediatorMap.enabled=false；。完成后再设置为true，这样可以避免MediatorMap侦听到ADDED\_TO\_STAGE事件后频繁进行不必要的操作。也可以使用[robotlegs-utilities-LazyMediator](https://github.com/eidiot/robotlegs-utilities-LazyMediator)来解决这个部分的性能问题。
*   将一些对性能要求极高的部分写成独立的组件。

必须承认，Robotlegs的学习成本要比PureMVC等框架要高，也复杂一些。还有很多人不喜欢Inject方式，但是我要说，Robotlegs 绝对值得你去尝试。在项目开发中不断的学习和探索，将是一个十分有趣的过程。衷心希望有更多人学习和使用Robotlegs，也分享更多的经验。
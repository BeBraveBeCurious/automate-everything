# 4. 流程自动化

前端从诞生依赖可以说经历了一个从不规范到规范，再到自动化的一系列过程。最开始的时候没有前端这个领域，所有的前端职责都是由服务端来代替完成，当时的状况是非常混乱的，派别丛生，浏览器厂商规范不统一，模块机制混乱，代码风格写法各自为派，真有点群雄逐鹿的感觉。从HTML5，CSS3，ES6（536）的发布开始，前端已经趋于规范化。浏览器的规范趋向一致，微软的浏览器也逐渐跟上了步伐，浏览器兼容性问题向前迈进了一大步。如今前端开发的过程也越来越规范，越来越有规律可循。因此大家在想这些有规律的东西能不能够实现自动化（程序员最喜欢偷懒），能不能将枯燥无味的工作交给电脑来做，减少重复的工作，提高工作效率。 答案当然是可以的，而且目前业内已经有非常多的成功案例。从facebook的Waitir，google的自动化流程系统，到国内阿里的def，再到小公司使用的gitlab ci，发现很多公司都已经开始了流程自动化的探索。本章我会先讲述前端的工作流程是怎样的，然后对前端工作流程进行分析，分析哪些任务可以自动化，实现自动化的思路是什么，最后我会讲述如何搭建一个自动化的小平台。

## 前端工作流程

让我们开始之前，先来看下我们目前前端的工作流程是怎么样的，我分享的只是我个人的工作流程，不同公司和个人可能略有不同，但总的思路应该是差不多的，请不要太介意。

### 阶段一 需求产生 - 准备开发

这个阶段又可以分为如下三个小阶段：

#### 需求评审

正常情况下，从需求产生到准备开发，应该有一个需求评审会，这时候相关开发和产品经理会聚集在一起，商讨需求是如何产生的（敏捷开发的需求通常是从客户的反馈中产生的），我们做的功能有没有解决客户的问题，我们对需求的理解有没有偏差和需求的优先级等。经过彻底充分的讨论之后，如果需求理解没有问题，并且需求确实可以解决客户的问题，我们会为工作安排时间和优先级，将task列到看板（同时列到系统和白板上），并每天追踪更新。

> 理解产品经理的真正意图，明白需求产生的背景

#### 拆分组件和模块

前端目前比较推荐的工作方法是组件式开发，即将页面拆分成足够粒度的小组件和模块。由单独的人负责相关独立模块和组件的开发，做到组件和模块的复用，这个已经在第二章讲过了，不在此赘述。

#### 建立分支

目前大多数互联网公司的版本管理工具都是使用git，包括我们的公司，而且公司内部通常也有自己的git flow，我们做新功能的时候通常会建立一个feature分支，开发完毕后合并到release分支等待测试和发布。当然就算你是SVN思路也是一样的。

### 阶段二 开始开发 - 提交测试

#### 写单元测试

这里通常有两个比较常见的问题。问题一是能不能不写单元测试，或者说不写单元测试有什么不好的影响？第二个问题是为什么要先写单元测试而不是先写代码？我们对这两个问题一一进行解答。先说第一个问题，其实不写单元测试是很容易做到的，就好像我们不努力锻炼身体一样简单，但是当我们面对自己的一大块腹肌的时候，是不是有那么一丁点儿后悔呢？单元测试也是一样，当你开始写的时候，没有任何收效，据统计写单元测试的时间要高于写代码的时间，那么我们为什么还要“白”花那么多时间写测试用例呢？那是因为当随着应用规模逐渐扩大，复杂度逐渐上升的时候，完善的测试用例，给你修改代码的勇气。当单元测试显示all pass的时候，仿佛有人跟你说“干吧，哥们，没毛病。”。你不会因为怕改坏了代码而蹑手蹑脚。但这里要强调一点的是，覆盖率低的单元测试不但不能够起作用，反而会给人一种“干吧，哥们，没毛病。”的假象。这也是为什么我后面强调使单元测试覆盖率足够高的原因。我们再来看下第二个问题，这个问题涉及到一个概念叫测试驱动开发（TDD）。TDD的理念就是先写测试用例，然后写具体实现。TDD的一个重大优点就是你将具体实现放到后面，这样你就不会深陷细节的泥潭，你就拥有更清晰的视野，你会对业务或者逻辑理解更佳深刻。这就好像你看过很多高手写代码，它们会先将思路写下来，然后再写代码是一个道理。

回答了关于测试的常见问题，我们来看下单元测试究竟怎么写。其实也简单，单元测试通常来自于测试人员整理的测试用例，当然一些特殊的算法逻辑需要自己整理啦。一个原则就是能用测试同学就用测试同学，不用白不用，对吧？但是前端写单元测试的时候，总会感觉很困难，我一开始也是这么觉得的。后来我接触了函数式编程，整个人就感觉豁然开朗。传统的面向对象编程，命令式编程有一个非常大的弊端，引用Joe Armstrong（Erlang语言的创造者\)的一句话就是：

> 你想要一个香蕉，但得到的却是一个拿着香蕉的大猩猩

没错，当我回顾我很久之前的代码，的确如此。这也不能完全怪我们，我们已经习惯了各种假设，各种外部依赖。我们将这些变成理所当然，我们不断地改变状态的状态，导致状态难以追踪。因此我们写单元测试非常困难。当你开始以函数式编程理念写代码的时候，你会发现代码非常好测试。比如目前比较流行的react的展示组件，就是一个纯函数，对这样的组件进行测试就很简单。当你研究redux的代码的时候，你会发现redux的reducer设计的精妙，他将reduce在空间上的抽象变成reducer在时间上的抽象，并且reducer纯函数的理念也让代码更容易测试，具体可以看我的[这篇博文](https://my.oschina.net/wanjubang/blog/1580050)

#### 防御性编码

写代码之前要先想有没有轮子可用。如果没有在开始造轮子。我喜欢将逻辑划分为多个函数，然后在函数开头对入参进行校验，并对每一步可能出错的地方进行校验，典型的是npe（null pointer exception）。这就是典型的防御性编程。

```javascript
// String a -> Number b -> Boolean
function testA(a, b) {
   if (!a) return false;
   if (!isString(a)) return false;
   if (!b) return false;
   if(!isNumber(b)) return false
   return !!a.concat(b);
}
```

这种方法在前端尤其有效，因为js是动态语言，如果你不使用typescript等增加静态检测的功能的话，代码会变得脆弱不堪。一个简单的做法就是，假设每一行代码都会遇到异常情况，都会报错。

#### 使单元测试覆盖率足够高

前面讲了单元测试的重要性，以及单元测试的写法思路。这里强调一下单元测试的覆盖问题，低的单元测试覆盖率毫无用处，甚至会起反作用，因此保持足够高的单元测试覆盖率显得非常重要，业界普遍认可单元测试覆盖率在95%以上是比较合适的。

### 阶段三 测试完毕 - 可发布状态

这个阶段我们的代码已经经过了自测和测试人员的回归，我们认为可以发布上线了。通常我们还会让产品经理进行验收，看看是不是他们想要的效果。

#### 编译

目前写的代码是在多个文件的，我们的代码使用了很多浏览器不支持的特性，我们需要将css从js中提取出来等等。这些工作都需要通过编译来完成。

#### 包分析

我们的项目的代码是由许许多多的依赖构成的，我们会依赖一些框架如react，vue，我们会使用一些工具库如lodash，ramdajs，mostjs等。这些都构成了项目的不稳定和不确定。这也就是为什么大公司如阿里，会对外部依赖有着很强的执念。npm也察觉到了这一点，以至于现在的npm在安装过后都是锁定版本的。但这依旧无法保证依赖包的质量。 一个查看包质量的原则就是文档足够丰富，单元测试覆盖率足够高，受欢迎（start足够多），虽然上述条件不是一个库质量良好的充分条件，但却是必要条件，我们可以通过它筛选一大批不合格的库。

#### 代码检查

我们会对代码进行检查，有没有语法错误等。这一步可以通过eslint检查，也可以通过flow或者typescript这样的静态检查工具检查。总之，这一步是检查有没有错误代码或者不符合规范的代码。

#### 代码优化

代码已经通过了检查，这个时候我们需要对代码进行优化，比如压缩，合并，去空格去console等，或者提取公共依赖，再或者删除僵尸代码（tree shaking）。

#### CodeReview

我们会组织相关人员进行代码评审，确保代码质量。这部分是人工完成，是对前面工作的最后把关。

### 阶段四 准备发布 - 发布完成

#### 发布静态资源到CDN

我们资源发布到CDN等待最终的发布，保证版本发布之后，用户可以直接获得最新的CDN资源。

#### 打tag

我们将代码打tag，一个个tag就像是一个个里程碑。 当我们需要对某一个版本代码进行修复的时候，tag的作用就显示出来了。

#### 修改线上版本号

我们的功能已经达到了可以发布的状态，然后我们会对版本进行发布。修改线上的版本号，这样我们的用户就可以访问到我们最新写的代码了。

### 阶段五 发布上线 - 线上验证

我们已经将代码发布上线了，通常我们需要验证下代码是否正确发布，有没有影响线上其他功能。

上面的过程可能是大多数互联网公司的工作流程了。那么下一节我会对每一个阶段进行分析，找出可以自动化的点，并讲述自动化的技术思路是怎样的。

## 实现流程自动化的思路是什么

上面讲述了常规的需求产生到功能发布的完整过程，通过上面的分析，我们发现阶段三和阶段四是可以高度自动化的，阶段三和阶段四进行做了什么事，为了方便大家的理解，我整理了一个图：

![&#x56FE;3.1](https://github.com/azl397985856/automate-everything/master/illustrations/图3.1.png)

图中的虚线表示自动完成，无需人工。实线表示需要人工操作。图的中心点是dev，可以看出dev的操作有三个，分别是提交（commit）打tag，以及提交pr（pull request）。不同的操作会触发不同的钩子，完成不同的操作。我们一个节点一个节点进行分析，它们分别做了什么事，以及设计实现的思路。图中需要实现的系统有三个，第一个是包分析引擎（package analyser），第二个是CI中心，第三个是CD中心，我们分别来看。

### package analyser

包分析工具这里可以是前端的npm包分析，也可以是后端的比如maven包分析。这里以npm包分析为例，maven等其他包分析同理，只是具体技术实现细节不同，npm包分析和maven包分析只是具体策略不同，我们可以通过策略模式将具体的分析算法封装起来。首先看下包分析引擎实现的功能，其实包分析引擎就是分析应用依赖的包，并逐个递归分析其依赖包，找到其中有风险的依赖，并通知给使用者（项目拥有者）。具体功能包括但不限于分析有安全风险的包，提示有补丁更新，有了这些依赖数据，我们甚至可以统计公司范围内包的使用情况（包括各个版本），这些数据是很有用的。后面一节我们会具体分析包分析引擎的实现细节，使读者可以自行搭建一个npm包分析引擎。

### CI

CI（Continuous Integration）是持续将新功能集成到现有系统的一种做法，极限编程也借鉴了CI的基本思想。那么我们是怎样使用CI的呢？我刚才在图中也体现了，开发者的提交会触发CI，CI会做一些单元测试，代码检测等工作，如果不通过则反馈给相关人员，否则将通过的代码合并到库中。也就是说CI并不是一项技术，二是一种最佳实践，更多可以参考[wikipedia](https://en.wikipedia.org/wiki/Continuous_integration)。后面我会介绍实现一个CI的基本思路。

### CD

CD\(Continuous Delivery\)同样也是一种最佳实践，并不是一项技术。它的基本思想是保证代码随时可发布，它保证了代码发布的可信赖性，同时持续集成减少了开发人员的工作量。更多可以参考[wikipedia](https://en.wikipedia.org/wiki/Continuous_delivery)。后面我会介绍实现一个CD的基本思路。

当然我们的系统还比较不完善，我们还可以增加配置中心，方便对版本进行管理，还可以增加监测平台（第四章有讲），大家可以发挥自己的聪明才智。

## 我们还漏了什么

上面讲述了一个需求产生到功能上线的过程，我们也分析了如果进行自动化。我们还忽略了一个项目初期的一个过程，就是搭建脚手架的过程。如何将搭建脚手架的过程也自动化，配置化，最好还能在公司范围内保持一致性。

### 脚手架做了什么

当我们开始一个新项目的时候，要先进行技术调研和选型，当确定了技术方向的时候，我们需要一个架子，项目成员可以根据这个架子写代码。这个架子可以手工生成，很早之前我就是这么干的，但是手工生成的有很多缺点。第一就是效率低，第二就是不利于统一。为了解决这个问题，我们引入了云脚手架的概念。要明白云脚手架我们需要先知道脚手架。我们先来看下脚手架做了什么事，简单来说脚手架就是生成项目的初始代码。我们通过目前比较火的react的配套脚手架工具create-react-app\(以下简称cra\)来认识一下脚手架的工作原理。我们先来看下cra的使用方法：

```bash
npm install -g create-react-app

create-react-app my-app
cd my-app/
npm start
```

这样我们就有了下面的项目结构：

![&#x56FE;3.2](https://github.com/azl397985856/automate-everything/master/illustrations/图3.2.png)

如果要实现以上功能，一个最简单的方法就是执行create-react-app xxx 之后去远端下载文件到本地。其实cra的思路就是这样的，我用一张图来表示cra的基本过程：

![&#x56FE;3.3](https://github.com/azl397985856/automate-everything/master/illustrations/图3.3.png)

每一步的实现也比较简单，大家可以直接[查看源码](https://github.com/facebookincubator/create-react-app/master/packages/create-react-app/createReactApp.js)

### 中心化，可配置的脚手架服务

上面介绍了脚手架的作用和实现原理。但是上述的脚手架无法实现中心化，也就是说不同团队无法形成相互感知，具体来说就是不同项目的脚手架是不同的或者不能够实现高度定制化，因此我们需要构建一个中心化，可配置的脚手架服务，我称之为云脚手架。脚手架采用CS架构。客户端可以是一个CLI，服务端则是一个配置中心，模块发现中心。如下图是一个云脚手架的架构图：

![&#x56FE;3.4](https://github.com/azl397985856/automate-everything/master/illustrations/图3.4.png)

客户端通过命令告诉服务端想要初始化的模版信息，服务端会从模板库中查找对应模板，如果有则返回，没有则请求npm registry，如果有则返回并将其同步到模板库中，供下一次使用。如果没有返回失败。 这样对不同团队来说，脚手架是透明的，团队可以定制适合自己的脚手架，上传到模板库，供其他团队使用，这就形成了一个闭环。

## 如何搭建一个自动化平台

### 搭建package analyser

我将包分析引擎的工作过程分为以下三个阶段

#### 建立黑白名单

我们需要对包分析，分析的结果当然需要数据支撑。因此黑白名单是不可少的，我们可以自己补充黑白名单，我们甚至可以建立自己的黑白名单系统，当然也可以接入第三方的数据源。不管怎样，第一步我们需要有数据源，这是第一步也是最重要的一步。为了简单起见，我以JSON来描述一下我们的数据源:

```javascript
// 所有的key都是npm包的包名
{
 "whiteList": ["react", "redux", "ant-design"],
 "blackList": {
   "kid": ["insecure dependencies 'ssh-go'"]
 }
}
```

#### 递归分析包并匹配黑白名单

这一步需要递归分析包。 我们的输入只是一个配置文件，npm来说的话就是一个package.json文件。我们需要提取package.json的两个字段dependencies和devDependencies，两者就是项目的依赖包，区别在于后者是开发依赖。这个时候我们可以获得项目的一个依赖数组。形如：

```javascript
 const dependencies = [{
   name: "react",
   version: "15.4.2"
 }, {
   name: "react-redux",
   version: "5.0.3"
 }]
```

然后我们需要遍历数组，从npm registry（可以是官方的registry， 也可以是私有的镜像源）获取包的具体内容，并递归获取依赖。这个时候我们获取了项目所有的依赖的和深层依赖的包。最后我们需要根据包名去匹配黑白名单。我们还有一步需要做，就是获取包的更新日志，将有意义的日志（这里可以自己封装算法，究竟什么样的更新日志是有意义的，留给大家思考）输出给项目拥有者。

#### 结果输出

我们已经将所有的依赖包进行匹配，这个时候已经知道了系统依赖的白名单包，黑名单包和unknown包，我们有了匹配之后的数据源。

```javascript
 const result = {
    projectName: "demo"
    whiteList: ["react", "redux"],
    blackList: [{name: "kid", ["insecure dependencies 'ssh-go'"]}]，
    changeLog: [{name: ""react-redux, logs: {url: '', content: ''}}]
 }
```

我们要做到数据和显示分离。这个时候我们将数据单独存起来，然后采用友好的信息展示出来，这部分应该比较简单，不多说了。

### 搭建持续集成平台

如果想要搭建持续平台的话，最基础的三个服务是要有的，lint，test以及report。其实lint，test和report的具体实现已经超出了CI的范畴，这里就大致讲以下。对于lint来说，本质上是对js文本的检查，然后匹配一些规则，业界比较有名的是红宝书的作者nzakas的eslint，关于eslint的整体架构可以查阅[这里](https://github.com/eslint/eslint/master/docs/developer-guide/architecture.md)zakas的初衷不是重复造一个轮子，而是在实际需求得不到JSHint团队响应的情况下，自己开发并开源了eslint：一个支持可扩展、每条规则独立、不内置编码风格为理念编写一个js lint工具。对于test，其实就是运行开发人员写的测试用例，并保证运行正确且覆盖率足够高。如果上述步骤出错，则会向相关人员告警。下面是CI的架构图：

![&#x56FE;3.5](https://github.com/azl397985856/automate-everything/master/illustrations/图3.5.png)

代码会经过lint，途中会从配置中心拉取项目的presets和plugins，通过后进入下一个流水线test，test会将代码分发到浏览器云中进行单元测试和集成测试，并将结果发给相关人员，上述两个步骤如果出错也都会通过report service发送信息给相关人员。

### 搭建持续部署平台

持续部署在持续集成的基础上，将集成后的代码部署到更贴近真实运行环境的「类生产环境」（production-like environments）中。比如，我们完成单元测试后，可以把代码部署到连接数据库的 Staging 环境中更多的测试。如果代码没有问题，可以继续手动部署到生产环境中。因此持续部署最小的单元就是将代码划分为一个个可以发布的状态。下面是一个经典的企业级持续部署实现:

![&#x56FE;3.6](https://github.com/azl397985856/automate-everything/master/illustrations/图3.6.png)

[https://continuousdelivery.com/implementing/architecture/](https://continuousdelivery.com/implementing/architecture/)

可以看出持续集成，其实是将新模块融合到系统里面，形成一个可发布单元,它本身不涉及到发布的流程。真正将代码发布到线上，还是需要人工来操作。

> 可以通过配置中心实现代码发布

要想实现持续部署的架构，是需要代码和系统架构的配合，这是和前面我讲述的其他系统不一样。持续部署要求代码的耦合度要足够低，尽量少地影响其他模块。每当发布一个新功能的时候，不需要将全部代码测试回归，而是采用mock，stub等方式模拟外部依赖。目前比较流行的微服务，也是一种将代码解耦合的一种实践，它将系统划分为若干独立运行的服务，服务之间不知道彼此的存在，甚至服务之间的语言，技术架构都不相同。前面的章节讲述了组件化和模块化，在这里又可以看出组件化和模块化的重要性。

更多关于[持续部署实现](https://continuousdelivery.com/implementing/architecture/)的介绍

### 搭建通知服务等其他可接入的第三方服务

前面反复提到了反馈系统feedback。可能我们还需要其他第三方系统，比如数据可视化系统等。这里以接入通知服务为例，讲解如何接入一个通用的第三方系统。在谈通知服务具体细节之前，我们先来讲下接入一个第三方服务需要做什么。本质上接入不同的服务是服务治理的范畴，而目前服务治理当属微服务占据上风。这种通过不断接入“第三方”服务的方式使得业务和应用分离。在微服务之前，大家普遍的做法是将不同的系统做成不同的应用，然后通过某些手段进行通信。这种方式有一个显著的缺点就是应用有很多重复冗余的逻辑和代码。而微服务则不同，微服务将系统拆分成足够小的块，这样能够显著减少冗余。服务化有以下特点：

* 应用按业务拆分成服务
* 服务可独立部署
* 服务可被多个应用共享
* 服务之间可以通信

这里并不打算讨论服务治理的具体实施细节，但是需要明白的是通过这种微服务的思想。我们需要通知服务，只需要发送一个信号，告诉通知服务，通知服务返回一个信号，表示输出的结果。比如我需要接入邮件服务这个通知服务。代码大概是这样的:

```javascript
'use strict';
const nodemailer = require('nodemailer');
const promisify = require('promisify')

// Generate test SMTP service account from ethereal.email
// Only needed if you don't have a real mail account for testing
exports default mailer = async context => {

    // create reusable transporter object using the default SMTP transport
    let transporter = nodemailer.createTransport({
        host: 'smtp.ethereal.email',
        port: 587,
        secure: false, // true for 465, false for other ports
        auth: {
            user: account.user, // generated ethereal user
            pass: account.pass  // generated ethereal password
        }
    });

    // setup email data with unicode symbols
    let mailOptions = {
        from: '"Fred Foo 👻" <foo@blurdybloop.com>', // sender address
        to: 'bar@blurdybloop.com, baz@blurdybloop.com', // list of receivers
        subject: 'Hello ✔', // Subject line
        text: 'Hello world?', // plain text body
        html: '<b>Hello world?</b>' // html body
    };

    // send mail with defined transport object
    await promisify(transporter.sendMail(mailOptions, (error, info) => {
        if (error) {
            return console.log(error);
        }
        console.log('Message sent: %s', info.messageId);
        // Preview only available when sending through an Ethereal account
        console.log('Preview URL: %s', nodemailer.getTestMessageUrl(info));

        // Message sent: <b658f8ca-6296-ccf4-8306-87d57a0b4321@blurdybloop.com>
        // Preview URL: https://ethereal.email/message/WaQKMgKddxQDoou...

    }));

    return {
           status: 200,
           body: 'send sucessfully',
           headers: {
               'Foo': 'Bar'
           }
      }
};
```

如果每一个应用需要使用邮件服务，就需要写这样的一堆代码，如果公司的系统不同导致语言不同，还需要在不同语言都实现一遍，很麻烦，而如果将发送邮件抽象成通知服务的具体实现，就可以减少冗余代码，甚至java也可以调用我们上面用js写的邮件服务了。

> 小提示。当我们需要使用邮件服务的时候，最好不要在代码中直接向邮件服务发送消息，而是向通知服务这种抽象层次更高的服务发送。

有一个流行的概念是faas\(function as a service\)。它往往和无服务一起被谈起，无服务不是说没有服务器，而是将服务架构透明，对于普通开发者来说就好像没有服务器一样，这样就可以将我们从服务器环境中解放出来，专注于逻辑本身。fission\(Fast Serverless Functions for Kubernetes\)是一个基于k8s的无服务框架。通过它开发者可以只关注逻辑本身，我们可以直接将上面的mail方法作为部署单元。下面是一个例子：

```bash
  $ fission env create --name nodejs --image fission/node-env

  $ curl https://notification.severless.com/mailer.js > mailer.js

  # Upload your function code to fission
  $ fission function create --name mailer --env nodejs --code mailer.js

  # Map GET /mailer to your new function
  $ fission route create --method GET --url /mailer --function mailer

  # Run the function.  This takes about 100msec the first time.

  $ curl -H "Content-Type: application/json" -X POST -d '{"user":"user", "pass": "pass"}' http://$FISSION_ROUTER/mailer
```

这样如果有一个系统需要将mailer服务切换成sms服务就很简单了：

```javascript
  $ fission env create --name nodejs --image fission/node-env

  $ curl https://notification.severless.com/sms.js > sms.js

  # Upload your function code to fission
  $ fission function create --name sms --env nodejs --code sms.js

  # Map GET /mailer to your new function
  $ fission route create --method GET --url /sms --function sms

  # Run the function.  This takes about 100msec the first time.

  $ curl -H "Content-Type: application/json" -X POST -d '{"user":"user", "pass": "pass"}' http://$FISSION_ROUTER/sms
```

服务的实现也足够简单，只需要关心具体逻辑就OK了。

## 自动化脚本

### 哪些地方应该自动化

上面讲述了软件开发的过程，以及我们可以将哪些过程自动化。这一节，我们讲述自动化的第二部分自动化脚本。刨除软件开发本身，计算机中其实也充满了重复性工作，同样也充满了解决这些重复工作的自动化解决方案，这些解决方案可以是一个脚本，可以是一个软件或者插件等，总之它将人们从重复性的工作中解脱了出来。举个例子，我们都有过下载视频的经历，我们看上了某个网上的一个视频，我们想下载下来，但是在下载的时候，发现只有VIP可以下载。我们就去网上查找解决方案。我们按照教程历经千辛万苦终于将视频下载了下来。下次我们又要下载视频了，我们还要经历了上面的步骤（我们甚至还要再看一遍教程）。于是自动下载在线网站视频的自动化解决方法出现了，人们只需要简单的操作就可以将自己心爱的视频下载下来，多么省心！类似的还有很多，比如批量处理工具，一键重装系统工作等等，根本数不过来。

本质上，任何应该自动化的都应该被自动化。只要是重复性工作，都应该将其自动化。为什么电脑可以处理的东西，非要人工来处理呢？开发者应该将精力花费在更值得花费的地方，一些有创造性的工作上。上面介绍了普通用户的例子，现在我举一个工作中的例子。比如之前我的公司的发布，就是一个简单的流程，每次发布都要按照流程执行，这就很适合去自动化。我们当时的发布流程是这样的

```bash
git tag publish/版本号 
git push origin publish/版本号
```

然后去cdn（[https://g.alicdn.com/dingding/react-hrm-h5/version/index.js）](https://g.alicdn.com/dingding/react-hrm-h5/version/index.js）) 上查看是否发布成功。 我觉得这已经花费我一定的时间了，而且新来的同学不得不学习这种繁琐的东西。 为什么不能自动化呢？ 借用墨菲定律下， 该自动化的终将实现自动化。 自动化真的不止是减少时间，更重要的是减少出错的可能。 软件工程领域有这么一句话，减少bug有两种方式，一种是少写代码以至于没有明显bug。 二是写很多代码以至于没有明显bug。

> 少写代码，少做重复的事，这是我的信条。

如果你认真观察的话，你会发现可以自动化的东西实在是太多了。当你真正将自动化贯彻到实际编码生活中去的时候，你会发现你花费在重复工作上的时间在缩少，并且你的幸福感会提升，你会很有成就感。之前看过一个文章，文章里面说它会将任何可以自动化的东西自动化（不仅限于工作），它会编码控制如果在晚上下班的时候，自己的session还在的话（还没下班），就发自动给老婆发短信，短信内容是从预先设置好的短语中随机选取的。

> 之前我在微博上看到一个研究，它通过分析女朋友的微博，来分析女朋友的情绪。我觉得上面发送短信的时候，如果可以根据老婆的情绪对应智能回复不同内容会更棒。

### 运维

如果大家有运维经验的话，会知道运维经常需要开启，停止，重启服务。这些工作有很强的规律性，很适合做自动化。幸运的是，我们借助shell，可以与操作系统深度对话，轻松实现上面提到的运维需求。下面是一个shell脚本实现服务启动，停止和重启的例子。

```bash
#! /bin/sh
DIR= `pwd`;
NODE= `which node`

// 第一个参数是action，是start，stop和restart中的一个。
ACTION=$1

# utils
get_pid() {
   echo `ps x|grep my-server-name  |awk '{print $1}'`
}

# 启动
start() {
 pid= `get_pid`;

 if [ -z $pid ]; then
   echo 'server is already running';
 else
   $NODE $DIR/server.js 2>&1 &
   echo 'server is running now'
 fi
}
// stop和restart代码省略
case "$ACTION" in
   start)
      start
   ;;
   stop)
      stop
   ;;
 esac
```

借助shell强大的编程能力，还有将数据抽象成流，并通过组合流完成复杂任务的能力，我们可以构建非常复杂的脚本。我们甚至可以将检测三方库潜在风险信息功能做成脚本，然后集成到CI中，只有想不到，没做做不到。这里只是给大家提供思路，希望大家可以根据这个思路进行延伸，从而将一切应该被自动化的东西全部自动化。

### 开发流程自动化

上面讲述了哪些地方应该被自动化。大家看了后很可能是拿了锤子的疯子，你会发现所有的东西都是钉子，是不是任何东西都可以自动化？当然不是！ 可以自动化的应该是有着极强规律的枯燥活动，而充满创新的任务还是要让开发者自己享受。因此当你觉得某项工作枯燥无味，并且有着很强的规律和判别指标的时候就是你拿起手里锤子的时候。比如我每天都要回报我的工作，将自己的进度同步给组里其他人，虽然很枯燥乏味（希望我的领导不会看到），但是并没有规律性。因此不应该被自动化。

这里再举一个例子。我将开发过程中需要处理的事情进行了分类，我称为元脚本（meta-script），分别有如下内容：

1. concat-readme \(将项目中所有的readme组合起来，形成一个完整的readme\)
2. generate-changelog （根据commit msg 生成 changelog）
3. serve-markdown （根据markdown生成静态网站）
4. lint （代码质量检测）
5. start server （开启开发服务器）
6. stop server （停止运行开发服务器）
7. restart server （重启开发服务器）
8. start-attach （attach到浏览器，以在编辑器中进行调试）

每一个meta-script都是一个小的脚本或者一个外部库（external library），由于我使用的是npm作为包管理工具，因此我将meta-script放到了package.json文件中的script里面。这样我就可以通过运行`npm run xxx` 执行对应的脚本或者外部库了。但是别忘了，有时候我们需要做一些复杂的任务，比如我需要在浏览器中查看项目中所有的readme。那么我需要先把项目中的readme全部concat起来，然后将concat的内容作为数据源，传给serve-markdown，然后serve-markdown传给start-server。代码大概是这样的:

```bash
npm run concat-readme > npm run serve-markdown > npm run start-server --port 1089
```

我称上面的代码task，然后我们把上面的代码也放到package.json的script中，似乎这种做法很好地解决了问题。但是它有几个缺点。

* meta-script 和 task 混杂在一起，这样本身并不可怕，但是此时并不是所有的script都可以很好重用，我们的task并不是为了重用。
* package.json是作为版本控制的一部分存在，如果某个开发者希望根据自己的情况定制一个task，就不应该放到这里了。

因此我的做法是将meta-script放到版本库（这个例子我们放到了package.json中），然后将task放到编辑器中控制。我使用的编辑器是VSCODE，它有一个task manager功能，也可以下载第三方插件进行扩展。然后我们可以自己定义task，比如上面的我们可以作为个人配置保存起来，命名为start doc-site。

我们可以继续组合：

```bash
npm run changelog > npm run serve-markdown > npm run start-server --port 1089
```

我们通过meta-script又增加了一个很好用的task，我们可以命名为start changelog-site。

还有更多：

```bash
npm run stop > npm run start-attach
```

我们又实现了一个“放弃”浏览器调试，而用editor调试的task，我们称之为editor-debug。

我们可以增加更多的meta-script，我们可以根据meta-script组合更多task。然后我们只需要one-key就可以实现任意中组合的功能，是不是很棒？自己动手试试把！

### 无处不在的自动化

上面举的例子是一个典型的开发流程，那么其他日常的自动化怎么去做呢？比如我要控制电脑发送邮件，比如我要控制电脑睡眠，我要调整电脑的音量等。虽然我们也可以按照上面的思路，写一个元脚本，然后将元脚本组合。但是这里的元脚本似乎并不能通过node的cli程序或者shell脚本实现。但是这恰好是自动化必不可少的一环。因此我们面临一个GUI的自动化运行过程，将我们繁琐重复的UI工作中解脱出来。这里介绍在mac下自动化的例子。

#### JXA

说到mac中自动化，jxa是一种使用javaScript与mac中的app进行通讯的技术。通过它开发者可以通过js获取到app的实例，以及实例的属性和方法。通过jxa我们可以轻松通过JavaScript来自动化脚本完成诸如给某人发送邮件，打开特定软件，获取iTunes的播放信息等功能。下面举个发送邮件的例子：

```javascript
const Mail = Application("Mail");

const body = "body";

let message = Mail.OutgoingMessage().make();
message.visible = true;
message.content = body;
message.subject = "Hello World";
message.visible = true;

message.toRecipients.push(Mail.Recipient({address: "a@duiba.com.cn", name: "zhangsan"}));
message.toRecipients.push(Mail.Recipient({address: "b@duiba.com.cn", name: "lisi"}));

message.attachments.push(Mail.Attachment({ fileName: "/Users/lucifer/Downloads/sample.txt"}));

Mail.outgoingMessages.push(message);
Mail.activate();
```

有两种方式运行上面的例子，一种是命令行方式，另一种是直接作为脚本运行。

1. 命令行方式运行

```bash
osascript -l JavaScript -e 'Application("iTunes").isrunning()'
```

1. 作为脚本运行

一种方式是保存为文件之后运行

```bash
osascript /Users/luxiaopeng/jxa/hello.js
```

另一种是用苹果自带的脚本编辑器：

![&#x56FE;3.8](https://github.com/azl397985856/automate-everything/master/illustrations/图3.8.png)

运行之后效果是这样的：

![&#x56FE;3.7](https://github.com/azl397985856/automate-everything/master/illustrations/%20图3.7.png)

jxa提供了丰富的api供我们使用。详细可以查看脚本编辑器-文件-字典：

![&#x56FE;3.9](https://github.com/azl397985856/automate-everything/master/illustrations/图3.9.png)

比如我要查看dash的api：

![&#x56FE;3.10](https://github.com/azl397985856/automate-everything/master/illustrations/图3.10.png)

但是遗憾的是并不是所有的app都提供了很多有用的api，比如钉钉，也并不是所有的程序都有字典，比如qq，微信。好消息是mac自带的程序接口还是比较丰富的。但是我们发现尽管如此，我们想要实现某些功能，还是会比较复杂。对于不想太深入了解并且想要自动化的开发者来说一款简单的工具是有必要的，下面我介绍一款在mac下的神器。

#### Alfred

JXA的功能非常强大，但是其功能比较繁琐。如果你只是想简单地写一个自动化脚本，做一些简单的了解。介绍大家一个更加简单却不失强大的工具-alfred- workflow。 你可以自定义自己的工作流，支持GUI，shell脚本甚至前面提到的jxa写工作流。其简单易用性，以及其独特的流式处理，各种组合特性使得它功能非常强大。下面是我的alfred-workflow：

![&#x56FE;3.11](https://github.com/azl397985856/automate-everything/master/illustrations/图3.11.png)

你可以像我拆分开发流程一样将你的工作流拆解，每一部分实现自己的功能，设置语言可以不一样。比如处理用户输入用bash，然后bash将输入流重定向到perl脚本等都是可以的。 alfred-workflow可以允许你简单地添加文件操作，web操作，剪贴板等，设置不用写任何代码。

![&#x56FE;3.12](https://github.com/azl397985856/automate-everything/master/illustrations/图3.12.png)

## 总结

本章通过前端工作流程入手，讲解了前端开发中的工作，并且试图将其中可以自动化的步骤进行自动化集成。然后讲述了完善的一个自动化平台系统是怎样的，以及各个子系统实现的具体思路是怎样的，通过我的讲解，我相信大家应该已经理解了自动化的工作内容，甚至可以自己动手搭建一个简单的自动化平台了。但是程序员中的自动化远不止将实现需求的流程自动化，我们还会搞一些提高效率的小工具，本质上它们也是自动化。只不过他不属于工程化，在本书的附录部分，我也会提供一些自动化小脚本。


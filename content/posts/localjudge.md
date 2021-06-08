---
title: "Localjudge - Linux / Mac 党的 Online Judge 无 IDE 环境搭建 "
date: 2021-06-08T15:23:19+08:00
draft: false
---

最近是在研究青少年的编程教育，竟然发现这也是个庞大的圈子呢。经朋友推荐，才知道[洛谷](https://www.luogu.com.cn)这个平台，还被大家的刷题热情所感动呢。

洛谷这样的平台有很多，叫 OJ 平台，Online Judge，是在线评判的意思。简单来说，就是给你个文本框能提交代码就可以。那么本地开发怎么办呢？然后就看都各种 IDE，各种插件，何必呢？如果是 Linux / Mac 的话，必要工具都有的。于是，不妨自己开发个本地开发的工具看看。

<https://github.com/aguegu/localjudge>

代码并不多，也欢迎童鞋们参与改进。

系统要求：Linux 或 Mac

具体来说，要有 g++ 和 make

额外安装的：nodejs (>=12) 和 entr (可通过包管理工具安装)

原理很简单，nodejs脚本(src/index.mjs)，其实是个子线程（就是调用测试代码）的启动器。它从项目文件夹的 `readme.yaml` 文件中读取测试数据（可多组）。然后把成败的结果都输出，并且还有个简单的超时判断（3秒）。

在 `Makefile` 中，主要是对项目文件夹中 main.cpp 的实时编译和运行。集大成者是 `make watch` 命令，通过 `entr` 监听项目文件夹中 `*.cpp` 文件的变化（保存动作），发现变化，就编译，编译成功的话，就继续拿刚才的测试数据跑跑看。

我只实验了2道入门最简单的题目，提交到洛谷平台后，需要将语言选项设为 `C++11`，否则编译会有问题。

详细内容欢迎参考项目 [readme](https://github.com/aguegu/localjudge#readme) 文件。也欢迎留言讨论。

这么一通操作下来，其实体验，应该是接近 nodejs 开发后端的 [nodemon](<https://www.npmjs.com/package/nodemon>) + [mocha](https://mochajs.org/) 的体验。只不过 `nodemon` 被 `entr` 取代，`mocha` 被 `localjudge` 取代，`npm scripts` 被 `Makefile` 取代。

项目虽小，其实要和玩算法的童鞋介绍起来，也要费一点劲儿。或者说可以通过这个小项目看看工作中的应用开发和他们玩的算法有什么不一样。

1. 关注安装环境，这里是 linux，有终端的地方，项目发布出来别人要能用得起来。
2. 尽量多用现成的工具，取自开源，回馈开源，自己不用造那么多轮子，如果说我用一个 nodejs 程序，实现全部功能可以吗？当然可以，但是 Make，entr，YAML 都是常用的，而且经过长时间历史验证的程序，为什么我要自作聪明去开发呢。正如杨绛所说“你的问题主要在于读书不多而想得太多”。作为工程师的话，如果找轮子，应用现有轮子的能力，其实比写轮子更重要，而且这些轮子是可以一直帮助你的，localjudge 的逻辑很简单，我相信任何一个能调用子进程的编程语言都能实现。只是我比较偏爱 js 一些。未来如果换其它语言，Make，entr，YAML 这些依然可以帮到你，这些轮子可以接着用，而我们开发的应用（localjudge）本身，只要关注把自己改作的事情做好。
3. 自己应用的开发也是，nodejs 生态（npm）里面的包就很多能用，像 [chalk](https://www.npmjs.com/package/chalk), [yaml](https://www.npmjs.com/package/yaml) 这些以前就积累了，[pidusage](https://www.npmjs.com/package/pidusage) 就是这回就是现学现卖的。项目不怕小，也配备一个基本的文档，才算相对完整一些。
4. 写程序不是画界面，发现太多的编程入门课程都在学画UI，从我二十多年前的VB开始，到现在少儿学 python，至于 scratch 就更不用说了。问问自己核心的代码到底有多少？画界面也好，然后你就可以感觉画界面，维护界面是个多么费劲的事情。然后你就知道为什么命令行的功能远比GUI强大得多。所以，开发一个把自己的事情做好的小命令行工具（就像那么多 shell 命令）一样，比一个高大全的GUI应用具有更长的生命力。
5. 作为前浪，希望后浪们多用 Linux，区分清楚编辑器（editor）和编译器（compiler），并且多和终端（shell）打交道

欢迎大家试试看 localjudge，也欢迎移步到 [洛谷讨论区](https://www.luogu.com.cn/discuss/show/321053)继续讨论。

# 《About RESTful》

原文地址：[https://github.com/howiehu/about-restful](https://github.com/howiehu/about-restful "原文地址")

## 目录
[《About RESTful》](#about-restful)

* [目录](#目录)
* [作者](#作者)
* [前言](#前言)
* [容易搞错的概念](#容易搞错的概念)
* [什么是 REST](#什么是-rest)
  * [名词解释](#名词解释)
  * [简介](#简介)
  * [关键目标](#关键目标)
  * [判断是否是 RESTful 的约束条件](#判断是否是-restful-的约束条件)
  * [REST 基本概念](#rest-基本概念)
     * [词汇重用 vs 随意扩展：HTTP 与 SOAP](#词汇重用-vs-随意扩展http-与-soap)
  * [REST 接口设计指导原则](#rest-接口设计指导原则)
  * [REST 核心原则](#rest-核心原则)
* [什么是 RESTful Web API](#什么是-restful-web-api)
  * [与本文相关的 HTTP 知识](#与本文相关的-http-知识)

## 作者

作者：[胡皓](http://blog.huhao.name "胡皓") (Howie Hu)

Twitter：howiehu

新浪微博：[长安胡小闹](http://weibo.com/howiehu "长安胡小闹")

GitHub：[howiehu](http://github.com/howiehu "howiehu")

## 前言

写这篇文章的起因，是因为我需要在2013年9月21日的 **第四次西安 Rubyist 线下交流活动** 中做一次有关于 RESTful 的技术分享。但是在我准备技术分享材料的过程中发现：虽然平时看起来 RESTful 这个东西还算比较好理解，但是由于 RESTful 并没有一个统一的标准或者规范，再加上相关资料非常多，这些资料没有统一的观点，甚至没有统一的中文命名方式，每个人的看法也都有所差异。所以，当真正需要做技术分享的时候，才发现没有那么容易。

正因为以上原因，在这里，我将把我所看过的资料，按照我个人的理解方式进行汇总、整理和精炼，力求写出一篇能够比较全面且容易理解的介绍 RESTful 的文章。

由于获取资料的途径、个人能力和理解方式等客观因素，本文可能存在许多不足之处，望大家能够谅解，同时也 **非常欢迎大家在 [GitHub](https://github.com/howiehu/about-restful "原文地址") 上提出修改意见** ，十分感谢！

## 容易搞错的概念

RESTful 这个词其实很容易误解和误用，我们经常能够听到大家去频繁的使用这个词。但是我们必须要注意的是， **RESTful 这个词只是描述某一种架构是否符合 REST 架构风格，而并没有确定其具体的设计细节** 。所以，我们在说出 RESTful 这个词的时候， **必须搞清楚你所描述的对象：到底是符合 REST 架构风格，还是仅仅采用了 RESTful Web API 这种具体的架构** 。

## 什么是 REST

### 名词解释

REST (Representational State Transfer) 在维基百科上被译为“表征状态转移”，在百度百科上被译为“表述性状态转移”，在其他地方还被译为“表现层状态转化”等等。

如此繁多的中文翻译实在是让人难以理解，但是，在不断了解各种解释之后，我认为对于 REST 来说，一个比较好的解释是： **资源的-某种表现形式的-状态转化** 。

可能您看到这个描述依然还是不大明白其中的意思，没关系，接下来我会进一步说明。

### 简介

REST 是一种用于提取和抽象分布式超媒体（超链接+多媒体）系统内部结构元素的架构风格。REST 专注于组件所扮演的角色、组件间交互的约束条件以及对于重要数据元素的解释方式, 忽略组件的实现细节以及协议语法。目前，REST 已经成为了一种首选的 Web API 设计模型。

REST 由 Roy Thomas Fielding 博士于2000年的一篇博士论文中提出（他也是 HTTP 1.0 和 1.1 协议的主要编写者之一）。

REST 架构风格基于 HTTP/1.0 的已有设计，由 W3C 技术架构小组（TAG）在开发 HTTP/1.1 的同时进行并行开发。如今的万维网便是符合 REST 架构风格的最大系统实现。

REST 风格的架构通常由客户端和服务器组成。客户端向服务器发起请求，服务器处理请求并返回恰当的响应。请求和响应建立在资源的表现形式的转化之上。资源可以是一切能够被定址的清晰且有意义的概念。资源的表现形式通常是一种能够捕获该资源当前以及预期状态的文档。

当准备转化到一个新的状态时，客户端开始发出请求。当一个或多个请求被发出后，客户端处于过渡状态。每一个应用程序状态的表现形式均包含用于客户端选择并发起下一个新状态转化请求的链接。

（以上内容是照搬维基百科上的，比较学术化，看不懂没关系，不必深究。）

### 关键目标

REST 的关键目标包括：

* 组件间交互的可伸缩性
* 接口的通用性
* 组件的独立部署
* 通过中间组件来减少延迟、实施安全策略和封装遗留系统

### 判断是否是 RESTful 的约束条件

**客户端-服务器分离**

客户端通过统一的接口与服务器相分离。举例来说，这种关注点分离意味着：客户端不关心数据的存储，它们都应当在每个服务器内部来实现，这样就改进了客户端代码的可移植性。服务器不关心用户界面或者用户状态，所以服务器可以更加简单并且具有可伸缩性。只要它们之间的接口不发生改变，那么服务器和客户端均能够被替换和独立开发。

**无状态**

在客户端与服务器通信时，任何客户端的上下文都不应当被服务器所存储。客户端的每一次请求均包含有足够的必要信息，会话状态应当保存在客户端。需要注意的是，会话状态应当能够在类似数据库维护的时候从一台服务器迁移到另一台服务器，并且能够保持验证状态。

**可缓存**

就像在互联网上一样，客户端能够缓存服务器的响应。服务器响应必须能够显式的或者隐式的将自己定义为可缓存的或者不可缓存的，从而避免客户端在接下来的请求中重用不一致的或过期的数据。管理良好的缓存机制能够部分的或全部的抵消掉一些客户端与服务器之间的交互，从而改进了可伸缩性和性能。

**多层系统**

客户端通常无法知道自己连接的是最终服务器还是中间服务器。中间服务器能够通过开启负载平衡和共享缓存来改进系统的可伸缩性，它们同样还能够实施安全策略。

**统一接口**

客户端与服务器之间的统一接口能够简化和解耦系统架构，并且能够使各个部件独立进化，这些内容将在下面的接口设计指导原则中进行详细介绍。

**随需代码（可选）**

服务器功能能够通过客户端可执行代码的转化来临时进行扩展和自定义，比如可以包含 Java applets 这样的已编译组件或者 JavaScript 这样的客户端脚本。随需代码是 REST 架构风格中唯一一个可选的约束条件。

一个能够符合上述所有约束的应用程序或者服务就可以被称作是 **“RESTful”** 的，反之，如果违反了上述任何一条约束的话，就不能被称作 RESTful。

### REST 基本概念

REST 是通过观察当前 Web 应用的运作方式而抽象出来的。Roy Fielding 认为：

**设计良好的 Web 应用表现为一系列的网页，这些网页可以看作的虚拟的状态机，用户选择这些链接导致下一网页传输到用户端展现给使用的人，而这正代表了状态的转变。**

REST 最初是通过 HTTP 的上下文来描述的，但是它并不局限于这种协议。如果能够基于有意义的表现形式的状态转化，并为应用程序提供丰富且一致的词汇表，那么RESTful 架构能够建立在任何的应用程序层协议之上。

RESTful 应用程序能够最大程度的利用已经存在的，拥有良好接口定义的，提供内置功能的各种网络协议，并最小程度的在其之上增加新的特定应用程序特性。

#### 词汇重用 vs 随意扩展：HTTP 与 SOAP

除了 URI（统一资源标识符）、互联网媒体类型、HTTP 请求和 HTTP 状态码等元素以外，HTTP 还拥有一种操作词汇叫做 **请求方法（Request methods）** ，其中用的比较多的有：

    GET
    POST
    PUT
    PATCH
    DELETE

（未完待续）

### REST 接口设计指导原则

提供统一的接口是任何 REST 服务必须考虑的基本原则。

**标识资源**

每一个独立的资源在请求中都拥有唯一的标识，比如在基于 Web 的 REST 系统中使用 URI 来标识资源。资源本身从概念上与返回给客户端的表现形式相分离，举例来说，服务器并不会向客户端发送它的数据库，取而代之的是使用 HTML、XML 或者 JSON 来表现数据库中的记录，再比如使用中文并用 UTF-8 进行编码，这些都取决于客户端请求和服务器的实现细节。

**通过表现形式操作资源**

当客户端获取到了资源的某种表现形式，包括任何附加的元数据，它都有足够的信息来修改或删除服务器上的相应资源，只要它拥有那样做的权限。

**自描述消息**

每一条消息都含有足够的信息来描述如何处理这条消息，比如，需要使用何种方式来处理互联网媒体类型（以前叫做 MIME 类型）。服务器响应也会明确标明它是否可被缓存。

**超媒体即应用状态引擎（HATEOAS）**

客户端只能在服务器动态标识的超媒体内部通过动作进行状态转化（比如超文本中的超链接）。除了访问应用程序的简单的固定入口点之外，客户端不能获得到超出先前从服务器获取到的表现形式中的描述以外的任何特定资源的特定可用动作。

### REST 核心原则

REST 的一个重要概念是资源的存在（特定信息的来源），每一个资源都应当通过全局标识符来引用（比如 HTTP 中的 URI）。为了操作这些资源，网络组件（用户代理和源服务器）通过标准化接口（比如 HTTP）与资源进行通信并交换资源的表现形式（传递信息的实际文档）。比如，一个资源需要表现一个圆形（作为一个逻辑对象），它会接受并返回一个包含了圆心和半径的 SVG 格式文件，也可以接受并返回一个包含有圆上三个点的坐标并用逗号分隔的列表（三点可以确定一个圆）。

任何数量的连接器（比如客户端、服务器、缓存、隧道，等等）都可以调用请求，但是不会看到自己“过去的请求”（被称之为“多层”，REST的另一个重要约束条件，同时也是许多其他信息和网络架构的共有原则）。因此，一个应用程序只需要通过两件东西与资源进行交互：资源的标识和所需要的动作——它不需要知道缓存、代理、网关、防火墙、隧道或者任何其他的东西，服务器将会保存这些信息。应用程序需要了解返回信息（表现形式）的格式，通常会像是 HTML、XML 或者 JSON 这样的文档，也可能是图像、纯文本或者其他的内容。

## 什么是 RESTful Web API

RESTful Web API （也可以叫做 RESTful Web Service）是一种基于 HTTP 和 REST 架构风格的 Web API 实现。它是一个资源的集合，有四个方面的定义：

* 使用基本的 URI，比如 ```http://example.com/resources/``` 。
* 使用互联网媒体类型表现数据，通常使用的是 JSON，但是也可以使用其他有效的互联网媒体类型。
* 使用 HTTP 请求方法来支持操作（比如 GET、PUT、POST 或者 DELETE 等 HTTP 动词，Ruby on Rails 4.0 以后的版本中，用 PATCH 代替了 PUT 以便减少数据传输）。
* API 必须是超文本驱动的。

下面的表格显示了 HTTP 请求方法如何用来实现 Web API：

资源 | GET | PUT | POST | DELETE
---- | --- | --- | ---- | ------
http://example.com/resources | 用于列出集合成员中的所有 URI 或者其他细节信息 | 用其他的集合替换整个当前集合 | 在当前集合中创建一个新的条目，新条目的 URI 通常会自动生成并返回 | 删除整个集合

### 与本文相关的 HTTP 知识

### 一些成熟的 RESTful Web API 框架

* Sinatra
* ASP.NET Web API

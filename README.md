![](https://i.postimg.cc/4xhqKLQj/image.png)

# 服务端开发实践与工程架构

在[某熊的技术之路指北 ☯](https://github.com/wx-chevalier/Developer-Zero-To-Mastery)中我们介绍过，本系列[服务端架构与实践](https://github.com/wx-chevalier/Backend-Series)与[深入浅出分布式基础架构](https://github.com/wx-chevalier/Distributed-Infrastructure-Series)这两个系列承载了笔者在泛服务端开发、运维相关工作中的知识沉淀。

# Nav | 导航

您可以通过以下任一方式阅读笔者的系列文章，涵盖了技术资料归纳、编程语言与理论、Web 与大前端、服务端开发与基础架构、云计算与大数据、数据科学与人工智能、产品设计等多个领域：

- 在 Gitbook 中在线浏览，每个系列对应各自的 Gitbook 仓库。

| [Awesome Lists](https://ngte-al.gitbook.io/i/) | [Awesome CheatSheets](https://ngte-ac.gitbook.io/i/) | [Awesome Interviews](https://github.com/wx-chevalier/Developer-Zero-To-Mastery/tree/master/Interview) | [Awesome RoadMaps](https://github.com/wx-chevalier/Developer-Zero-To-Mastery/tree/master/RoadMap) | [Awesome-CS-Books-Warehouse](https://github.com/wx-chevalier/Awesome-CS-Books-Warehouse) |
| ---------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |


| [编程语言理论与实践](https://ngte-pl.gitbook.io/i/) | [软件工程、数据结构与算法、设计模式、软件架构](https://ngte-se.gitbook.io/i/) | [现代 Web 开发基础与工程实践](https://ngte-web.gitbook.io/i/) | [大前端混合开发与数据可视化](https://ngte-fe.gitbook.io/i/) | [服务端开发实践与工程架构](https://ngte-be.gitbook.io/i/) | [分布式基础架构](https://ngte-infras.gitbook.io/i/) | [数据科学，人工智能与深度学习](https://ngte-aidl.gitbook.io/i/) | [产品设计与用户体验](https://ngte-pd.gitbook.io/i/) |
| --------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------- |


- 前往 [xCompass https://wx-chevalier.github.io](https://wx-chevalier.github.io/home/#/search) 交互式地检索、查找需要的文章/链接/书籍/课程，或者关注微信公众号：某熊的技术之路。

![](https://i.postimg.cc/3RVYtbsv/image.png)

- 在下文的 [MATRIX 文章与代码矩阵 https://github.com/wx-chevalier/Developer-Zero-To-Mastery](https://github.com/wx-chevalier/Developer-Zero-To-Mastery) 中查看文章与项目的源代码。

| [服务端应用程序开发基础](./服务端基础) | [微服务与云原生](./微服务与云原生) | [深入浅出 Node.js 全栈架构](./Node) | [Spring Boot 5 与 Spring Cloud 微服务实践](./Spring) | [DevOps 与 SRE 实战](./DevOps) | [信息安全与渗透测试必知必会](./信息安全与渗透测试) | [测试与高可用保障](./测试与高可用保障) |
| -------------------------------------- | ---------------------------------- | ----------------------------------- | ---------------------------------------------------- | ------------------------------ | -------------------------------------------------- | -------------------------------------- |


# Preface | 前言

![](https://tva1.sinaimg.cn/large/007DFXDhgy1g4jlvqqryvj30u014o4qp.jpg)

基于 HTTP 协议的 Web API 是时下最为流行的一种分布式服务提供方式。无论是在大型互联网应用还是企业级架构中，我们都见到了越来越多的 SOA 或 RESTful 的 Web API。为什么 Web API 如此流行呢？我认为很大程度上应归功于简单有效的 HTTP 协议。HTTP 协议是一种分布式的面向资源的网络应用层协议，无论是服务器端提供 Web 服务，还是客户端消费 Web 服务都非常简单。再加上浏览器、Javascript、AJAX、JSON 以及 HTML5 等技术和工具的发展，互联网应用架构设计表现出了从传统的 PHP、JSP、ASP.NET 等服务器端动态网页向 Web API + RIA(富互联网应用)过渡的趋势。Web API 专注于提供业务服务，RIA 专注于用户界面和交互设计，从此两个领域的分工更加明晰。在这种趋势下，Web API 设计将成为服务器端程序员的必修课。然而，正如简单的 Java 语言并不意味着高质量的 Java 程序，简单的 HTTP 协议也不意味着高质量的 Web API。要想设计出高质量的 Web API，还需要深入理解分布式系统及 HTTP 协议的特性。

[![image.png](https://i.postimg.cc/Y2vPQ05k/image.png)](https://postimg.cc/G91zCL3S)

- UI 交互层：Windows UI、PC Web UI、移动 App UI、微信小程序 UI、摄像头视觉识别人机界面、语音交互人机界面

- 应用层：无数碎片化的、场景化的前台业务应用，如零售、采购、招聘、报销...

- 业务中台：协同平台（工作流引擎/消息引擎等）、应用组件（聚合支付/电子发票/电子合同/自动报税等等）、集成开发平台/定制开发平台/实施平台/运维平台，如零售中台、人力中台、财务中台....

- 数据与算法中台：数据库/NOSQL 数据库、大数据计算平台/数据仓库数据湖/可视化、人工智能 NLP/机器学习、大数据技术平台（如 Hadoop 和 Spark）、AI 技术引擎

- 中间件：微服务中间件、面向对象技术/组件技术/SOA、服务中间件/微服务中间件技术

- 基础设施层：云计算 IaaS（服务器、存储、网络、文件系统）、云 IaaS 平台

# 版权

<img src="https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg"/><img src="https://parg.co/bDm" />

笔者所有文章遵循 [知识共享 署名-非商业性使用-禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)，欢迎转载，尊重版权。如果觉得本系列对你有所帮助，欢迎给我家布丁买点狗粮(支付宝扫码)~

![default](https://i.postimg.cc/y1QXgJ6f/image.png)

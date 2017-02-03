# Microsoft REST API 指南
## Microsoft REST API 指南工作组

 | | |
---------------------------- | -------------------------------------- | ----------------------------------------
Dave Campbell (CTO C+E)      | Rick Rashid (CTO ASG)                  | John Shewchuk (Technical Fellow, TED HQ)
Mark Russinovich (CTO Azure) | Steve Lucco (Technical Fellow, DevDiv) | Murali Krishnaprasad (Azure App Plat)
Rob Howard (ASG)             | Peter Torr  (OSG)                      | Chris Mullins (ASG)

<div style="font-size:150%">
文档编辑者: John Gossman (C+E), Chris Mullins (ASG), Gareth Jones (ASG), Rob Dolin (C+E), Mark Stafford (C+E)<br/>
</div>



# Microsoft REST API 指南
## 1 摘要
作为一个设计的原则，鼓励应用程序开发者通过 RESTful 的 HTTP 接口去获取资源。为了让追随 Microsoft REST API 指南的开发者有尽可能优雅的使用体验，
REST APIs 应该保持一贯的设计原则，来提高易用性和更具有倡导性。

此文档建立了 Microsoft REST APIs 应该遵循的指南，使得 RESTFUL 的接口可以被一致的开发。


## 2 目录
<!-- TOC depthFrom:1 depthTo:3 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Microsoft REST API 指南 2.3](#microsoft-rest-api-guidelines-23)
  - [Microsoft REST API 指南工作组](#microsoft-rest-api-guidelines-working-group)
- [Microsoft REST API Guidelines](#microsoft-rest-api-guidelines)
  - [1 摘要](#1-摘要)
  - [2 目录](#2-table-of-contents)
  - [3 介绍](#3-introduction)
    - [3.1 推荐读物](#31-recommended-reading)
  - [4    指南解读](#4-interpreting-the-guidelines)
    - [4.1    指南的应用](#41-application-of-the-guidelines)
    - [4.2    已有服务指南和服务版本](#42-guidelines-for-existing-services-and-versioning-of-services)
    - [4.3    语气用词](#43-requirements-language)
    - [4.4    许可](#44-许可)
  - [5    分类](#5-taxonomy)
    - [5.1    错误](#51-errors)
    - [5.2    故障](#52-faults)
    - [5.3    耗时](#53-latency)
    - [5.4    完成时间](#54-time-to-complete)
    - [5.5    长久运行的API 故障](#55-long-running-api-faults)
  - [6    客户端指南](#6-client-guidance)
    - [6.1    忽略的规则](#61-ignore-rule)
    - [6.2    变量排序规则](#62-variable-order-rule)
    - [6.3    静默处理失败规则](#63-silent-fail-rule)
  - [7    一致性的基础](#7-consistency-fundamentals)
    - [7.1    URL 结构](#71-url-structure)
    - [7.2    URL 长度](#72-url-length)
    - [7.3    规范标示](#73-canonical-identifier)
    - [7.4    支持的方法](#74-supported-methods)
    - [7.5    标准的请求头部](#75-standard-request-headers)
    - [7.6    标准的返回头部](#76-standard-response-headers)
    - [7.7    自定义头部](#77-custom-headers)
    - [7.8    指定头部作为请求参数](#78-specifying-headers-as-query-parameters)
    - [7.9    PII 参数](#79-pii-parameters)
    - [7.10   返回格式](#710-response-formats)
    - [7.11   HTTP 状态码](#711-http-status-codes)
    - [7.12   可选的客户端库](#712-client-library-optional)
  - [8    跨域资源共享](#8-cors)
    - [8.1    客户端指南](#81-client-guidance)
    - [8.2    服务端指南](#82-service-guidance)
  - [9    集合](#9-collections)
    - [9.1    元素的键](#91-item-keys)
    - [9.2    序列化](#92-serialization)
    - [9.3    集合URL的模式](#93-collection-url-patterns)
    - [9.4    大集合](#94-big-collections)
    - [9.5    改变集合](#95-changing-collections)
    - [9.6    排列集合](#96-sorting-collections)
    - [9.7    过滤](#97-filtering)
    - [9.8    分页](#98-pagination)
    - [9.9    集合的混合操作](#99-compound-collection-operations)
  - [10    Delta查询](#10-delta-queries)
    - [10.1    Delta链接](#101-delta-links)
    - [10.2    实体表示](#102-entity-representation)
    - [10.3    获取delta链接](#103-obtaining-a-delta-link)
    - [10.4    delta链接返回的内容](#104-contents-of-a-delta-link-response)
    - [10.5    使用delta链接](#105-using-a-delta-link)
  - [11    JSON 标准化](#11-json-standardizations)
    - [11.1    原始类型的JSON格式化标准](#111-json-formatting-standardization-for-primitive-types)
    - [11.2    日期和时间](#112-guidelines-for-dates-and-times)
    - [11.3    日期和时间的JSON序列化](#113-json-serialization-of-dates-and-times)
    - [11.4    持续](#114-durations)
    - [11.5    间隔](#115-intervals)
    - [11.6    重复的间隔](#116-repeating-intervals)
  - [12    版本](#12-versioning)
    - [12.1   版本格式](#121-versioning-formats)
    - [12.2    版本定义时机](#122-when-to-version)
    - [12.3    重大改变的定义](#123-definition-of-a-breaking-change)
  - [13    长时间运行的操作](#13-long-running-operations)
    - [13.1    资源为基础的长时间运行操作 (RELO)](#131-resource-based-long-running-operations-relo)
    - [13.2    增量式的长时间运行的操作](#132-stepwise-long-running-operations)
    - [13.3    操作结果的保留政策](#133-retention-policy-for-operation-results)
  - [14    通过webhook推送通知](#14-push-notifications-via-webhooks)
    - [14.1    作用域](#141-scope)
    - [14.2    原则](#142-principles)
    - [14.3    订阅类型](#143-types-of-subscriptions)
    - [14.4    调用顺序](#144-call-sequences)
    - [14.5    订阅验证](#145-verifying-subscriptions)
    - [14.6    接收通知](#146-receiving-notifications)
    - [14.7    以编程方式管理订阅](#147-managing-subscriptions-programmatically)
    - [14.8    安全](#148-security)
  - [15    未支持的请求](#15-unsupported-requests)
    - [15.1    基本指南](#151-essential-guidance)
    - [15.2    允许的特性列表](#152-feature-allow-list)
  - [16     附录](#16-附录)
    - [16.1    序列图](#161-sequence-diagram-notes)
  

<!-- /TOC -->

## 3 介绍
开发者多数都是通过HTTP 接口去访问Microsoft 云平台的资源。
尽管每个服务通常都会提供语言相关的框架来包装自己的 API 接口，但这些操作最终都可以归结为 HTTP 请求。
Microsoft 必须提供对各种客户端和服务的支持，而不能依赖于每个开发环境上的各种丰富的框架。
因此，这个指南的一个目标就是保证Microsoft 的REST APIs可以被任何支持基础HTTP的客户端方便并且容易的被调用。

为了给开发者如丝般顺滑的使用体验，让 API 更易被使用，保证 API 遵守一致的设计指南非常重要。
本文档的目的就是建立一个指南，让 Microsoft REST API 的开发者们去遵守，以便保证 API 的一致性。

保持一致性的好处会慢慢累积；一致性可以让团队充分利用通用的代码，模式，文档以及设计决策。

本指南有如下目标：

- 贯穿Microsoft，定义了一致的API设计和实践指南。
- 积极地采纳业界REST/HTTP 的最佳实践。
- 让应用程序开发者更加容易的使用REST 接口访问Microsoft 的服务。
- 让服务开发者可以在服务实现之前，提前测试，编写REST 接口文档。
- 让非Microsoft人员使用这些指南来设计他们的REST 接口。

*注: 此指南被设计为与 REST 架构的服务相配合使用，尽管服务并不一定遵守 REST 规定。此文档里的 “REST” 表示服务受到 REST 的启发，并不是照搬教科书*

### 3.1 推荐读物

理解 REST 架构风格背后的哲学是开发优质 HTTP 服务的基础。
如果你还是对 RESTful 设计一脸懵逼，下面有一些资源可供参考：

[REST 维基百科][rest-on-wikipedia] -- REST 的通用的定义和核心思想。

[REST 论文][fielding] -- REST 这一章是关于 Roy Fieling 的关于 网络架构的论文，“架构风格和网络应用架构设计”。


[RFC 7231][rfc-7231] -- 定义了 HTTP/1.1 语义规范，被认为是最权威的资料。

[REST 实践][rest-in-practice] -- 一本关于 REST 基础的书籍。

## 4 指南解读
### 4.1 指南的应用
本指南对 Microsoft 或者合作伙伴的服务的任何开放的 REST API 都适用。

私有的或者内部的 API 也应该尝试去遵守本指南，因为内部服务最终都有可能开放出来。

API 的一致性不仅对外部消费者非常重要，对于内部服务的消费者也同样重要。本指南提供了对于任何服务都适用的最佳实践。

当然也有一些合理的理由不遵守本指南。显然，一些提供 REST 接口的服务实现了外部的 REST API 标准或者需要与它保持兼容，而不是本指南。还有一些服务对于性能有特殊要求，接口需要适用其他合适传输数据，例如二进制协议。

### 4.2 先有服务和服务演进指南
我们不建议为了符合指南规范对现在已有的服务做出重大的更改。

当决定服务不在兼容老版本的时候，那下一版本发布的服务应该遵守指南的约定。

当新添加一个 API 的时候，那么这个 API 应该与其他相同版本的 API 保持一致的风格。

如果当前的服务遵守的是 1.0 版本的指南，当添加新的 API 的时候，也同样遵守 1.0 版本的指南。当服务的下一个大版本发布的时候，统一修改 API 使其遵循指南的最新版本。

*译者注*：最后一句的意思是，当本指南版本有更新的版本时候才会考虑。

### 4.3 祈使语气

本文档里出现的关键词 "MUST," "MUST NOT," "REQUIRED," "SHALL," "SHALL NOT," "SHOULD," "SHOULD NOT," "RECOMMENDED," "MAY," 和 "OPTIONAL" 的具体含义可以参考 [RFC 2119文档](https://www.ietf.org/rfc/rfc2119.txt);

### 4.4 许可

本作品根据知识共享署名4.0国际许可协议进行许可。
要查看此许可证的副本，请访问http://creativecommons.org/licenses/by/4.0/或发送信件到Creative Commons，PO Box 1866，Mountain View，CA 94042，USA。

## 5 分类
作为符合 Microsoft REST API 准则的一份子，服务必须（MUST）符合以下定义的分类。

### 5.1 错误
错误（或更具体地说服务错误）被定义为客户端向服务传递无效数据，服务 _正确_ 拒绝该数据。

示例包括无效凭据，不正确的参数，未知的版本ID或类似信息。

这些通常是“4xx”HTTP错误代码，并且是客户端传递不正确或无效数据的结果。

错误 _不会_ 影响总体API可用性。

### 5.2 故障
故障，或更具体地，服务故障，被定义为服务无法正确返回以响应有效的客户端请求。这些通常是“5xx”HTTP错误代码。

故障确实 _有助于_ 总体API可用性。

由于速率限制或配额故障而失败的呼叫不得(MUST NOT)计为故障。

由于服务快速失败请求（通常为了自身的保护）而失败的调用会计为故障。

### 5.3 耗时
耗时定义为特定API调用完成的时间，尽可能接近客户端的计算结果。

此指标同样适用于同步和异步API。

对于长时间的调用，耗时是根据初始请求测量的，并测量调用（而不是整个操作）完成所需的时间。

### 5.4 完成时间
具体长操作（更具体的讲是复杂操作）的服务必须（MUST）跟踪这些操作的“完成时间”指标。

### 5.5 长时间运行的 API 故障
对于需要长时间运行的API，在技术上，可能初始请求开始操作和获取结果的请求正常工作（每个返回200），但是底层操作失败。

长期运行故障必须（MUST）作为故障汇总到总体可用性指标中。

## 6 客户端指南
为了确保客户与 RESTFUL 服务交互获得最佳体验，客户应（SHOULD）遵守以下最佳实践：

### 6.1 忽略规则
对于松散耦合的客户端，其中在调用之前数据的确切形状是未知的，如果服务器返回客户端不期望的东西，则客户端必须安全地忽略它。

某些服务可以（MAY）在不更改版本号的情况下向响应添加字段。 这样做的服务必须（MUST）在其文档中明确说明，客户端必须（MUST）忽略未知字段。

### 6.2 变量排序规则
客户不能（MUST NOT）依赖服务响应的 JOSN 数据的显示的顺序。例如，客户端应该（SHOULD）对JSON对象中的字段重新排序具有容错性。

当服务支持时，客户端可以（MAY）请求以特定顺序返回数据。例如，服务可以支持使用 orderBy 的请求参数来指定JSON数据中元素的顺序。

服务还可以（MAY）明确地指定元素的顺序。例如，服务可以（MAY）始终返回JSON对象的“类型”信息作为对象中的第一个字段，以简化客户端上的响应解析。客户端可以（MAY）依赖于由服务显式标识的排序行为。

### 6.3 静默处理失败规则
请求可选服务器功能（如 Header 里的 OPTIONS 字段）的客户端必须（MUST）对服务器具有容错性，如果服务端不支持，那么应该可以安全的忽略该特定功能。

## 7 一致性的基础
### 7.1 URL 结构
人类应该（SHOULD）能够轻松地阅读和构造 URL 。

这有助于在没有得到良好支持的客户端库的平台上发现和轻松采用。

一个结构良好的 URL 示例：

```
https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox
```

一个不好的 URL 示例如下：

```
https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')
```
一种常见的模式是使用 URL 作为值。服务可以（MAY）使用 URL 作为值。
例如，如下的 URL 是可以被接受的：

```
https://api.contoso.com/v1.0/items?url=https://resources.contoso.com/shoes/fancy
```

### 7.2 URL 长度
在RFC 7230的[3.1.1] [rfc-7230-3-1-1]部分的HTTP 1.1消息格式定义了请求行的长度没有限制，包括目标URL。

RFC：

> HTTP不会对请求行的长度设置预定义的限制。 [...] 如果服务端接收到超过自身处理处理的长 URI，它必须（MUST）用414（URI Too Long）状态码响应。

可生成长度超过2,083个字符的 URLs 的服务必须(MUST)为他们希望支持的客户提供方便。以下是确定目标客户端是否支持的一些资料：

 * [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)
 * [https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/)
 
还要注意，一些技术栈具有强制或者可调节的 url 限制，因此在设计您的服务时记住这一点。

### 7.3 规范标示符
除了友好的 URL, 可以移动或重命名的资源应该(应该)暴露一个包含唯一稳定标识符的URL。

可能（MAY）需要与服务交互，并通过资源的可读性较好的名字来获取一个稳定的 URL，就像一些服务里 "/my" 的用法一样。

这个文档的标识符不一定是一个 GUID（全局唯一标识符）。

一个包含规范标识符的 URL 示例如下：

```
https://api.contoso.com/v1.0/people/7011042402/inbox
```

### 7.4 支持的方法
操作必须（MUST）尽可能使用合适的 HTTP 方法，并且必须（MUST）遵守操作幂等。

HTTP方法经常被称为HTTP动词。

这些术语在本上下文中是同义的，但是HTTP规范使用 “方法” 术语。

下面是 Microsoft REST 服务应该支持的方法的列表。并非所有资源都支持所有方法，但是所有使用以下方法的资源必须符合其用法。

方法  | 描述                                                                                                                | 是否幂等
------- | -------------------------------------------------------------------------------------------------------------------------- | -------------
GET     | 返回对象的当前值                                                                                      | True
PUT     | 替换对象或创建命名对象（如果适用）                                                               | True
DELETE  | 删除一个对象                                                                                                        | True
POST    | 基于提供的数据创建一个新对象，或提交命令                                                        | False
HEAD    | 返回GET响应的对象的元数据。 支持GET方法的资源也应（MAY）支持HEAD方法 | True
PATCH   | 对对象应用部分更新                                                                                        | False
OPTIONS | 获取有关请求的信息; 详情见下文                                                               | True

<small>表 1</small>

*译者注：* 幂等接口可以认为是这样一类接口，调用传参一样的话，返回值也都一样。有点类似纯函数的概念。

#### 7.4.1 POST
POST操作应该（SHOULD）支持 Location响 应头，以通过 Location 头指定任何未显式命名的已创建资源的位置。

例如，假设一个允许创建托管服务器的服务，它将由如下服务 URL 命名：

```http
POST http://api.contoso.com/account1/servers
```

响应将类似：

```http
201 Created
Location: http://api.contoso.com/account1/servers/server321
```

其中“server321”是服务分配的服务器名称。

服务还可以（MAY）在响应中返回创建的项目的完整元数据。

#### 7.4.2 PATCH
PATCH已经被 IETF 标准化为用于增量更新现有对象的方法（参见[RFC 5789] [rfc-5789]）。
符合 Microsoft REST API 准则的API应支持 PATCH。

#### 7.4.3 通过PATCH创建资源（UPSERT语义）
允许调用者在创建时指定键值的服务应(SHOULD)支持UPSERT语义，以及那些必须（MUST）支持使用PATCH创建资源的服务。
因为 PUT 被定义为内容的完全替换，所以客户端使用PUT修改数据是很危险的。

不了解（因此导致忽略）资源上的属性的客户端不太可能在尝试更新资源时在PUT上提供它们的值，因此这些属性可能（MAY）会无意中被删除。

服务可以(MAY)支持PUT来更新现有资源，但是如果他们这样做，它们必须(MUST)使用替换语义（即在PUT之后，资源的属性必须(MUST) 匹配请求中提供的属性，包括删除未提供的任何服务器属性）。

在UPSERT语义下，对不存在的资源的PATCH调用由服务器处理为“创建”，对已存在资源的PATCH调用被处理为“更新”。 为了确保更新请求不被视为创建或反之亦然，客户端可以在请求头中指定前置条件。

如果一个PATCH请求包含一个 If-Match 头，服务不能（MUST NOT）将PATCH请求作为一个新建来处理。如果包含一个值为“*”的If-None-Match头，服务不能（MUST NOT）将PATCH请求作为一个更新来处理。

如果服务不支持UPSERT，则对不存在的资源的PATCH调用必须抛出一个 HTTP "409 Conflict" 的错误。

#### 7.4.4 Options 和 link headers
OPTIONS 允许客户端检索有关资源的信息，服务通过返回 Allow 响应头来表明资源支持的有效方法，将检索的次数降到最低。

此外，服务应该（SHOULD）包括一个 Link 头（参见[RFC 5988] [rfc-5988]），以指向相关资源的文档：

```http
Link: <{help}>; rel="help"
```

其中{help}是文档资源的URL。

有关使用OPTIONS的示例，请参见[预检CORS跨域调用] [cors-preflight]。

### 7.5 标准请求头
Microsoft REST API 指南服务将使用下面的请求 Header 表。使用这些 Header 不是强制的，但如果使用它们必须一致使用。

所有请求头部的值必须遵循规范中规定的语法规则，其中定义了头部字段。

许多HTTP头在[RFC7231] [rfc-7231]中定义，但是可以在[IANA头文件注册表] [IANA-headers]中找到已批准头的完整列表。



Header                            | 类型                                  | 描述
--------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Authorization                     | String                                           | 请求的授权信息
Date                              | Date                                             | 请求的时间戳记，基于客户端的时钟，在[RFC 5322] [rfc-5322-3-3]日期和时间格式。 服务器不应该对客户端时钟的准确性做任何假设。 此 Header 可以包含在请求中，但在提供时必须采用此格式。 格林威治标准时间（GMT）必须在提供时用作此 Header 时区参考。 例如：'Wed，24 Aug 2016 18:41:30 GMT`。 请注意，GMT 与 UTC（协调世界时）一样被用于此目的。
Accept                            | Content type                                     | 客户端请求的响应内容类型，例如：<ul> <li> application/xml </li> <li> text/xml </li> <li> application/json </li> <li> text/javascript(for JSONP）</li> </ul>根据HTTP指南，这只是一个提示，响应可能有不同的内容类型，例如blob fetch，其中成功的响应将只是blob流作为有效载荷。 对于遵循 OData 的服务，应遵循 OData 中指定的优先级顺序。
Accept-Encoding                   | Gzip, deflate                                    | REST端应支持GZIP和DEFLATE编码（如果适用）。 对于非常大的资源，服务可以忽略并返回未压缩的数据。
Accept-Language                   | "en", "es", etc.                                 | 指定响应的首选语言。 服务不需要支持这个，但是如果一个服务支持本地化，它必须(MUST)通过Accept-Language头来这样做。
Accept-Charset                    | Charset type like "UTF-8"                        | 默认是UTF-8，但服务应该能(SHOULD)够处理ISO-8859-1。
Content-Type                      | Content type                                     | 请求正文的Mime类型（PUT / POST / PATCH）
Prefer                            | return=minimal, return=representation            | 如果指定了return = minimal，那么服务应该（SHOULD）在成功的插入或更新后返回一个空的 Body。 如果指定了return = representation，那么服务应该返回响应中创建或更新的资源。 如果有场景，客户端有时会从响应中获益，那么服务应该支持这个头部，但有时响应将施加太多的带宽上的压力。
If-Match, If-None-Match, If-Range | String                                           | 通过使用乐观并发控制的支持资源更新的服务必须（MUST）支持If-Match头字段。 服务也可以（MAY）使用与ETag相关的其他头字段，只要它们遵循HTTP规范。

### 7.6 标准响应头
服务应（SHOULD）返回以下响应头，除非在“必需”列中注明。

响应头    | 是否必须                                      | 描述
------------------ | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Date               | All responses                                 | 基于服务器时钟，以RFC 5322日期和时间格式处理响应的时间戳。 此报头必须包含在响应中。 格林威治标准时间（GMT）必须（MUST）用作此 Header 的时区参考。 例如：Wed，24 Aug 2016 18:41:30 GMT。 请注意，GMT 与 UTC（协调世界时）一样被用于此目的。
Content-Type       | All responses                                 | 响应内容类型
Content-Encoding   | All responses                                 | GZIP或DEFLATE
Preference-Applied | 当 Request 被指定时                     | 是否应用了在Prefer请求头中指示的首选项
ETag               | 当请求的资源具有实体标签时 | ETag响应头字段提供所请求变量的实体标签的当前值。 与If-Match，If-None-Match和If-Range一起使用，以实现乐观并发控制。

### 7.7 自定义头信息
对于给定API的基本操作，不得要求（MUST NOT）自定义 Header。

本文档中的一些准则规定使用非标准HTTP头。

此外，一些服务可能需要添加额外的功能，这是通过HTTP头暴露出来。

以下指南有助于在使用自定义标头时保持一致性。

不是标准HTTP标头的标头必须（MUST）采用以下两种格式之一：

1. 用IANA注册为“临时”的头的通用格式（[RFC 3864] [rfc-3864]）
2. 用于注册用途的 Header 的作用域格式

这两种格式会在下文详细描述。

### 7.8 将头指定为查询参数
有些 Header 对于某些场景（例如AJAX客户端）提出了挑战，特别是在进行跨域调用时，可能不支持添加 Header。
因此，除了头之外，一些头可以（MAY）被接受为查询参数，参数名字具有与头相同的命名：

并非所有标头都是查询参数，包括大多数标准HTTP标头。

考虑何时接受头作为参数的标准是：

1. 任何自定义头必须也必须（MUST）被接受为参数。
2. 必须接受的标准头也可以（MAY）作为参数。
3. 所需的具有安全敏感性的头（例如，授权头信息）可能不（MIGHT NOT）适合作为参数; 服务所有者应该根据具体情况评估这些。

这个规则的一个例外是Accept头。

通常的做法是使用具有简单名称的方案，而不是用于Accept的HTTP规范中描述的完整功能。

### 7.9 PII （Personal Identification Information）参数
根据其组织的隐私政策，客户端不应（SHOULD NOT）在URL中（作为路径或查询字符串的一部分）传输个人身份信息（PII）参数，因为此信息可能会通过客户端，网络和服务器日志和其他机制无意中显示。

因此，服务应该（SHOULD）接受作为报头发送的PII参数。

然而，在许多情况下，由于客户端或软件的限制，上述建议不能被遵循。为了解决这些限制，服务应该（SHOULD）也接受这些PII参数作为URL的一部分与本指南的其余部分一致。

接受PII参数的服务（无论是在网址中还是作为 Header）都应(SHOULD)符合其组织工程领导层指定的隐私权政策。

这通常包括建议客户端用于传输 PII 的报头，并且实施特别的预防措施以确保日志和其他服务数据收集被正确处理。

### 7.10 响应格式
为了使组织拥有成功的平台，它们必须以开发人员习惯使用的格式提供数据，并以一致的方式允许开发人员使用通用代码处理响应。

基于Web的通信，特别是当涉及移动或其他低带宽客户端时，由于各种原因已经朝着JSON的方向快速发展，包括其基于JavaScript的客户端的更轻重量和易于消费的倾向。

JSON 属性名应该（SHOULD）是驼峰风格的。

服务应该（SHOULD）提供 JSON 作为默认编码格式。

#### 7.10.1 客户端指定的响应格式
在 HTTP 中，客户端应该使用 Accept 头来请求响应数据格式。这是一个提示，并且服务器可以选择性的忽略它，即使这不是典型的良好的服务器。

客户端可以（MAY）发送多个 Accept 头，服务可以选择其中之一。

默认的响应格式（没有提供 Accept 头）应该（SHOULD）是 application/json，所有的服务必须（MUST）支持 application/json。


Accept Header    | Response type                      | 备注
---------------- | ---------------------------------- | -------------------------------------------
application/json | 有效载荷应该作为JSON返回 | 对于 JSONP 也接受 text/javascript 格式

```http
GET https://api.contoso.com/v1.0/products/user
Accept: application/json
```

#### 7.10.2 错误情况下的响应
对于非成功条件，开发人员应该能够编写统一逻辑来处理跨不同 Microsoft REST API 准则服务的错误。
这允许构建简单可靠的基础设施作为区别于成功响应的单独流程来处理异常。
以下基于OData v4 JSON 规范。
然而，它是非常通用的，不需要特定的 OData 结构。
API 应该（SHOULD）使用这种格式，即使他们没有使用其他 OData 结构

错误的响应必须（MUST）使用单个 JSON 对象。
该对象必须（MUST）有一个键值对，键命名为 “error”, 值必须（MUST）是一个 JSON 对象。

这个对象必须（MUST）包含名字为 “code” 和 “message“ 的键值对，也可以（MAY) 包含名字为 ”target“，”detail“ 或者 ”intererror“ 的键值对。

键值为 ”code“ 对应的值是一个语言无关的字符串。
值是一个服务自定义的错误码，并且应该（SHOULD）是可读性较好的。
这个 code 是对 HTTP 返回值错误码的一个补充，提供了更具体的信息。
服务应该（SHOULD）有一个相对较小的错误码集合（大约 20 个左右），并且所有的客户端必须（MUST）能够处理这些错误码。
大多数服务通常会有大量的具体的错误码，这些错误码对于客户端来说通常是不需要关心的。
这些错误码应该（SHOULD）定义在 ”intererror“ 这个键值对中。
新添加的 ”code“ 对于现有的客户端是可见的，因此是一个不兼容的改动，需要版本号增加。
服务端可以通过把新的错误码放在 ”intererror“ 里来避免不兼容的改动。

键值为 “message” 对应的值必须（MUST）能够清晰的表达错误信息。
这其实是给开发者的一个补充，不适合暴露给终端用户。
服务端如果想给终端用户提供一个合适的错误信息必须 （MUST）通过 [annotation][odata-json-annotations] 或者自定义属性。
服务不应该（SHOULD NOT）把 “message” 做本地化处理，因此这样可能（MAY）会导致开发者难以阅读，也使得错误信息难以通过网络来搜索。

键值为 “target” 对应的值对应着详细的错误信息（例如错误信息里属性的名称）。

键值为 ”detail“ 对应的值为必须（MUST）是包含了 ”code“ 和 “message” 键值对的 JOSN Array，也可能（MAY）包含了上述的 “target” 键值对。
“detail” 中的 JSON Array 里包含的对象通常蕴含了请求相关的错误信息。
示例如下。

键值为 “intererror” 对应的值必须（MUST）是一个对象。
对象的内容是服务自定义的。
服务如果想返回比根部更具体的错误信息必须（MUST）通过包含 “code” 和 “intererror” 的键值对。每一层嵌套的 “intererror” 对象代表了比上级更加具体的错误信息。
当处理错误的时候，客户端必须（MUST）遍历所有的嵌套的 “intererror” ，然后选择最深层次的错误信息。
这种模式允许服务在不影响兼容性的情况下引入新的错误代码，就的错误代码仍然存在，新的错误代码嵌套在层级关系中。
服务可以（MAY）根据不同的调用方返回不同深度和详情的错误信息。
例如，在开发环境中，最深层次的 “intererror“ 可以（MAY）包含帮助调试服务的内部信息。
为了保护信息安全不被泄露，服务应该（SHOULD）暴露太多的细节。
错误对象可能（MAY）包含服务端自定义的键值对，这些键值对可能（MAY）根据 code 而变化。
服务端自定义的错误类型应该（SHOULD）在服务的元数据文档里进行声明。
示例如下。

返回值中的错误信息可以（MAY）在任意的 JSON 对象中包含[annotations][odata-json-annotations]。

我们建议对于服务出现短暂的错误，可以有重试机制，服务的返回的 Header 应该（SHOULD）包含头 Retry-After，来表明客户端最短的重试时间。


##### ErrorResponse : Object

属性 | 类型 | 必须 | 描述
-------- | ---- | -------- | -----------
`error` | Error | ✔ | 错误对象

##### Error : Object

属性 | 类型 | 必须 | 描述
-------- | ---- | -------- | -----------
`code` | String (enumerated) | ✔ | 服务定义的错误码中的一个。
`message` | String | ✔ | 人类可读的错误描述信息。
`target` | String |  | 错误的 ”target“
`details` | Error[] |  | 关于此错误的一组详细信息。
`innererror` | InnerError |  | 包含更具体的信息的错误对象。

##### InnerError : Object

Property | Type | Required | Description
-------- | ---- | -------- | -----------
`code` | String |  | A more specific error code than was provided by the containing error.
`innererror` | InnerError |  | An object containing more specific information than the current object about the error.

##### 示例

"innererror" 示例:

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Previous passwords may not be reused",
    "target": "password",
    "innererror": {
      "code": "PasswordError",
      "innererror": {
        "code": "PasswordDoesNotMeetPolicy",
        "minLength": "6",
        "maxLength": "64",
        "characterTypes": ["lowerCase","upperCase","number","symbol"],
        "minDistinctCharacterTypes": "2",
        "innererror": {
          "code": "PasswordReuseNotAllowed"
        }
      }
    }
  }
}
```
在这个示例中，最基本的错误代码是 ”BadArgument“，但也是客户端最关心的，”intererror“ 里包含了更具体的错误代码。
”PasswordReuseNotAllowed“ 可能是服务中后来被加入的，再次之前只返回上层的 ”PasswordDoesNotMeetPolicy“ 的错误码。
”PasswordDoesNotMeetPolicy“ 错误同时也包含了附加的键值对，这允许客户端根据服务端的配置动态的判断用户的输入是否符合要求，或者把服务端的约束做本地化处理后显示给用户。

"details" 示例:

```json
{
  "error": {
    "code": "BadArgument",
    "message": "Multiple errors in ContactInfo data",
    "target": "ContactInfo",
    "details": [
      {
        "code": "NullValue",
        "target": "PhoneNumber",
        "message": "Phone number must not be null"
      },
      {
        "code": "NullValue",
        "target": "LastName",
        "message": "Last name must not be null"
      },
      {
        "code": "MalformedValue",
        "target": "Address",
        "message": "Address is not valid"
      }
    ]
  }
}
```

在这个示例中，次请求出现了多个问题，问题分别罗列在 ”detail“ 属性中。


### 7.11 HTTP 状态码
应该（SHOULD）使用标准的 HTTP 状态码；此部分请参考标准的 HTTP 状态码定义。

### 7.12 客户端可选库
开发者必须（MUST）可以在不同的平台或者使用不同的语言来进行开发，例如 Windows，MacOS，Linux，C#，Python，Node.js，或者是 Ruby。

服务应该（SHOULD）可以被简单的 HTTP 请求工具访问，例如不费吹灰之力的 curl。

服务开发入口应该（SHOULD）提供类似 ”获取开发者 Token“ 的工具来方便的实验，为 curl 提供支持。

## 8 CORS
符合 Microsoft REST API 指南的服务必须（MUST）支持[CORS（跨源资源共享）][cors]。
服务应该（SHOULD）支持 CORS * 的允许来源，并通过有效的 OAuth 令牌实施授权。
服务不应该（SHOULD NOT）支持具有源验证的用户凭证。
特殊情况可能（MAY）有例外。

### 8.1 客户端指南
Web 开发通常不需要做什么特殊处理就可以享受 CORS 的优势。
标准的 XMLHttpRequest 屏蔽了 HTTP 握手过程的细节。

其他的平台，例如 .NET 已经提供了 CORS 的支持。

#### 8.1.1 避免预检
因为 CORS 协议可以触发预检请求，从而向服务器发送了额外的网络请求，所以性能关键型应用程序可能会针对性的避免额外的网络请求。
CORS 背后的精神是避免旧的，没有 CORS 的浏览器进行的任何简单跨域请求进行预检。
所有其他请求需要预检。

如果其方法是 GET，HEAD 或 POST，并且除了 Accept，Accept-Language 和 Content-Language 之外不包含任何请求头，那么我们称这个请求是”简单的”，同时会避免预检。
对于 POST 请求，还允许 Content-Type 头，但前提是其值为 “application/x-www-form-urlencoded”，“multipart/form-data” 或 “text/plain”。
对于任何其他头或请求值，将会发生预检请求。

### 8.2 服务端指南
最坏的情况下， 服务必须（MUST）:

- 了解浏览器在跨域请求上发送的 Origin 请求头，以及在检查访问权限的预检 OPTIONS 请求上发送的 Access-Control-Request-Method 请求头。
- 如果请求中存在 Origin 头:
	- 如果请求使用 OPTIONS 方法并包含 Access-Control-Request-Method 头，那么它是一个预检请求，用于在实际请求之前探测访问。否则，它是一个实际请求。对于预检请求，除了执行以下步骤添加头部之外，服务必须（MUST）不执行额外的处理并且必须（MUST）返回 200 OK。对于非预检请求，除了请求的常规处理之外，还添加以下标头。
	- 向响应添加 Access-Control-Allow-Origin 头，其中包含与Origin请求头相同的值。注意，这需要服务来动态生成头值。不需要Cookie或任何其他形式的[用户凭据] [cors-user-credentials]的资源可以（MAY）使用通配符星号（*）进行响应。请注意，通配符在这里是可以接受的，而不是下面描述的任何其他标头。
	- 如果调用者需要访问不在[简单响应头] [cors-simple-headers]（Cache-Control，Content-Language，Content-Type，Expires，Last-Modified，Pragma）集合中的响应头，然后添加一个 Access-Control-Expose-Headers 头，其中包含客户端应该具有访问权限的附加响应头名称列表。
	- 如果请求需要 Cookie ，请添加值为 “true” 的 Access-Control-Allow-Credentials 标头。
	- 如果请求是预检请求（见第一个项），那么服务必须（MUST）：
		- 添加一个 Access-Control-Allow-Headers 响应头，其中包含客户端允许使用的请求头名称列表。此列表只需要包含不在[简单请求头] [cors-simple-headers]（Accept，Accept-Language，Content-Language）集合中的头。如果服务接受的报头没有限制，则服务可以（MAY）简单地返回与客户端发送的访问控制请求报头头相同的值。
		- 添加一个 Access-Control-Allow-Methods 响应头，其中包含允许调用者使用的HTTP方法列表。
    


添加前缀为 Access-Control-Max-Age 的响应头，其中包含此预检响应有效的秒数（因此可以在后续实际请求之前避免再次发送预检请求）。 请注意，虽然习惯上使用像2592000（30天）这样的大值，但是许多浏览器自我强加了更低的限制（例如5分钟）。

因为浏览器预检响应缓存非常弱，来自预检响应的额外往返请求会伤害性能。性能至关重要的交互式 Web 客户端使用的服务应（SHOULD）避免使用引起预检请求的模式。

- 对于 GET 和 HEAD 调用，避免要求不是上述简单集合的一部分的请求标头。可以让它们作为查询参数提供。
   - Authorization 头不是简单集合的一部分，因此认证令牌务必通过 “access_token” 查询参数发送，用于需要认证的资源。请注意，不建议在URL中传递身份验证令牌，因为它可能导致令牌记录在服务器日志中，并暴露给有权访问这些日志的任何人。通过 URL 接受身份验证令牌的服务必须采取措施减轻安全风险，例如使用短期身份验证令牌，抑制身份验证令牌被记录，以及控制对服务器日志的访问。

- 避免需要 Cookie 。如果设置了 “withCredentials” 属性，则 XmlHttpRequest 只会在跨网域请求上发送 Cookie ;这也会导致预检请求。
   - 需要基于Cookie的身份验证的服务必须（MUST）使用 “dynamic canary” 来保护所有接受 Cookie 的 API。

- 对于 POST 调用，在适用的情况下，更喜欢在（“application / x-www-form-urlencoded，”“multipart / form-data”，“text / plain”）集合中的简单内容类型。任何其他 Content-Type 将引起预检请求。
   - 服务不得（MUST NOT）以避免 CORS 预检请求的名义违反其他API建议。特别地，根据建议，由于 Content-Type，大多数POST请求实际上将需要预检请求。
   - 如果消除预检是至关重要的，那么服务可能（MAY）支持数据传输的替代机制，但也必须（MUST）支持建议的方法。

此外，在适当的时机，服务可以（MAY）支持JSONP模式，以用于简单的，仅GET的跨域访问。
在 JSONP 中，服务接受一个参数，指示格式（ _format = json_ ）和一个指示回调的参数（ _callback = someFunc_ ），并返回一个 text/ javascript 文档，其中回调函数中包裹了 JSON 格式的响应数据，

更多关于 JSONP 的介绍请移步维基百科 [JSONP](https://en.wikipedia.org/wiki/JSONP)。

## 9 Collections
### 9.1 Item keys
Services MAY support durable identifiers for each item in the collection, and that identifier SHOULD be represented in JSON as "id". These durable identifiers are often used as item keys.

Collections that support durable identifiers MAY support delta queries.

### 9.2 Serialization
Collections are represented in JSON using standard array notation.

### 9.3 Collection URL patterns
Collections are located directly under the service root when they are top level, or as a segment under another resource when scoped to that resource.

For example:

```http
GET https://api.contoso.com/v1.0/people
```

Whenever possible, services MUST support the "/" pattern.
For example:

```http
GET https://{serviceRoot}/{collection}/{id}
```

Where:
- {serviceRoot} – the combination of host (site URL) + the root path to the service
- {collection} – the name of the collection, unabbreviated, pluralized
- {id} – the value of the unique id property. When using the "/" pattern this MUST be the raw string/number/guid value with no quoting but properly escaped to fit in a URL segment.

#### 9.3.1    Nested collections and properties
Collection items MAY contain other collections.
For example, a user collection MAY contain user resources that have multiple addresses:

```http
GET https://api.contoso.com/v1.0/people/123/addresses
```

```json
{
  "value": [
    { "street": "1st Avenue", "city": "Seattle" },
    { "street": "124th Ave NE", "city": "Redmond" }
  ]
}
```

### 9.4 Big collections
As data grows, so do collections.
Planning for pagination is important for all services.
Therefore, when multiple pages are available, the serialization payload MUST contain the opaque URL for the next page as appropriate.
Refer to the paging guidance for more details.

Clients MUST be resilient to collection data being either paged or nonpaged for any given request.

```json
{
  "value":[
    { "id": "Item 1","price": 99.95,"sizes": null},
    { … },
    { … },
    { "id": "Item 99","price": 59.99,"sizes": null}
  ],
  "@nextLink": "{opaqueUrl}"
}
```

### 9.5 Changing collections
POST requests are not idempotent.
This means that two POST requests sent to a collection resource with exactly the same payload MAY lead to multiple items being created in that collection.
This is often the case for insert operations on items with a server-side generated id.

For example, the following request:

```http
POST https://api.contoso.com/v1.0/people
```

Would lead to a response indicating the location of the new collection item:

```http
201 Created
Location: https://api.contoso.com/v1.0/people/123
```

And once executed again, would likely lead to another resource:

```http
201 Created
Location: https://api.contoso.com/v1.0/people/124
```

While a PUT request would require the indication of the collection item with the corresponding key instead:

```http
PUT https://api.contoso.com/v1.0/people/123
```

### 9.6 Sorting collections
The results of a collection query MAY be sorted based on property values.
The property is determined by the value of the _$orderBy_ query parameter.

The value of the _$orderBy_ parameter contains a comma-separated list of expressions used to sort the items.
A special case of such an expression is a property path terminating on a primitive property.

The expression MAY include the suffix "asc" for ascending or "desc" for descending, separated from the property name by one or more spaces.
If "asc" or "desc" is not specified, the service MUST order by the specified property in ascending order.

NULL values MUST sort as "less than" non-NULL values.

Items MUST be sorted by the result values of the first expression, and then items with the same value for the first expression are sorted by the result value of the second expression, and so on.
The sort order is the inherent order for the type of the property.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name
```

Will return all people sorted by name in ascending order.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc
```

Will return all people sorted by name in descending order.

Sub-sorts can be specified by a comma-separated list of property names with OPTIONAL direction qualifier.

For example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name desc,hireDate
```

Will return all people sorted by name in descending order and a secondary sort order of hireDate in ascending order.

Sorting MUST compose with filtering such that:

```http
GET https://api.contoso.com/v1.0/people?$filter=name eq 'david'&$orderBy=hireDate
```

Will return all people whose name is David sorted in ascending order by hireDate.

#### 9.6.1 Interpreting a sorting expression
Sorting parameters MUST be consistent across pages, as both client and server-side paging is fully compatible with sorting.

If a service does not support sorting by a property named in a _$orderBy_ expression, the service MUST respond with an error message as defined in the Responding to Unsupported Requests section.

### 9.7 Filtering
The _$filter_ querystring parameter allows clients to filter a collection of resources that are addressed by a request URL.
The expression specified with _$filter_ is evaluated for each resource in the collection, and only items where the expression evaluates to true are included in the response.
Resources for which the expression evaluates to false or to null, or which reference properties that are unavailable due to permissions, are omitted from the response.

Example: return all Products whose Price is less than $10.00

```http
GET https://api.contoso.com/v1.0/products?$filter=price lt 10.00
```

The value of the _$filter_ option is a Boolean expression.

#### 9.7.1 Filter operations
Services that support _$filter_ SHOULD support the following minimal set of operations.

Operator             | Description           | Example
-------------------- | --------------------- | -----------------------------------------------------
Comparison Operators |                       |
eq                   | Equal                 | city eq 'Redmond'
ne                   | Not equal             | city ne 'London'
gt                   | Greater than          | price gt 20
ge                   | Greater than or equal | price ge 10
lt                   | Less than             | price lt 20
le                   | Less than or equal    | price le 100
Logical Operators    |                       |
and                  | Logical and           | price le 200 and price gt 3.5
or                   | Logical or            | price le 3.5 or price gt 200
not                  | Logical negation      | not price le 3.5
Grouping Operators   |                       |
( )                  | Precedence grouping   | (priority eq 1 or city eq 'Redmond') and price gt 100

#### 9.7.2 Operator examples
The following examples illustrate the use and semantics of each of the logical operators.

Example: all products with a name equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk'
```

Example: all products with a name not equal to 'Milk'

```http
GET https://api.contoso.com/v1.0/products?$filter=name ne 'Milk'
```

Example: all products with the name 'Milk' that also have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' and price lt 2.55
```

Example: all products that either have the name 'Milk' or have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=name eq 'Milk' or price lt 2.55
```

Example 54: all products that have the name 'Milk' or 'Eggs' and have a price less than 2.55:

```http
GET https://api.contoso.com/v1.0/products?$filter=(name eq 'Milk' or name eq 'Eggs') and price lt 2.55
```

#### 9.7.3 Operator precedence
Services MUST use the following operator precedence for supported operators when evaluating _$filter_ expressions.
Operators are listed by category in order of precedence from highest to lowest.
Operators in the same category have equal precedence:

Group           | Operator | Description
--------------- | -------- | ---------------------
Grouping        | ( )      | Precedence grouping
Unary           | not      | Logical Negation
Relational      | gt       | Greater Than
                | ge       | Greater than or Equal
                | lt       | Less Than
                | le       | Less than or Equal
Equality        | eq       | Equal
                | ne       | Not Equal
Conditional AND | and      | Logical And
Conditional OR  | or       | Logical Or

### 9.8 Pagination
RESTful APIs that return collections MAY return partial sets.
Consumers of these services MUST expect partial result sets and correctly page through to retrieve an entire set.

There are two forms of pagination that MAY be supported by RESTful APIs.
Server-driven paging mitigates against denial-of-service attacks by forcibly paginating a request over multiple response payloads.
Client-driven paging enables clients to request only the number of resources that it can use at a given time.

Sorting and Filtering parameters MUST be consistent across pages, because both client- and server-side paging is fully compatible with both filtering and sorting.

#### 9.8.1 Server-driven paging
Paginated responses MUST indicate a partial result by including a continuation token in the response.
The absence of a continuation token means that no additional pages are available.

Clients MUST treat the continuation URL as opaque, which means that query options may not be changed while iterating over a set of partial results.

Example:

```http
GET http://api.contoso.com/v1.0/people HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...],
  "@nextLink": "{opaqueUrl}"
}
```

#### 9.8.2 Client-driven paging
Clients MAY use _$top_ and _$skip_ query parameters to specify a number of results to return and an offset.
The server SHOULD honor the values specified by the client; however, clients MUST be prepared to handle responses that contain a different page size or contain a continuation token.

Note: If the server can't honor _$top_ and/or _$skip_, the server MUST return an error to the client informing about it instead of just ignoring the query options.
This will avoid the risk of the client making assumptions about the data returned.

Example:

```http
GET http://api.contoso.com/v1.0/people?$top=5&$skip=2 HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  ...,
  "value": [...]
}
```

#### 9.8.3 Additional considerations
**Stable order prerequisite:** Both forms of paging depend on the collection of items having a stable order.
The server MUST supplement any specified order criteria with additional sorts (typically by key) to ensure that items are always ordered consistently.

**Missing/repeated results:** Even if the server enforces a consistent sort order, results MAY be missing or repeated based on creation or deletion of other resources.
Clients MUST be prepared to deal with these discrepancies.
The server SHOULD always encode the record ID of the last read record, helping the client in the process of managing repeated/missing results.

**Combining client- and server-driven paging:** Note that client-driven paging does not preclude server-driven paging.
If the page size requested by the client is larger than the default page size supported by the server, the expected response would be the number of results specified by the client, paginated as specified by the server paging settings.

**Page Size:** Clients MAY request server-driven paging with a specific page size by specifying a _$maxpagesize_ preference.
The server SHOULD honor this preference if the specified page size is smaller than the server's default page size.

**Paginating embedded collections:** It is possible for both client-driven paging and server-driven paging to be applied to embedded collections.
If a server paginates an embedded collection, it MUST include additional continuation tokens as appropriate.

**Recordset count:** Developers who want to know the full number of records across all pages, MAY include the query parameter _$count=true_ to tell the server to include the count of items in the response.

### 9.9 Compound collection operations
Filtering, Sorting and Pagination operations MAY all be performed against a given collection.
When these operations are performed together, the evaluation order MUST be:

1. **Filtering**. This includes all range expressions performed as an AND operation.
2. **Sorting**. The potentially filtered list is sorted according to the sort criteria.
3. **Pagination**. The materialized paginated view is presented over the filtered, sorted list. This applies to both server-driven pagination and client-driven pagination.

## 10 Delta queries
Services MAY choose to support delta queries.

### 10.1 Delta links
Delta links are opaque, service-generated links that the client uses to retrieve subsequent changes to a result.

At a conceptual level delta links are based on a defining query that describes the set of results for which changes are being tracked.
The delta link encodes the collection of entities for which changes are being tracked, along with a starting point from which to track changes.

If the query contains a filter, the response MUST include only changes to entities matching the specified criteria.
The key principles of the Delta Query are:
- Every item in the set MUST have a persistent identifier. That identifier SHOULD be represented as "id". This identifier is a service defined opaque string that MAY be used by the client to track object across calls.
- The delta MUST contain an entry for each entity that newly matches the specified criteria, and MUST contain a "@removed" entry for each entity that no longer matches the criteria.
- Re-evaluate the query and compare it to original set of results; every entry uniquely in the current set MUST be returned as an Add operation, and every entry uniquely in the original set MUST be returned as a "remove" operation.
- Each entity that previously did not match the criteria but matches it now MUST be returned as an "add"; conversely, each entity that previously matched the query but no longer does MUST be returned as a "@removed" entry.
- Entities that have changed MUST be included in the set using their standard representation.
- Services MAY add additional metadata to the "@removed" node, such as a reason for removal, or a "removed at" timestamp. We recommend teams coordinate with the Microsoft REST API Guidelines Working Group on extensions to help maintain consistency.

The delta link MUST NOT encode any client top or skip value.

### 10.2 Entity representation
Added and updated entities are represented in the entity set using their standard representation.
From the perspective of the set, there is no difference between an added or updated entity.

Removed entities are represented using only their "id" and an "@removed" node.
The presence of an "@removed" node MUST represent the removal of the entry from the set.

### 10.3 Obtaining a delta link
A delta link is obtained by querying a collection or entity and appending a $delta query string parameter.
For example:

```http
GET https://api.contoso.com/v1.0/people?$delta
HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Matt"},
    { "id": "2", "name": "Mark"},
    { "id": "3", "name": "John"},
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

Note: If the collection is paginated the deltaLink will only be present on the final page but MUST reflect any changes to the data returned across all pages.

### 10.4 Contents of a delta link response
Added/Updated entries MUST appear as regular JSON objects, with regular item properties.
Returning the added/modified items in their regular representation allows the client to merge them into their existing "cache" using standard merge concepts based on the "id" field.

Entries removed from the defined collection MUST be included in the response.
Items removed from the set MUST be represented using only their "id" and an "@removed" node.

### 10.5 Using a delta link
The client requests changes by invoking the GET method on the delta link.
The client MUST use the delta URL as is -- in other words the client MUST NOT modify the URL in any way (e.g., parsing it and adding additional query string parameters).
In this example:

```http
GET https://{opaqueUrl} HTTP/1.1
Accept: application/json

HTTP/1.1 200 OK
Content-Type: application/json

{
  "value":[
    { "id": "1", "name": "Mat"},
    { "id": "2", "name": "Marc"},
    { "id": "3", "@removed": {} },
    { "id": "4", "name": "Luc"}
  ],
  "@deltaLink": "{opaqueUrl}"
}
```

The results of a request against the delta link may span multiple pages but MUST be ordered by the service across all pages in such a way as to ensure a deterministic result when applied in order to the response that contained the delta link.

If no changes have occurred, the response is an empty collection that contains a delta link for subsequent changes if requested.
This delta link MAY be identical to the delta link resulting in the empty collection of changes.

If the delta link is no longer valid, the service MUST respond with _410 Gone_. The response SHOULD include a Location header that the client can use to retrieve a new baseline set of results.

## 11 JSON 标准化
### 11.1 原始类型的JSON格式化标准
基本值序列化到JSON必须要符合[RFC4627][rfc-4627]的标准。

### 11.2 日期和时间
#### 11.2.1 生成日期
服务生成的日期必须符合 `DateLiteral` 格式，除非有其他不可抗力，不然都应当遵循 `Iso8601Literal` 格式。
用了 `StructuredDateLiteral` 格式的服务不能生成 `T` 种类的日期，除非在有额外的精度需求且 ECMAScript 客户端明确不支持这种情况下。
（非标准声明：当决定使用哪种 `DateKind` 标准化时，大致顺序是`E, C, U, W, O, X, I, T`。这种顺序对 ECMAScript， .NET 和 C++程序是最优的。）

#### 11.2.2 使用日期
服务必须接受使用了同样 `DateLiteral`（如果可用，包括`DateKind`）格式的客户端生成的日期，也应该接受使用了任何 `DateLiteral` 格式的日期。

#### 11.2.3 兼容性
对于同样 `DateLiteral` 格式（如果可用，包括`DateKind`）类型的资源，服务必须使用同样的类型，在整个服务中，所有资源都应该这样。

服务产生的 `DateLiteral` 变化格式（如果可用，包括 `DateKind` ）或者服务接受的 `DateLiteral` 简化格式（如果可用，包括`DateKind`）都必须当成重大变更处理。对于服务接受的 `DateLiteral` 扩展格式不用当成一个重大改变。

### 11.3 日期和时间的JSON序列化
日期时间与JSON的相互转化是一个难题。
尽管 ECMAScript 支持大部分字面内置类型，但并没有定义一个日期字面格式。
网络上支持的是 [ISO 8601 时间格式的 ECMAScript 子集][iso-8601]，但是有些情况下需要其他格式。
对于那些情况，这篇文档定义了一个不同格式下明确代表日期的 JSON 序列化格式。
其他序列化格式（例如XML）可以通过这种格式推导出来。

#### 11.3.1 `DateLiteral` 格式
 JSON 形式表达的日期通过以下语法来序列化。
通常来说，一个 `DateValue` 如果不是 ISO 8601 标准的字符串，那就是同时包含  `kind` 和 `value` 这两个定义了一个时间节点属性的 JSON 对象。
下面内容是一个与上下文有关的语法；虽然 `DateValue` 的解析非常依赖于 `DateKind` 的值，但是这样也减少了用来描述格式的内容。

```
DateLiteral:
  Iso8601Literal
  StructuredDateLiteral

Iso8601Literal:
  定义于 http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15 中的一个字符串文本。注意 ISO 8601 的全语法（如无分隔符的 "basic format"）并不支持。
  除特例外，所有日期都默认为UTC。

StructuredDateLiteral:
  { DateKindProperty , DateValueProperty }
  { DateValueProperty , DateKindProperty }

DateKindProperty
  "kind" : DateKind

DateKind:
  "C"            ; 如下
  "E"            ; 如下
  "I"            ; 如下
  "O"            ; 如下
  "T"            ; 如下
  "U"            ; 如下
  "W"            ; 如下
  "X"            ; 如下

DateValueProperty:
  "value" : DateValue

DateValue:
  UnsignedInteger        ; 此处未定义
  SignedInteger        ; 此处未定义
  RealNumber        ; 此处未定义
  Iso8601Literal        ; 此处未定义
```

#### 11.3.2 日期格式化的注解
一个使用了 `Iso8601Literal` 的 `DateLiteral` 是较为简单的。
下面举个例子，属性名为 `creationDate` ，值为 UTC 时间 2015年2月13日下午1:15 的对象：

```json
{ "creationDate" : "2015-02-13T13:15Z" }
```
`StructuredDateLiteral` 由 `DateKind` 和 `DateValue` 同时组成。`DataValue` 的有效值（与值的含义）依赖于 `DateKind`。下面的表格描述了它们的有效组合和含义：

DateKind | DateValue       | 简称 & 说明                                                                                                                  | 更多
-------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------
C        | UnsignedInteger | "CLR"; 0001.01.01 00:00 以来的毫秒数； 不可为负值。 *下有说明*                                  | [MSDN][clr-time]
E        | SignedInteger   | "ECMAScript"; 1970.01.01 00:00以来的毫秒数。                                                                            | [ECMA International][ecmascript-time]
I        | Iso8601Literal  | "ISO 8601"; 仅在 ECMAScript 标准中使用。                                                                                           |
O        | RealNumber      | "OLE Date"; 整数部分为1899.12.31 00:00以来的天数， 小数部分是一天内的时间 (0.5 = 中午). | [MSDN][ole-date]
T        | SignedInteger   | "Ticks"; 1601.01.01 00：:00以来的时钟（100纳秒间隔）数。 *下有说明*                                             | [MSDN][ticks-time]
U        | SignedInteger   | "UNIX"; 1970.01.01 00:00以来的秒数。                                                                                        | [MSDN][unix-time]
W        | SignedInteger   | "Windows"; 1601.01.01 00:00以来的毫秒数。*下有说明*                                                               | [MSDN][windows-time]
X        | RealNumber      | "Excel"; 等同于 `O` 型除了1900年未被正确当做闰年，还有第0天表示 "一月 0 (零)。"                                     | [Microsoft Support][excel-time]

**`C` 类和 `W` 类的重要提醒:** 本地 CLR 和 Windows 时间都由100纳秒 "时钟" 数代表。
为了和被限制了精度的 ECMAScript 客户端交互操作，这些被当做 `DateLiteral` 操作的数值 ，在序列化和反序列化时 ，_必须从毫秒转化，也必须转化到毫秒_ 。
1毫秒等于10,000时钟数。

**`T` 类的重要提醒:** 这种类型保存了 Windows 本地时间类型的完整精度（且与 CLR 格式可以轻易相互转化），但与 ECMAScript 客户端不兼容。
因此，只有在需要额外精度，且不与 ECMAScript 客户端交互的场景种才能使用。

下面例子同样是属性名为 creationDate 的对象，值为 2015.02.13下午1:15，几种表达格式：

```json
[
  { "creationDate" : { "kind" : "O", "value" : 42048.55 } },
  { "creationDate" : { "kind" : "E", "value" : 1423862100000 } }
]
```

分离 kind 和 value 的一个好处就是，当一个客户端知道此kind由哪个服务使用，不需要使用其他语法分析就可以解析数值。
通常情况下，值会是一个数字，会使编程更容易：

```csharp
//这个服务总会给出 ECMAScript 格式的日期
var date = new Date(serverResponse.someObject.creationDate.value);
```

### 11.4 持续时长
[持续时长][wikipedia-iso8601-durations] 序列化时需遵循 [ISO 8601][wikipedia-iso8601-durations]。
持续时长通过 `P[n]Y[n]M[n]DT[n]H[n]M[n]S` 格式表示。
标准化表示：
- P 是持续时长的标志 (旧称 "period") ，放至在持续时长表示开头。
- Y 标志年份，前面是年份数量。
- M 标志月份，前面是月份数量。
- W 标志星期，前面是星期数量。
- D 标志天数，前面是天数数量。
- T 标志时间，前面是时间区块之前出现。
- H 标志小时，前面是小时数量。
- M 标志分钟，前面是分钟数量。
- S 标志秒钟，前面是秒钟数量。

例如，"P3Y6M4DT12H30M5S" 代表持续时长为 "三年，六个月，四天，12小时，30分钟，和5秒。"

### 11.5 间隔
[ISO 8601][wikipedia-iso8601-intervals] 中定义了 [间隔][wikipedia-iso8601-intervals]。
- 开始时间和结束时间，例如 "2007-03-01T13:00:00Z/2008-05-11T15:30:00Z"
- 开始时间和持续时长，例如 "2007-03-01T13:00:00Z/P1Y2M10DT2H30M"
- 持续时长和结束时间，例如 "P1Y2M10DT2H30M/2008-05-11T15:30:00Z"
- 只有持续时长，例如 "P1Y2M10DT2H30M," 带有额外的上下文信息

### 11.6 重复间隔
[重复间隔][wikipedia-iso8601-repeatingintervals]，按照 [ISO 8601][wikipedia-iso8601-repeatingintervals]：

> 在间隔表达式之前添加 "R[n]/"来构成重复间隔表示，n表示重复次数。
省略n值表示无限重复。

例如，开始时间为 "2008-03-01T13:00:00Z," 重复 "P1Y2M10DT2H30M" 5次，表示为 "R5/2008-03-01T13:00:00Z/P1Y2M10DT2H30M."

## 12 Versioning
**All APIs compliant with the Microsoft REST API Guidelines MUST support explicit versioning.** It's critical that clients can count on services to be stable over time, and it's critical that services can add features and make changes.

### 12.1 Versioning formats
Services are versioned using a Major.Minor versioning scheme.
Services MAY opt for a "Major" only version scheme in which case the ".0" is implied and all other rules in this section apply.
Two options for specifying the version of a REST API request are supported:
- Embedded in the path of the request URL, at the end of the service root: `https://api.contoso.com/v1.0/products/users`
- As a query string parameter of the URL: `https://api.contoso.com/products/users?api-version=1.0`

Guidance for choosing between the two options is as follows:

1. Services co-located behind a DNS endpoint MUST use the same versioning mechanism.
2. In this scenario, a consistent user experience across the endpoint is paramount. The Microsoft REST API Guidelines Working Group recommends that new top-level DNS endpoints are not created without explicit conversations with your organization's leadership team.
3. Services that guarantee the stability of their REST API's URL paths, even through future versions of the API, MAY adopt the query string parameter mechanism. This means the naming and structure of the relationships described in the API cannot evolve after the API ships, even across versions with breaking changes.
4. Services that cannot ensure URL path stability across future versions MUST embed the version in the URL path.

Certain bedrock services such as Microsoft's Azure Active Directory may be exposed behind multiple endpoints.
Such services MUST support the versioning mechanisms of each endpoint, even if that means supporting multiple versioning mechanisms.

#### 12.1.1 Group versioning
Group versioning is an OPTIONAL feature that MAY be offered on services using the query string parameter mechanism.
Group versions allow for logical grouping of API endpoints under a common versioning moniker.
This allows developers to look up a single version number and use it across multiple endpoints.
Group version numbers are well known, and services SHOULD reject any unrecognized values.

Internally, services will take a Group Version and map it to the appropriate Major.Minor version.

The Group Version format is defined as YYYY-MM-DD, for example 2012-12-07 for December 7, 2012. This Date versioning format applies only to Group Versions and SHOULD NOT be used as an alternative to Major.Minor versioning.

##### 12.1.1.1 Examples of group versioning

Group      | Major.Minor
---------- | -----------
2012-12-01 | 1.0
           | 1.1
           | 1.2
2013-03-21 | 1.0
           | 2.0
           | 3.0
           | 3.1
           | 3.2
           | 3.3

Version Format                | Example                | Interpretation
----------------------------- | ---------------------- | ------------------------------------------
{groupVersion}                | 2013-03-21, 2012-12-01 | 3.3, 1.2
{majorVersion}                | 3                      | 3.0
{majorVersion}.{minorVersion} | 1.2                    | 1.2

Clients can specify either the group version or the Major.Minor version:

For example:

```http
GET http://api.contoso.com/acct1/c1/blob2?api-version=1.0
```

```http
PUT http://api.contoso.com/acct1/c1/b2?api-version=2011-12-07
```

### 12.2 When to version
Services MUST increment their version number in response to any breaking API change.
See the following section for a detailed discussion of what constitutes a breaking change.
Services MAY increment their version number for nonbreaking changes as well, if desired.

Use a new major version number to signal that support for existing clients will be deprecated in the future.
When introducing a new major version, services MUST provide a clear upgrade path for existing clients and develop a plan for deprecation that is consistent with their business group's policies.
Services SHOULD use a new minor version number for all other changes.

Online documentation of versioned services MUST indicate the current support status of each previous API version and provide a path to the latest version.

### 12.3 Definition of a breaking change
Changes to the contract of an API are considered a breaking change.
Changes that impact the backwards compatibility of an API are a breaking change.

Teams MAY define backwards compatibility as their business needs require.
For example, Azure defines the addition of a new JSON field in a response to be not backwards compatible.
Office 365 has a looser definition of backwards compatibility and allows JSON fields to be added to responses.

Clear examples of breaking changes:

1. Removing or renaming APIs or API parameters
2. Changes in behavior for an existing API
3. Changes in Error Codes and Fault Contracts
4. Anything that would violate the [Principle of Least Astonishment][principle-of-least-astonishment]

Services MUST explicitly define their definition of a breaking change, especially with regard to adding new fields to JSON responses and adding new API arguments with default fields.
Services that are co-located behind a DNS Endpoint with other services MUST be consistent in defining contract extensibility.

The applicable changes described [in this section of the OData V4 spec][odata-breaking-changes] SHOULD be considered part of the minimum bar that all services MUST consider a breaking change.

## 13 Long running operations
Long running operations, sometimes called async operations, tend to mean different things to different people.
This section sets forth guidance around different types of long running operations, and describes the wire protocols and best practices for these types of operations.

1. One or more clients MUST be able to monitor and operate on the same resource at the same time.
2. The state of the system SHOULD be discoverable and testable at all times. Clients SHOULD be able to determine the system state even if the operation tracking resource is no longer active. The act of querying the state of a long running operation should itself leverage principles of the web. i.e. well defined resources with uniform interface semantics. Clients MAY issue a GET on some resource to determine the state of a long running operation
3. Long running operations SHOULD work for clients looking to "Fire and Forget" and for clients looking to actively monitor and act upon results.
4. Cancellation does not explicitly mean rollback. On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc. Following a cancelled operation, It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

### 13.1 Resource based long running operations (RELO)
Resource based modeling is where the status of an operation is encoded in the resource and the wire protocol used is the standard synchronous protocol.
In this model state transitions are well defined and goal states are similarly defined.

_This is the preferred model for long running operations and should be used wherever possible._ Avoiding the complexity and mechanics of the LRO Wire Protocol makes things simpler for our users and tooling chain.

An example may be a machine reboot, where the operation itself completes synchronously but the GET operation on the virtual machine resource would have a "state: Rebooting", "state: Running" that could be queried at any time.

This model MAY integrate Push Notifications.

While most operations are likely to be POST semantics, In addition to POST semantics services MAY support PUT semantics via routing to simplify their APIs.
For example, a user that wants to create a database named "db1" could call:

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the databases segment is processing the PUT operation.

Services MAY also use the hybrid defined below.

### 13.2 Stepwise long running operations
A stepwise operation is one that takes a long, and often unpredictable, length of time to complete, and doesn't offer state transition modeled in the resource.
This section outlines the approach that services should use to expose such long running operations.

Service MAY expose stepwise operations.

> Stepwise Long Running Operations are sometimes called "Async" operations.
This causes confusion, as it mixes elements of platforms ("Async / await", "promises", "futures") with elements of API operation.
This document uses the term "Stepwise Long Running Operation" or often just "Stepwise Operation" to avoid confusion over the word "Async".

Services MUST perform as much synchronous validation as practical on stepwise requests.
Services MUST prioritize returning errors in a synchronous way, with the goal of having only "Valid" operations processed using the long running operation wire protocol.

For an API that's defined as a Stepwise Long Running Operation the service MUST go through the Stepwise Long Running Operation flow even if the operation can be completed immediately.
In other words, APIs must adopt and stick with a LRO pattern and not change patterns based on circumstance.

#### 13.2.1 PUT
Services MAY enable PUT requests for entity creation.

```http
PUT https://api.contoso.com/v1.0/databases/db1
```

In this scenario the _databases_ segment is processing the PUT operation.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

For services that need to return a 201 Created here, use the hybrid flow described below.

The 202 Accepted should return no body.
The 201 Created case should return the body of the target resource.

#### 13.2.2 POST
Services MAY enable POST requests for entity creation.

```http
POST https://api.contoso.com/v1.0/databases/

{
  "fileName": "someFile.db",
  "color": "red"
}
```

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

#### 13.2.3 POST, hybrid model
Services MAY respond synchronously to POST requests to collections that create a resource even if the resources aren't fully created when the response is generated.
In order to use this pattern, the response MUST include a representation of the incomplete resource and an indication that it is incomplete.

For example:

```http
POST https://api.contoso.com/v1.0/databases/ HTTP/1.1
Host: api.contoso.com
Content-Type: application/json
Accept: application/json

{
  "fileName": "someFile.db",
  "color": "red"
}
```

Service response says the database has been created, but indicates the request is not completed by including the Operation-Location header.
In this case the status property in the response payload also indicates the operation has not fully completed.

```http
HTTP/1.1 201 Created
Location: https://api.contoso.com/v1.0/databases/db1
Operation-Location: https://api.contoso.com/v1.0/operations/123

{
  "databaseName": "db1",
  "color": "red",
  "Status": "Provisioning",
  [ … other fields for "database" …]
}
```

#### 13.2.4 Operations resource
Services MAY provide a "/operations" resource at the tenant level.

Services that provide the "/operations" resource MUST provide GET semantics.
GET MUST enumerate the set of operations, following standard pagination, sorting, and filtering semantics.
The default sort order for this operation MUST be:

Primary Sort           | Secondary Sort
---------------------- | -----------------------
Not Started Operations | Operation Creation Time
Running Operations     | Operation Creation Time
Completed Operations   | Operation Creation Time

Note that "Completed Operations" is a goal state (see below), and may actually be any of several different states such as "successful", "cancelled", "failed" and so forth.

#### 13.2.5    Operation resource
An operation is a user addressable resource that tracks a stepwise long running operation.
Operations MUST support GET semantics.
The GET operation against an operation MUST return:

1. The operation resource, it's state, and any extended state relevant to the particular API.
2. 200 OK as the response code.

Services MAY support operation cancellation by exposing DELETE on the operation.
If supported DELETE operations MUST be idempotent.

> Note: From an API design perspective, cancellation does not explicitly mean rollback.
On a per-API defined case it may mean rollback, or compensation, or completion, or partial completion, etc.
Following a cancelled operation It SHOULD NOT be a client's responsibility to return the service to a consistent state which allows continued service.

Services that do not support operation cancellation MUST return a 405 Method Not Allowed in the event of a DELETE.

Operations MUST support the following states:

1. NotStarted
2. Running
3. Succeeded. Terminal State.
4. Failed. Terminal State.

Services MAY add additional states, such as "Cancelled" or "Partially Completed". Services that support cancellation MUST sufficiently describe their cancellation such that the state of the system can be accurately determined and any compensating actions may be run.

Services that support additional states should consider this list of canonical names and avoid creating new names if possible: Cancelling, Cancelled, Aborting, Aborted, Tombstone, Deleting, Deleted.

An operation MUST contain, and provide in the GET response, the following information:

1. The timestamp when the operation was created.
2. A timestamp for when the current state was entered.
3. The operation state (notstarted / running / completed).

Services MAY add additional, API specific, fields into the operation.
The operation status JSON returned looks like:

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-01-03.45Z",
  "status": "notstarted | running | succeeded | failed"
}
```

##### 13.2.5.1 Percent complete
Sometimes it is impossible for services to know with any accuracy when an operation will complete.
Which makes using the Retry-After header problematic.
In that case, services MAY include, in the operationStatus JSON, a percent complete field.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "percentComplete": "50",
  "status": "running"
}
```

In this example the server has indicated to the client that the long running operation is 50% complete.

##### 13.2.5.2 Target resource location
For operations that result in, or manipulate, a resource the service MUST include the target resource location in the status upon operation completion.

```json
{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.6 Operation tombstones
Services MAY choose to support tombstoned operations.
Services MAY choose to delete tombstones after a service defined period of time.

#### 13.2.7 The typical flow, polling
- Client invokes a stepwise operation by invoking an action using POST
- The server MUST indicate the request has been started by responding with a 202 Accepted status code. The response SHOULD include the location header containing a URL that the client should poll for the results after waiting the number of seconds specified in the Retry-After header.
- Client polls the location until receiving a 200 OK response from the server.

##### 13.2.7.1 Example of the typical flow, polling
Client invokes the restart action:

```http
POST https://api.contoso.com/v1.0/databases HTTP/1.1
Accept: application/json

{
  "fromFile": "myFile.db",
  "color": "red"
}
```

The server response indicates the request has been created.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

Client waits for a period of time then invokes another request to try to get the operation status.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds that results are still not ready and optionally provides a recommendation to wait 30 seconds.

```http
HTTP/1.1 200 OK
Retry-After: 30

{
  "createdDateTime": "2015-06-19T12-01-03.4Z",
  "status": "running"
}
```

Client waits the recommended 30 seconds and then invokes another request to get the results of the operation.

```http
GET https://api.contoso.com/v1.0/operations/123
Accept: application/json
```

Server responds with a "status:succeeded" operation that includes the resource location.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "createdDateTime": "2015-06-19T12-01-03.45Z",
  "lastActionDateTime": "2015-06-19T12-06-03.0024Z",
  "status": "succeeded",
  "resourceLocation": "https://api.contoso.com/v1.0/databases/db1"
}
```

#### 13.2.8 The typical flow, push notifications
1. Client invokes a long running operation by invoking an action using POST. The client has a push notification already setup on the parent resource.
2. The service indicates the request has been started by responding with a 202 Accepted status code. The client ignores everything else.
3. Upon completion of the overall operation the service pushes a notification via the subscription on the parent resource.
4. The client retrieves the operation result via the resource URL.

##### 13.2.8.1 Example of the typical flow, push notifications existing subscription
Client invokes the backup action.
The client already has a push notification subscription setup for db1.

```http
POST https://api.contoso.com/v1.0/databases/db1?backup HTTP/1.1
Accept: application/json
```

The server response indicates the request has been accepted.

```http
HTTP/1.1 202 Accepted
Operation-Location: https://api.contoso.com/v1.0/operations/123
```

The caller ignores all the headers in the return.

The target URL receives a push notification when the operation is complete.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "value": [
    {
      "subscriptionId": "1234-5678-1111-2222",
      "context": "subscription context that was specified at setup",
      "resourceUrl": "https://api.contoso.com/v1.0/databases/db1",
      "userId" : "contoso.com/user@contoso.com",
      "tenantId" : "contoso.com"
    }
  ]
}
```

#### 13.2.9 Retry-After
In the examples above the Retry-After header indicates the number of seconds that the client should wait before trying to get the result from the URL identified by the location header.

The HTTP specification allows the Retry-After header to alternatively specify a HTTP date, so clients should be prepared to handle this as well.

```http
HTTP/1.1 202 Accepted
Operation-Location: http://api.contoso.com/v1.0/operations/123
Retry-After: 60
```

Note: The use of the HTTP Date is inconsistent with the use of ISO 8601 Date Format used throughout this document, but is explicitly defined by the HTTP standard in [RFC 7231][rfc-7231-7-1-1-1]. Services SHOULD prefer the integer number of seconds (in decimal) format over the HTTP date format.

### 13.3 Retention policy for operation results
In some situations, the result of a long running operation is not a resource that can be addressed.
For example, if you invoke a long running Action that returns a Boolean (rather than a resource).
In these situations, the Location header points to a place where the Boolean result can be retrieved.

Which begs the question: "How long should operation results be retained?"

A recommended minimum retention time is 24 hours.

Operations SHOULD transition to "tombstone" for an additional period of time prior to being purged from the system.

## 14 Push notifications via webhooks
### 14.1 Scope
Services MAY implement push notifications via web hooks.
This section addresses the following key scenario:

> Push notification via HTTP Callbacks, often called Web Hooks, to publicly-addressable servers.

The approach set forth is chosen due to its simplicity, broad applicability, and low barrier to entry for service subscribers.
It's intended as a minimal set of requirements and as a starting point for additional functionality.

### 14.2 Principles
The core principles for services that support web hooks are:

1. Services MUST implement at least a poke/pull model. In the poke/pull model, a notification is sent to a client, and clients then send a request to get the current state or the record of change since their last notification. This approach avoids complexities around message ordering, missed messages, and change sets.  Services MAY add more data to provide rich notifications.
2. Services MUST implement the challenge/response protocol for configuring callback URLs.
3. Services SHOULD have a recommended age-out period, with flexibility for services to vary based on scenario.
4. Services SHOULD allow subscriptions that are raising successful notifications to live forever and SHOULD be tolerant of reasonable outage periods.
5. Firehose subscriptions MUST be delivered only over HTTPS. Services SHOULD require other subscription types to be HTTPS. See the "Security" section for more details.

### 14.3 Types of subscriptions
There are two subscription types, and services MAY implement either, both, or none.
The supported subscription types are:

1. Firehose subscriptions – a subscription is manually created for the subscribing application, typically in an app registration portal.  Notifications of activity that any users have consented to the app receiving are sent to this single subscription.
2. Per-resource subscriptions – the subscribing application uses code to programmatically create a subscription at runtime for some user-specific entity(s).

Services that support both subscription types SHOULD provide differentiated developer experiences for the two types:

1. Firehose – Services MUST NOT require developers to create code except to directly verify and respond to notifications.  Services MUST provide administrative UI for subscription management.  Services SHOULD NOT assume that end users are aware of the subscription, only the subscribing application's functionality.
2. Per-user – Services MUST provide an API for developers to create and manage subscriptions as part of their app as well as verifying and responding to notifications.  Services MAY expect end users to be aware of subscriptions and MUST allow end users to revoke subscriptions where they were created directly in response to user actions.

### 14.4 Call sequences
The call sequence for a firehose subscription MUST follow the diagram below.
It shows manual registration of application and subscription, and then the end user making use of one of the service's APIs.
At this part of the flow, two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope.)
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.

The final part of the sequence is the notification flow itself.

Non-normative implementation guidance:  A resource in the service changes and the service needs to run the following logic:

1. Determine the set of users who have access to the resource, and could thus expect apps to receive notifications about it on their behalf.
2. See which of those users have consented to receiving notifications and from which apps.
3. See which apps have registered a firehose subscription.
4. Join 1, 2, 3 to produce the concrete set of notifications that must be sent to apps.

It should be noted that the act of user consent and the act of setting up a firehose subscription could arrive in either order.
Services SHOULD send notifications with setup processed in either order.

![Firehose subscription setup][websequencediagram-firehose-subscription-setup]

For a per-user subscription, app registration is either manual or automated.
The call flow for a per-user subscription MUST follow the diagram below.
It shows the end user making use of one of the service's APIs, and again, the same two things MUST be stored:

1. The service MUST store the end user's act of consent to receiving notifications from this specific application (typically a background usage OAUTH scope).   
2. The subscribing application MUST store the end user's tokens in order to call back for details once notified of changes.  

In this case, the subscription is set up programmatically using the end-user's token from the subscribing application.
The app MUST store the ID of the registered subscription alongside the user tokens.

Non normative implementation guidance: In the final part of the sequence, when an item of data in the service changes and the service needs to run the following logic:

1. Find the set of subscriptions that correspond via resource to the data that changed.
2. For subscriptions created under an app+user token, send a notification to the app per subscription with the subscription ID and user id of the subscription-creator.
- For subscriptions created with an app only token, check that the owner of the changed data or any user that has visibility of the changed data has consented to notifications to the application, and if so send a set of notifications per user id to the app per subscription with the subscription ID.

  ![User subscription setup][websequencediagram-user-subscription-setup]

### 14.5 Verifying subscriptions
When subscriptions change either programmatically or in response to change via administrative UI portals, the subscribing service needs to be protected from malicious or unexpected calls from services pushing potentially large volumes of notification traffic.

For all subscriptions, whether firehose or per-user, services MUST send a verification request as part of creation or modification via portal UI or API request, before sending any other notifications.

Verification requests MUST be of the following format as an HTTP/HTTPS POST to the subscription's _notificationUrl_.

```http
POST https://{notificationUrl}?validationToken={randomString}
ClientState: clientOriginatedOpaqueToken (if provided by client on subscription-creation)
Content-Length: 0
```

For the subscription to be set up, the application MUST respond with 200 OK to this request, with the _validationToken_ value as the sole entity body.
Note that if the _notificationUrl_ contains query parameters, the _validationToken_ parameter must be appended with an `&`.

If any challenge request does not receive the prescribed response within 5 seconds of sending the request, the service MUST return an error, MUST NOT create the subscription, and MUST NOT send further requests or notifications to _notificationUrl_.

Services MAY perform additional validations on URL ownership.

### 14.6 Receiving notifications
Services SHOULD send notifications in response to service data changes that do not include details of the changes themselves, but include enough information for the subscribing application to respond appropriately to the following process:

1. Applications MUST identify the correct cached OAuth token to use for a callback
2. Applications MAY look up any previous delta token for the relevant scope of change
3. Applications MUST determine the URL to call to perform the relevant query for the new state of the service, which MAY be a delta query.

Services that are providing notifications that will be relayed to end users MAY choose to add more detail to notification packets in order to reduce incoming call load on their service.
 Such services MUST be clear that notifications are not guaranteed to be delivered and may be lossy or out of order.

Notifications MAY be aggregated and sent in batches.
Applications MUST be prepared to receive multiple events inside a single push notification.

The service MUST send all Web Hook data notifications as POST requests.

Services MUST allow for a 30-second timeout for notifications.
If a timeout occurs or the application responds with a 5xx response, then the service SHOULD retry the notification with exponential back-off.
All other responses will be ignored.

The service MUST NOT follow 301/302 redirect requests.

#### 14.6.1 Notification payload
The basic format for notification payloads is a list of events, each containing the id of the subscription whose referenced resources have changed, the type of change, the resource that should be consumed to identify the exact details of the change and sufficient identity information to look up the token required to call that resource.

For a firehose subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab18b",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files?$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

For a per-user subscription, a concrete example of this may look like:

```json
{
  "value": [
    {
      "subscriptionId": "32b8cbd6174ab183",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    },
    {
      "subscriptionId": "97b391179fa22",
      "clientState ": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z",
      "resource": "https://api.contoso.com/v1.0/users/user@contoso.com/files/$delta",
      "userId" : "<User GUID>",
      "tenantId" : "<Tenant Id>"
    }
  ]
}
```

Following is a detailed description of the JSON payload.

A notification item consists a top-level object that contains an array of events, each of which identified the subscription due to which this notification is being sent.

Field | Description
----- | --------------------------------------------------------------------------------------------------
value | Array of events that have been raised within the subscription’s scope since the last notification.

Each item of the events array contains the following properties:

Field              | Description
------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
subscriptionId     | The id of the subscription due to which this notification has been sent.<br/>Services MUST provide the *subscriptionId* field.
clientState        | Services MUST provide the *clientState* field if it was provided at subscription creation time.
expirationDateTime | Services MUST provide the *expirationDateTime* field if the subscription has one.
resource           | Services MUST provide the resource field. This URL MUST be considered opaque by the subscribing application.  In the case of a richer notification it MAY be subsumed by message content that implicitly contains the resource URL to avoid duplication.<br/>If a service is providing this data as part of a more detailed data packet, then it need not be duplicated.
userId             | Services MUST provide this field for user-scoped resources.  In the case of user-scoped resources, the unique identifier for the user should be used.<br/>In the case of resources shared between a specific set of users, multiple notifications must be sent, passing the unique identifier of each user.<br/>For tenant-scoped resources, the user id of the subscription should be used.
tenantId           | Services that wish to support cross-tenant requests SHOULD provide this field. Services that provide notifications on tenant-scoped data MUST send this field.

### 14.7 Managing subscriptions programmatically
For per-user subscriptions, an API MUST be provided to create and manage subscriptions.
The API must support at least the operations described here.

#### 14.7.1 Creating subscriptions
A client creates a subscription by issuing a POST request against the subscriptions resource.
The subscription namespace is client-defined via the POST operation.

```
https://api.contoso.com/apiVersion/$subscriptions
```

The POST request contains a single subscription object to be created.
That subscription object has the following properties:

Property Name   | Required | Notes
--------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------
resource        | Yes      | Resource path to watch.
notificationUrl | Yes      | The target web hook URL.
clientState     | No       | Opaque string passed back to the client on all notifications. Callers may choose to use this to provide tagging mechanisms.

If the subscription was successfully created, the service MUST respond with the status code 201 CREATED and a body containing at least the following properties:

Property Name      | Required | Notes
------------------ | -------- | -------------------------------------------------------------------------------------------
id                 | Yes      | Unique ID of the new subscription that can be used later to update/delete the subscription.
expirationDateTime | No       | Uses existing Microsoft REST API Guidelines defined time formats.

Creation of subscriptions SHOULD be idempotent.
The combination of properties scoped to the auth token, provides a uniqueness constraint.

Below is an example request using a User + Application principal to subscribe to notifications from a file:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "resource": "http://api.service.com/v1.0/files/file1.txt",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "32b8cbd6174ab18b",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

Below is an example using an Application-Only principal where the application is watching all files to which it's authorized:

```http
POST https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {ApplicationPrincipalBearerToken}

{
  "resource": "All.Files",
  "notificationUrl": "https://contoso.com/myCallbacks",
  "clientState": "clientOriginatedOpaqueToken"
}
```

The service SHOULD respond to such a message with a response format minimally like this:

```json
{
  "id": "8cbd6174abb391179",
  "expirationDateTime": "2016-02-04T11:23Z"
}
```

#### 14.7.2 Updating subscriptions
Services MAY support amending subscriptions.
 To update the properties of an existing subscription, clients use PATCH requests providing the ID and the properties that need to change.
Omitted properties will retain their values.
To delete a property, assign a value of JSON null to it.

As with creation, subscriptions are individually managed.

The following request changes the notification URL of an existing subscription:

```http
PATCH https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}

{
  "notificationUrl": "https://contoso.com/myNewCallback"
}
```

If the PATCH request contains a new _notificationUrl_, the server MUST perform validation on it as described above.
If the new URL fails to validate, the service MUST fail the PATCH request and leave the subscription in its previous state.

The service MUST return an empty body and `204 No Content` to indicate a successful patch.

The service MUST return an error body and status code if the patch failed.

The operation MUST succeed or fail atomically.

#### 14.7.3 Deleting subscriptions
Services MUST support deleting subscriptions.
Existing subscriptions can be deleted by making a DELETE request against the subscription resource:

```http
DELETE https://api.contoso.com/files/v1.0/$subscriptions/{id} HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

As with update, the service MUST return `204 No Content` for a successful delete, or an error body and status code to indicate failure.

#### 14.7.4 Enumerating subscriptions
To get a list of active subscriptions, clients issue a GET request against the subscriptions resource using a User + Application or Application-Only bearer token:

```http
GET https://api.contoso.com/files/v1.0/$subscriptions HTTP 1.1
Authorization: Bearer {UserPrincipalBearerToken}
```

The service MUST return a format as below using a User + Application principal bearer token:

```json
{
  "value": [
    {
      "id": "32b8cbd6174ab18b",
      "resource": " http://api.contoso.com/v1.0/files/file1.txt",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

An example that may be returned using Application-Only principal bearer token:

```json
{
  "value": [
    {
      "id": "6174ab18bfa22",
      "resource": "All.Files ",
      "notificationUrl": "https://contoso.com/myCallbacks",
      "clientState": "clientOriginatedOpaqueToken",
      "expirationDateTime": "2016-02-04T11:23Z"
    }
  ]
}
```

### 14.8 Security
All service URLs must be HTTPS (that is, all inbound calls MUST be HTTPS). Services that deal with Web Hooks MUST accept HTTPS.

We recommend that services that allow client defined Web Hook Callback URLs SHOULD NOT transmit data over HTTP.
This is because information can be inadvertently exposed via client, network, server logs and other mechanisms.

However, there are scenarios where the above recommendations cannot be followed due to client endpoint or software limitations.
Consequently, services MAY allow web hook URLs that are HTTP.

Furthermore, services that allow client defined HTTP web hooks callback URLs SHOULD be compliant with privacy policy specified by engineering leadership.
This will typically include recommending that clients prefer SSL connections and adhere to special precautions to ensure that logs and other service data collection are properly handled.

For example, services may not want to require developers to generate certificates to onboard.
Services might only enable this on test accounts.

## 15 Unsupported requests
RESTful API clients MAY request functionality that is currently unsupported.
RESTful APIs MUST respond to valid but unsupported requests consistent with this section.

### 15.1 Essential guidance
RESTful APIs will often choose to limit functionality that can be performed by clients.
For instance, auditing systems allow records to be created but not modified or deleted.
Similarly, some APIs will expose collections but require or otherwise limit filtering and ordering criteria, or MAY not support client-driven pagination.

### 15.2 Feature allow list
If a service does not support any of the below API features, then an error response MUST be provided if the feature is requested by a caller.
The features are:
- Key Addressing in a collection, such as: `https://api.contoso.com/v1.0/people/user1@contoso.com`
- Filtering a collection by a property value, such as: `https://api.contoso.com/v1.0/people?$filter=name eq 'david'`
- Filtering a collection by range, such as: `http://api.contoso.com/v1.0/people?$filter=hireDate ge 2014-01-01 and hireDate le 2014-12-31`
- Client-driven pagination via $top and $skip, such as: `http://api.contoso.com/v1.0/people?$top=5&$skip=2`
- Sorting by $orderBy, such as: `https://api.contoso.com/v1.0/people?$orderBy=name desc`
- Providing $delta tokens, such as: `https://api.contoso.com/v1.0/people?$delta`

#### 15.2.1 Error response
Services MUST provide an error response if a caller requests an unsupported feature found in the feature allow list.
The error response MUST be an HTTP status code from the 4xx series, indicating that the request cannot be fulfilled.
Unless a more specific error status is appropriate for the given request, services SHOULD return "400 Bad Request" and an error payload conforming to the error response guidance provided in the Microsoft REST API Guidelines.
Services SHOULD include enough detail in the response message for a developer to determine exactly what portion of the request is not supported.

Example:

```http
GET https://api.contoso.com/v1.0/people?$orderBy=name HTTP/1.1
Accept: application/json
```

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": {
    "code": "ErrorUnsupportedOrderBy",
    "message": "Ordering by name is not supported."
  }
}
```

## 16 附录
### 16.1 Sequence diagram notes
All sequence diagrams in this document are generated using the WebSequenceDiagrams.com web site.
To generate them, paste the text below into the web tool.

#### 16.1.1 Push notifications, per user flow

```
=== Being Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual App Registration


Developer <--> DB Portal : Login into Portal, App Registration UX
DB Portal -> +DB App Registration: App Name etc.
note over DB App Registration: Confirm Portal Access Token

DB App Registration -> -DB Portal: App ID
DB Portal <--> App Server: Developer copies App ID

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Manual Notification Registration

Developer <--> DB Portal: webhook registration UX
DB Portal -> +DB Notifications: Register: App Server webhook URL, Scope, App ID
Note over DB Notifications : Confirm Portal Access Token
DB Notifications -> -DB Portal: notification ID
DB Portal --> App Server : Developer may copy notification ID


note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token
=== End Text ===
```

#### 16.1.2 Push notifications, firehose flow

```
=== Being Text ===
note over Developer, Automation, App Server:
     An App Developer like MovieMaker
     Wants to integrate with primary service like Dropbox
end note
note over DB Portal, DB App Registration, DB Notifications, DB Auth, DB Service: The primary service like Dropbox
note over Client: The end users' browser or installed app

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : App Registration

alt Automated app registration
   Developer <--> Automation: Configure
   Automation -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm App Access Token
   DB App Registration -> -Automation: App ID, App Secret
   Automation --> App Server : Embed App ID, App Secret
else Manual app registration
   Developer <--> DB Portal : Login into Portal, App Registration UX
   DB Portal -> +DB App Registration: App Name etc.
   note over DB App Registration: Confirm Portal Access Token

   DB App Registration -> -DB Portal: App ID
   DB Portal <--> App Server: Developer copies App ID
end

note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Client Authorization

Client -> +App Server : Request access to DB protected information
App Server -> -Client : Redirect to DB Authorization endpoint with authorization request
Client -> +DB Auth : Redirected authorization request
Client <--> DB Auth : Authorization UX
DB Auth -> -Client : Redirect back to App Server with code
Client -> +App Server : Redirect request back to access server with access code
App Server -> +DB Auth : Request tokens with access code
note right of DB Service: Cache that this User ID provided access to App ID
DB Auth -> -App Server : Response with access, refresh, and ID tokens
note right of App Server : Cache tokens by user ID
App Server -> -Client : Return information to client



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Registration

App Server->+DB Notifications: Register: App server webhook URL, Scope, App ID
note over DB Notifications : Confirm User Access Token
DB Notifications -> -App Server: notification ID
note right of App Server : Cache the Notification ID and User Access Token



note over Developer, Automation, App Server, DB Portal, DB App Registration, DB Notifications, Client : Notification Flow

Client <--> DB Service: Changes to user data - typical via interacting with App Server via Client
DB Service -> App Server : Notification with notification ID and user ID
App Server -> +DB Service : Request changed information with cached access tokens and "since" token
note over DB Service: Confirm User Access Token
DB Service -> -App Server : Response with data and new "since" token
note right of App Server: Update status and cache new "since" token



=== End Text ===
```
[fielding]: https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm
[IANA-headers]: http://www.iana.org/assignments/message-headers/message-headers.xhtml
[rfc7231-7-1-1-1]: https://tools.ietf.org/html/rfc7231#section-7.1.1.1
[rfc-7230-3-1-1]: https://tools.ietf.org/html/rfc7230#section-3.1.1
[rfc-7231]: https://tools.ietf.org/html/rfc7231
[rest-in-practice]: http://www.amazon.com/REST-Practice-Hypermedia-Systems-Architecture/dp/0596805829/
[rest-on-wikipedia]: http://en.wikipedia.org/wiki/Representational_state_transfer
[rfc-5789]: http://tools.ietf.org/html/rfc5789
[rfc-5988]: http://tools.ietf.org/html/rfc5988
[rfc-3339]: https://tools.ietf.org/html/rfc3339
[rfc-5322-3-3]: https://tools.ietf.org/html/rfc5322#section-3.3
[cors-preflight]: http://www.w3.org/TR/cors/#resource-preflight-requests
[rfc-3864]: http://www.ietf.org/rfc/rfc3864.txt
[odata-json-annotations]: http://docs.oasis-open.org/odata/odata-json-format/v4.0/os/odata-json-format-v4.0-os.html#_Instance_Annotations
[cors]: http://www.w3.org/TR/access-control/
[cors-user-credentials]: http://www.w3.org/TR/access-control/#user-credentials
[cors-simple-headers]: http://www.w3.org/TR/access-control/#simple-header
[rfc-4627]: https://tools.ietf.org/html/rfc4627
[iso-8601]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.15
[clr-time]: https://msdn.microsoft.com/en-us/library/System.DateTime(v=vs.110).aspx
[ecmascript-time]: http://www.ecma-international.org/ecma-262/5.1/#sec-15.9.1.1
[ole-date]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms221199(v=vs.85).aspx
[ticks-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[unix-time]: https://msdn.microsoft.com/en-us/library/1f4c8f33.aspx
[windows-time]: https://msdn.microsoft.com/en-us/library/windows/desktop/ms724290(v=vs.85).aspx
[excel-time]: http://support.microsoft.com/kb/214326?wa=wsignin1.0
[wikipedia-iso8601-durations]: http://en.wikipedia.org/wiki/ISO_8601#Durations
[wikipedia-iso8601-intervals]: http://en.wikipedia.org/wiki/ISO_8601#Time_intervals
[wikipedia-iso8601-repeatingintervals]: http://en.wikipedia.org/wiki/ISO_8601#Repeating_intervals
[principle-of-least-astonishment]: http://en.wikipedia.org/wiki/Principle_of_least_astonishment
[odata-breaking-changes]: http://docs.oasis-open.org/odata/odata/v4.0/errata02/os/complete/part1-protocol/odata-v4.0-errata02-os-part1-protocol-complete.html#_Toc406398209
[websequencediagram-firehose-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDogTWFudWFsAIFzEQoKCgCDAgo8LS0-AIIqCiA6IExvZ2luIGludG8Agj8JAII1ECBVWCAKACoKLT4gKwCCWBM6AIQGBU5hbWUgZXRjLgCDFQ4AGxJDb25maXJtAIEBCEFjY2VzcyBUb2tlbgoKAIM3EyAtPiAtAINkCQBnBklEAIEMCwCBVQUAhQIMAIR3CmNvcGllcwArCACCIHAAhHMMAIMKDwCDABg6IHdlYmhvb2sgcgCCeg4AgnUSAIVQDToAhXYHZXIAgwgGAIcTBgBECVVSTCwgU2NvcGUAhzIGSUQKTgCGPQwAhhwNIACDBh4AHhEAgxEPbgCBagwAgxwNAIMaDiAAgx0MbWF5IGNvcHkALREAhVtqAIZHB0F1dGhvcml6AIY7BwCGXQctPiArAIEuDVJlcXVlc3QgYQCFOQZ0byBEQiBwcm90ZWN0ZWQgaW5mb3IAiiQGCgCDBQstPiAtAIctCVJlZGlyZWN0ADYHAGwNIGVuZHBvaW50AIoWBmEADw1yAHYGAIEQDACJVAcASwtlZAAYHgCICAgAMAcAcA4AhGoGAE0FAIEdFmJhY2sgdG8AhF8NaXRoIGNvZGUAghoaaQCBagcAgToHAD0JAII-B3MAPgsAglEHAEsFAIIzDgCBXw0Agn8GdG9rZW5zACcSAI0_BXJpZ2h0IG9mAItpDUNhY2hlIHRoYXQgdGhpcyBVc2VyIElEIHByb3ZpZGVkAINNCwCIZgoAggcJAIN7D3Nwb25zAI0_BwCECgYsIHJlZnJlc2gsIGFuZCBJRACBHAcAgQMPAIYADQCBDAcAgUUGYnkAjFkFIElEAIQkG3R1cm4AhF4MIHRvIGMAjR8FAIwRagCJVw1GbG93AIYqCQCMaQgAgmoKaGFuZ2UAj3YFAIFXBWRhdGEgLSB0eXBpY2FsIHZpYQCQDgVyYWN0aW5nAJAPBgCJQQt2aWEAjnsHCgCPNgogAIhDEACKZw0AkFMFAIkBDwCDDAUAgkYWKwBNCwCHWApjAIEyBQCHRg0AhWUHYWNoAIQeDACEfwVhbmQgInNpbmNlIgCFEQYAkSQOAIR3CgCNfwcAhHQFAIpQEACBUgsAhFAcAII8BWFuZCBuZXcAYRQAhFUTOiBVcGRhdGUgc3RhdHUAgSkGAIFDBQAxEwoKCg&s=mscgen
[websequencediagram-user-subscription-setup]: http://www.websequencediagrams.com/cgi-bin/cdraw?lz=bm90ZSBvdmVyIERldmVsb3BlciwgQXV0b21hdGlvbiwgQXBwIFNlcnZlcjogCiAgICAgQW4AEAUAJwkgbGlrZSBNb3ZpZU1ha2VyACAGV2FudHMgdG8gaW50ZWdyYXRlIHdpdGggcHJpbWFyeSBzZXJ2aWNlADcGRHJvcGJveAplbmQgbm90ZQoAgQwLQiBQb3J0YWwsIERCAIEJBVJlZ2lzdHIAgRkHREIgTm90aWZpYwCBLAVzACEGdXRoACsFUwBgBjogVGhlAF0eAIF_CkNsaWVudAAtBmVuZCB1c2VycycgYnJvd3NlciBvciBpbnN0YWxsZWQgYXBwCgCBIQwAgiQgAIFABQCBIS8AgQoGIDoAgWwRCgphbHQAgyUIAIEHBiByABQMICAAgxsLPC0tPgCDTws6IENvbmZpZ3VyZQogIACDaAsgLT4gKwCCWBMAegZOYW1lIGV0Yy4AhAgFAIMaDQAfEgBdBXJtAIQ_BUFjY2VzcyBUb2tlAIETBgCDOxIgLT4gLQCBFgxBcHAgSUQAhHwIY3JldACBGxAtPgCFFgsgOiBFbWJlZAAkFGVsc2UgTWFudWFsAIIEJACEbQkgOiBMb2dpbiBpbnRvAIUBCQCBKRFVWACGGAUALQoAgh8mAIIZKwCBCAcAgjoNAIIsHACGLwkAgj8IAIESDgCECAYAh1ELAIdFCmNvcGllcwAuCGVuZACEeGoAhWQHQXV0aG9yaXoAhV8HAIV6By0-ICsAg2ANUmVxdWVzdCBhAIRVBnRvIERCIHByb3RlY3RlZCBpbmZvcgCJQQYKAIQaCy0-IC0AhkoJUmVkaXJlY3QANgcAbA0gZW5kcG9pbnQAiTMGYQAPDXIAdgYAgRAMAIhxBwBLC2VkABgeAIRjCAAwB0EAcQxVWAoASQgAgRwWYmFjayB0bwCFdAwAilwFY29kZQCCGRppAIFpBwCBOQcAPQkAgj0HcwA-CwCCUAcASwUAgjIOAIFeDQCCfgZ0b2tlbnMAJxIAjFsFcmlnaHQgb2YAiwUNQ2FjaGUgdGhhdCB0aGlzIFVzZXIgSUQgcHJvdmlkZWQAg0wLAIU6BwCCBAwAg3oPc3BvbnMAjFsHAIQJBiwgcmVmcmVzaCwgYW5kIElEAIEcBwCBAw8AiDENAIEMBwCBRQZieQCLdQUgSUQAhCMbdHVybgCEXQwgdG8gYwCMOwUKCgCLL2oAjXUMAIwTDwCPNQotPisAjhwQOgCORQdlcgCMVwYAg3YIZWJob29rIFVSTCwgU2NvcGUAkAEGSUQAjwoOAI5rDSAAi2UKAINFBQCLYw0AHBEAgzUOOiBuAIE2DABgCACDCB1oZQCBaQ5JRACDYwUAahIAghB4RmxvdwCJMwkAjE0IAIV0CmhhbmdlAJIcBQCEYQVkYXRhIC0gdHlwaWNhbCB2aWEAkjQFcmFjdGluZwCSNQYAjV8LdmlhAJEhBwoAkVwKIACNfhAAhAsNAJJ5BQCCWQ8AhhYFAIVQFisATQsAimEKYwCBMgUAik8NAIhvB2FjaACHKAwAiAkFYW5kICJzaW5jZSIAiBsGAJNKDgCIAQoAhB0cAIFSCwCHWhwAgjwFYW5kIG5ldwBhFACHXxM6IFVwZGF0ZSBzdGF0dQCBKQYAgUMFADETCgoK&s=mscgen

# Entity 引擎

## 简介

OFBiz 的 Entity 引擎是一系列工具和模式的集合，用于对 Entity 数据进行建模和管理。在这里，Entity
指的是由一些数据字段与其他 Entity 之间的关系组成的一组数据。这个定义来源于关系型数据库的 ER 模型的
概念。Entity 引擎 的作用是简化企业级 Entity 数据的使用。其中包括对数据的定义、维护、QA、和与 Entity
相关的功能的开发。

Entity 引擎使用了很多业务层和集成层的设计模式。其中主要包括：`Business Delegate`，`Value Object`，
`Composite Entity (variation)`，`Value Object Assembler`，`Service Locator`，和 `Data Access Object`。

对这些模式的详细介绍可以参考[《Core J2EE Patterns》](http://www.corej2eepatterns.com/BusinessDelegate.htm)一书。

还有一些模式来自于[《EJB Design Patterns》](http://www.theserverside.com/news/1369776/Free-Book-EJB-Design-Patterns)一书，（作者：Floyd Marinescu ）。这些模式包括：`Data Transfer HashMap`，和 `Generic Attribute Access`。为了生成唯一的主键，Entity 引擎使用了一种被称作为 `Ethernet Key Generation` 的设计模式，
因为它使用了一种冲突检测机制来确保多个服务器可以使用同一个数据库来获取唯一主键（与具体数据库无关）。

Entity 引擎的主要目标是尽可能地减少事务性应用中与 Entity 相关的数据持久层的代码。为了实现这个目标，
所有的 `value objects` 都是用泛型来实现的，使用 map 来存储数据字段的名称和值。`get` 和 `set` 方法
接收一个 `String` 类型的代表字段名的参数，并用这个参数来验证在这个 Entity 中确实存在这样一个字段，然后
才会进行相应地 get 或者 set 操作。这种灵活度所带来的风险可以通过在Entity 引擎与应用之间定义一层 contract 而被减弱。
这些 contract 被包含在一个特殊的 xml 文件中。

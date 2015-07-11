# 新手入门

本章针对的读者是 OFBiz 的新手，介绍了 OFBiz 应用开发流程的一些基本知识。希望新手可以通过这个教程
快速的了解开发 OFBiz 应用的一些最佳实践和代码规范，以及修改 OFBiz 代码的一些流程。这是一个关于
OFBiz 的 “HelloWorld”。 :)

## 概述

本入门教程分为6个部分：

1. 如何创建和加载定制的模块，并且添加第一个页面，在页面上显示 “这是一个练习程序”。
1. 创建一些更复杂的 GUI 模型，与已有的数据表交互，并且学习如何让 web 应用更加安全。
1. 学习如何与数据库实体（entities）进行交互，创建执行 CRUD 操作的服务（services）， 创建用于
表单验证的事件（events）。
1. 学习如何根据自定义的条件来调用自动触发的服务，通过服务组来调用多个服务，以及利用服务接口的概念来
在服务之间共享通用的参数。
1. 创建新的实体，扩展已有的实体，以及为你的应用准备 XML 数据。
1. 最后，将介绍利用 Axis 去实现客户端和服务端应用的通讯。

完成以上这些步骤之后，你将升级为 OFBiz 的开发者。

### 一些建议：

- 在写代码之前，请先阅读一下文档：
  - OFBiz 代码贡献者最佳实践
  - 代码规范
  - 最佳实践
- 不要只是简单的复制粘贴文件
- 可以在[这个页面](https://cwiki.apache.org/confluence/display/OFBADMIN/OFBiz+Documentation+Index)搜索想要的文档
- 养成查看 console 日志的好习惯，它可以让你更容易的解决遇到的问题，同时也可以了解系统内部运行的步骤

## 第一部分

### 创建组件的定义文件

1. 在`hot-deploy/`目录下新建一个子目录，命名为`practice`，即 `(hot-deploy/practice)`。目录
的名称应该跟将要创建的新组件的名称一致。
**注意：** 所有关于这个组件的开发都应该只在这个子目录下进行。
1. 在 `hot-deploy/practice` 目录下新建一个 `ofbiz-component.xml` 文件，并将下面的内容复制到
这个文件中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ofbiz-component name="practice"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/ofbiz-component.xsd">
      <resource-loader   name="main" type="component"/>
    <webapp name="practice"
       title="Practice"
       server="default-server"
       base-permission="OFBTOOLS"
       location="webapp/practice"
       mount-point="/practice"
       app-bar-display="false"/>
</ofbiz-component>
```

#### `ofbiz-component.xml` 文件的说明

`ofbiz-component.xml` 文件的作用是让 OFBiz 知道该组件资源的位置，同时将这个位置添加到 `classpath` 中。

`<resource-loader>` 标签中的 `name` 属性可以是任意的值，在本示例中我们用 `main`。`type`属性
告诉 OFBiz 我们要加载的是一个 `component`。

```xml
<resource-loader name="main" type="component"/>
```

`<webapp>`标签有7个属性：

```xml
<webapp name="practice"
       title="Practice"
       server="default-server"
       base-permission="OFBTOOLS"
       location="webapp/practice"
       mount-point="/practice"
       app-bar-display="false"/>
```

下面的表格介绍了这些属性的意义：

|属性|说明|是否必须|
|---|---|---|
|`name`|web应用的名字|是|
|`title`|应用的标题，将会显示在导航的顶部|是|
|`server`|使用的服务器|是|
|`base-permission`|访问该组件所需的权限。在本例中，用户需要具有`OFBTOOLS`权限。因为`admin`用户具有这个权限，所以我们测试的时候并不需要创建一个新的用户|是|
|`location`|服务器默认的根目录|是|
|`mount-point`|用于访问该资源的 URL，在本例中，应该是 `localhost:8080/practice` |是|
|`app-bar-display`|如果设置为`true`，那么这个组件将会在主应用的 tabs 中显示，属于`common OFBiz decorator` 的一部分。|是|

### 创建web应用的配置文件

我们创建的应用将遵循 J2EE webapp 的标准。

1. 在`practice`组件中创建一个`webapp`目录（`hot-deploy/practice/webapp`）。这个目录将包含我们的组件中所有与
webapp 相关的文件。
1. 因为我们要创建的应用的名称是`practice`，所以创建一个 `hot-deploy/practice/webapp/practice` 目录。
一个组件可以包含多个web应用。比如 `marketing` 组件就包含了 `marketing` 和 `sfa` 两个web应用。
1. 创建一个 `hot-deploy/practice/webapp/practice/WEB-INF` 目录。
OFBiz 的 web 应用需要两个配置文件，`controller.xml` 和 `web.xml`。其中，`controller.xml`
用于告诉 OFBiz 如何去处理来自网站访问者的请求：调用哪个 action，渲染哪个页面等。`web.xml` 文件
用于告诉 OFBiz 哪些资源（数据库和业务逻辑）可以被这个web应用访问，以及如何去处理各种web有关的问题，
比如欢迎页面，重定向，和出错页面等。
1. 创建 `web.xml` 文件（web.xml文件遵循 J2EE web 应用规范），文件的内容可以参考其他组件中的内容。
需要改动的重要的值有：`<display-name>`，`localDispatcherName`，`mainDecoratorLocation`和
`webSiteId`。

```xml
<context-param>
    <param-name>webSiteId</param-name>
    <param-value>PRACTICE</param-value>
    <description>A unique ID used to look up the WebSite entity to get information about catalogs, etc.</description>
</context-param>
<context-param>
     <param-name>localDispatcherName</param-name>
     <param-value>practice</param-value>
     <description>A unique name used to identify/recognize the local dispatcher for the Service Engine</description>
</context-param>
<context-param>
     <param-name>mainDecoratorLocation</param-name>
     <param-value>component://practice/widget/PracticeScreens.xml</param-value>
     <!-- change the path to the following if the above doesn't work for you -->
     <!-- <param-value>component://practice/webapp/practice/widget/PracticeScreens.xml</param-value> -->
     <description>The location of the main-decorator screen to use for this webapp; referred to as a context variable in screen def XML files.</description>
</context-param>
```

  - 目前我们先设置 `websiteId` 的值为 `PRACTICE`。
  - 目前我们先设置 `mainDecoratorLocation` 的值为 "component://practice/widget/CommonScreens.xml"，这个配置项用于指定 screen 中主装饰器
  的位置：

```xml
${parameters.mainDecoratorLocation}
```
  这样做的好处是提高了代码的独立性，让我们在需要更改主装饰器的位置时，不用到处去更新 screen 中的代码。
  另一个好处是，它提高了 screen 代码的重用性，我们可以重用别的组件中的 screen，但是用自己的主装饰器
  来装饰它。
  - 创建 `controller.xml` 文件（用于 ofbiz webapp 控制器），这个文件的内容刚开始的时候会比较简单，
  但是会随着我们添加新的功能而不断增长。目前，先将下面的内容放进去：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<site-conf xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/site-conf.xsd">
       <include location="component://common/webcommon/WEB-INF/common-controller.xml"/>
       <description>Practice Component Site Configuration File</description>
       <owner>Copyright 2001-2009 The Apache Software Foundation</owner>
       <handler name="screen" type="view" class="org.ofbiz.widget.screen.ScreenWidgetViewHandler"/>
       <!-- Request Mappings -->
       <request-map uri="main">
           <security https="false" auth="false"/>
           <response name="success" type="view" value="main"/>
       </request-map>
       <!-- end of request mappings -->
       <!-- View Mappings -->
       <view-map name="main" type="screen" page="component://practice/widget/PracticeScreens.xml#main"/>
       <!-- change the path to the following if the above doesn't work for you -->
       <!-- <view-map name="main" type="screen" page="component://practice/webapp/practice/widget/PracticeScreens.xml#main"/> -->

       <!-- end of view mappings -->
</site-conf>
```

### 创建用户接口

1. 创建一个新的目录 `hot-deploy/practice/webapp/practice/error`。
  - 在 `error` 目录下创建一个 `error.jsp` 文件。文件中的内容可以参考其他任意模块中的 error.jsp
  文件。error 页面的位置需要在 controller.xml 文件的开头设置，例如 `<errorpage>/error/error.jsp</errorpage>`。
1. 创建一个子目录 `hot-deploy/practice/widget`。这个目录用于存放表单，菜单和 screen 等UI文件。
1. 在 `widget` 目录中创建一个 `PracticeScreens.xml` 文件。目前，先放入下面的内容：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<screens xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/widget-screen.xsd">
    <screen name="main">
        <section>
            <widgets>
                <label text="This is first practice"/>
            </widgets>
        </section>
    </screen>
</screens>
```

### 应用预览

到目前为止，我们已经创建了一个应用的基本的元素。让我们来预览一下应用被访问的整个流程。首先，浏览器端会发送一个请求，
比如：“localhost:8080/practice/control/main”

  - 当 OFBiz 收到这个请求，它会首先查看`/practice`，这是因为在 `ofbiz-component.xml` 文件中
  我们定义了 `mount point` 为 `/practice`。现在 OFBiz 知道 practice 组件将会接手剩下的处理。
  - OFBiz 接下来会查看 `controller.xml` 文件。在这个文件中，我们定义了 `request-maps` 和
  `view-maps`。 如果 OFBiz 找到名称为 `main` 的 request-map， 它会使用与其对应的 view-map。
  request-map 可以被配置为指向一个 view，或者也可以指向一个 event 或者 service。如果指向的是
  view，那么程序会继续向下查找 `controller.xml` 文件，去查找对应的 view-map。
  - 为了让教程简单，让我们先配置所有的 view 都指向一个 `type=screen`。这样，`page` 标签就会指定
  screen 定义文件的路径，并且在`#`后面制定用于显示的 screen 名字。

### 运行 Practice 应用

1. 现在，让我们来第一次运行我们创建的应用！
启动服务器，并且在浏览器中输入 `http://localhost:8080/practice/control/main`，浏览器应该出现
如下的页面显示 “This is first practice”。

![](http://placehold.it/400x300)

2. 在`practice`目录下创建一个 `index.jsp` 文件。这个文件用于当你访问 `http://localhost:8080/practice/` 这样的 URL 时做出响应。如果你输入 `http://localhost:8080/practice/unknown/request` 这样的 URL，请求将会按照 `web.xml` 中定义
的重定向路径进行跳转。在这种情况下，ContextFilter 会被用于过滤该请求，并且使用重定向路径来跳转请求。

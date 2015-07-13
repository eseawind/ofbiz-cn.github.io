# 第二部分

## UI 改进

### 创建装饰器

现在，让我们来为这个应用的页面创建一个装饰器。在 `widget` 目录创建一个 `CommonScreens.xml` 文件。
这个文件将包含公用的页面。一个公用的页面可以包含一个 header 和一个 footer，这样其他将这个公用页面做
为装饰器的页面就会包含这个 header 和 footer。

`CommonScreens.xml` 文件中的代码如下：

```html
<screen name="CommonPracticeDecorator">
      <section>
          <widgets>
               <decorator-section-include name="body"/>
          </widgets>
      </section>
</screen>
```

> 拓展阅读：[理解 OFBiz 的 Widget Toolkit](https://cwiki.apache.org/confluence/display/OFBIZ/Understanding+the+OFBiz+Widget+Toolkit)

在 `web.xml` 文件中添加对 `CommonScreens.xml` 文件的引用：

```xml
<context-param>
     <param-name>commonDecoratorLocation</param-name>
     <param-value>component://practice/webapp/practice/widget/CommonScreens.xml</param-value>
     <description>The location of the common-decorator screen to use for this webapp; referred to as a context variable in screen def XML files.</description>
</context-param>
```

### 创建菜单

在 `widget` 目录下创建文件 `PracticeMenus.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<menus xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://ofbiz.apache.org/dtds/widget-menu.xsd">
    <menu name="PracticeAppBar" title="PracticeApplication" extends="CommonAppBarMenu" extends-resource="component://common/widget/CommonMenus.xml">
        <menu-item name="main" title="Main"><link target="main"/></menu-item>
    </menu>
</menus>
```

在 CommonPracticeDecorator 中包含这个菜单文件：

```xml
<screen name="CommonPracticeDecorator">
        <section>
            <widgets>
                <include-menu location="component://practice/webapp/practice/widget/PracticeMenus.xml" name="PracticeAppBar"/>
                <decorator-section-include name="body"/>
            </widgets>
        </section>
</screen>
```

### 创建 action

在 `WEB-INF` 目录下创建一个子目录 `action`，这个目录将包含一些脚本语言。脚本语言用于为我们的页面
准备数据。我们将使用 groovy 语言作为我们的脚本语言。在早期版本的 OFBiz 中，也会用到 bsh 脚本语言。

> 拓展阅读： [Groovy 小技巧](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=6553850) & [Groovy 官网](http://www.groovy-lang.org/)。

> 注意：当使用 groovy 时，请保持良好的习惯，仅 import 用到的 class 和 package。使用 `Debug` 类
中的方法来进行 log。

在 `action` 文件夹下创建一个 `Person.groovy` 文件，这个脚本将用于从数据库中获取所有的 Person 记录。
文件中的代码如下：

```groovy
context.persons = delegator.findList("Person", null, null, null, null, false);
```

这行代码将从数据库中获取所有的 Person 记录，并将他们放在 persons 上下文中。Person 这个 list 将在
ftl 中被遍历显示所有的记录。

### 创建 ftl 文件

在 `practice` 文件中创建一个 `Person.ftl` 文件，用于显示 groovy 脚本获取的数据。

> 拓展阅读：[freemarker 文档](http://freemarker.org/docs/)

```
<#if persons?has_content>
  <h2>Some of the people who visited our site are:</h2>
  <br>
  <ul>
    <#list persons as person>
      <li>${person.firstName?if_exists} ${person.lastName?if_exists}</li>
    </#list>
  </ul>
</#if>
```

### 创建 person 页面

在 `PracticeScreens.xml` 文件中新加一个 `person` screen，并在 `PracticeMenus.xml` 文件
中新加一个菜单项：

```xml
<screen name="person">
    <section>
        <actions>
            <script location="component://practice/webapp/practice/WEB-INF/actions/Person.groovy"/>
        </actions>
        <widgets>
            <decorator-screen name="CommonPracticeDecorator" location="${parameters.commonDecoratorLocation}">
                <decorator-section name="body">
                    <platform-specific>
                        <html>
                            <html-template location="component://practice/webapp/practice/Person.ftl"/>
                        </html>
                    </platform-specific>
                </decorator-section>
            </decorator-screen>
        </widgets>
    </section>
</screen>
```

### 运行程序

修改 `controller.xml` 文件指向新的 screen。重启程序，并访问 `http://localhost:8080/practice/control/person`。

**程序输出的页面：**

![output](http://placehold.it/400x300)

## 创建用于显示 Person 实体的表单

Todo

## 为应用创建主装饰器

## 在 App bar 中显示应用

## 创建 UI labels

## 为应用添加用户认证

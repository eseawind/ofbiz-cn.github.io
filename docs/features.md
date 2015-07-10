# OFBiz 新功能特点

OFBiz 提供了丰富的功能特性，可以用于企业管理，电子商务，生产管理，项目管理，零售和贸易等等。
它可以被应用于各行各业，不同地区和不同规模的企业。

## 简介

OFBiz 提供了非常丰富的功能集，主要包括：

- 高级电子商务
- 分类目录管理
- 促销和定价管理
- 订单管理 (销售订单和采购订单)
- 客户管理
- 仓库管理
- 订单执行管理 (自动的库存更新，批量捡货、打包和装运)
- 记账功能 (发票，付款帐户、记帐帐户，固定资产)
- 生产管理
- 通用的工作流管理 (事件， 任务， 项目， 需求， 等等)
- 内容管理（网站，通用内容，博客，论坛，等等)
- 一个成熟的销售点模块 (POS)，使用 [XUI](http://xui.sourceforge.net/index.html) 作为客户端接口
- 国际化和本地化

## 基本应用

### 记账功能

- 发票，付款帐户、记帐帐户的功能页面
- 可以不用通过订单在发票上添加消费税
- FinAccount（银行账户）选择
- GlReconciliation
- 报表
  - 存货估价
  - 所得[损益]报表
  - 比较所得[损益]报表
  - 交易汇总
  - Gl Account Trail Balance
  - Monthly Trial Balance
  - Cost Center
  - 上述报表都可以被导出为 PDF/CSV 文件
  - 上述报表在显示的时候可以按照不同的群体进行汇总
  - OFBiz 中的支付网关 － "Chase Orbital"
  - 支持 Authorize.net 3.1
  - 集成了 RBS WorldPay
  - 在 Fin 下的记账账户和退货信息功能页面。群组（Party）的历史纪录 tab
  - 手工支付网关交易 （支持的有：authorise, capture, credit, refund, release）
  - 支持 CyberSource v5.0.2
  - 支付网关配置，以及应用到产品仓库支付方式的设置
  - 支持 PayPal
  - 支持 PayflowPro
  - 工资单明细
  - 不同货币支付
  - timeentries on an invoice overview
  - CapturePaymentsByInvoice service doesn't return not optional parameter while processing multiple orders invoice
  - 更新了 Cybersource 的配置和代码
  - 保存发票为模版
  - 设置按时段的支付形式（比如：“在30之后支付50%，在60天后支付40%，在90天后支付剩余的10%”）
  新增按照到期日期来选择发票的功能页面

### 内容管理和市场调查

- [Docbook](http://www.docbook.org) 文档和功能页面现在会在适合的主题中显示
- 内容上传页面支持选择已有内容，提高了内容的重用性
- 支持将 Docbook 的 xml 格式转化为 HTML 格式，[示例文档](http://demo-trunk-ofbiz.apache.org/cmssite/cms/APACHE_OFBIZ_HTML#N4009E)
- 改进的帮助系统

#### 市场调查

- 实现了设置调查问题默认值的功能
- 实现了 GEO 和 ENUMERATION 类型的调查问题
- 实现了在创建账户之前先进行邮箱验证的功能

### 人力资源管理

- 培训管理

### 生产管理 & 生产资源计划

- 定于产品模型和任务模版用于产品的生产
- 管理中间材料，市场，和最终产品的账单
- 包含一个集成的生产资源计划系统
- [更多详细信息](https://cwiki.apache.org/confluence/display/OFBIZ/Manufacturing+Management)

### 市场营销

管理联系人列表和进行大众传播（mass communications）。

### 销售自动化

管理账户，联系人和 Leads。监测机会的进展和管理预测。

### 请求，报价，订单和需求管理

- UPS 集成
- 支持在线获取 UPS 运费
- 在进行退货或者退款时，可以在原订单的基础上创建换货订单
- “立即更换”的退货类型，适用于不适合退货的情况，比如运费的成本比产品本身要高的情况
- 按发货地筛选订单
- 可是设置是否在网页上显示缺货的产品
- 可以在订单详情页面添加新的收获地址
- 可以在订单详情页面打印 PDF 格式的捡货清单
- 订单查询页面包含多种查询方法
- 标记订单为已检查
- 新增 "Wait Replacement Reserved" 的退货类型
- 在创建采购订单的时候，可以指定 orderId，如果不制定，将自动创建
- "cancelBackOrderDate"
- 在订单详情页面一次性添加多个产品
- 采购订单
- 消费税的计算
- 通过虚拟产品来实现可配置产品
- 将采购订单退回给供货商
- 保存订单项目

#### 信用卡支付

在订单组件中完成了对信用卡 security codes（[ccv2](http://baike.sogou.com/v5125106.htm)）的实现。


### 组织管理

- 添加关于组织、设备、容器和固定资产的地理位置演示数据
- 在 party 页面显示组织的状态和 externalId，只显示状态为 party_enabled 的组织
- 对财务历史页面的改进
- 改正了一个 “Address Match Map” CSV 上传的 bug
- 改进了 communication event 页面，创建了专用的 email 个内部备忘的表单
- 通过接收到的邮件来创建客户请求
- 当系统接收到不能识别的邮件时，添加可以快速创建 party 的表单
- 可以在自己的资料页面查看任务
- 新增一个定时的任务，每5分钟执行一次，用于发送邮件
- 更新了  Party Manager->View Profile->Subscriptions 页面

### 分类，产品，仓库，价格和库存管理

- Facility Tab 页面的 "Verify Pick" 可以用于验证捡货
- 改进了打包页面
- Facility-->Picking 新功能
- Picking screen 的改善
- FacilityRole 被迁移到 FacilityParty
- 产品目录的 “Inventory” 页面的改进
- 库存项目的查找
- FindProductConfigItems 页面转化为一个表单 widget
- status for non serialised items to the production run issuance routine
- 向可配置的产品添加评论
- 上传额外的产品视图（图片）
- Problems in virtual product's detail page if productFeatureTypeId contain non alfa characters
－ 在 productContent 表单上通过 contentId 创建链接
－ 运费总额添加优惠功能

#### 设备

- 新增  "Weight Package Only" 页面
- 改进了 UPS 集成功能

#### 库存

- 库存移动的搜索页面
- 导出仓库的库存报表为 CSV 文件

#### 促销

- 为 ProductPromoCode 实现了有效日期，现在可以为促销代码设定有效日期
- 改进了手动促销功能

### Work Effort 管理

- 支持 iCalendar
- iCalendar 支持被添加到 Work Effort 应用中
- 适用于 party 和 固定资产
- 支持 Temporal 表达式

## 特殊功能

- 资产管理和维护
- 商务智能 & 报告
- 电子商务集成
- LDAP 集成
- 用户自助服务
- POS & 网页 POS
- 项目管理
- 敏捷/极限 项目管理
- 支付方法集成
- 货运方法集成

## 国际化和本地化

目前 OFBiz 支持包括英语在内的23种不同的语言，总共有 15437 条目可以被翻译成不同的语言以实现
项目的本地化。详细翻译情况请参考[官方 Wiki](https://cwiki.apache.org/confluence/display/OFBIZ/Information+on+translated+languages+in+OFBiz)

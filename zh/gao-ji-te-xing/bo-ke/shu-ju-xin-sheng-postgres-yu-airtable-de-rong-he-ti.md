# 数据新生：Postgres 与 Airtable 的融合体

Teable 是一款无代码数据库产品，将以 Postgres 为代表的关系型数据库转换成了电子表格式的交互界面，将 Postgres 的强大能力无代码化，让低成本的构建强大可扩展的应用成为可能。

在这篇文章中，我们会描述对这款产品的设计理念，分享现在的产品进展。并且跟大家阐述将 Postgres 与 Airtable 结合会产生如此令人激动的全新能力以及我们对于未来无代码软件开发形式的愿景。

### 为 Postgres 构建一个漂亮的电子表格界面

数据库和电子表格像一对兄弟，一个严肃认真，性能卓越，为应用程序提供强有力的数据支撑，但是需要开发者通过代码来驱动。一个灵活易用，方便快捷，是现代办公体系中数据处理的基础设施，几乎是人人必备的基础技能。

我们创造了一个两者的有机结合体，不仅将普通的编辑操作转换为了所见即所得。还有机的继承了列数据类型的机制，使其其具有了创造强大，稳定应用的基础。

我们当然不是第一个尝试将表格界面作为数据库编辑方式的产品，有很多广为人知的解决方案都采用了类似的形式。但是我们希望更进一步，让一个团队中的所有成员都可以直接在同一个数据库上进行协作和搭建应用，并围绕这一点补充所有所需要的基础能力。

这是 Teable 的表格视图界面，在这里，你所看到的每一张表格，对应的都是 Postgres 中的一张物理表。&#x20;

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

我们成功地将数据库的表格，转换为类似于 Airtable 的电子表格界面，并且确保这个界面足够丝滑顺畅。

### 不仅是好看的 UI

针对不同的类型的数据，我们都提供了一个更合适这类数据的显示样式。而 Teable 在继承数据库基础的文本、数字、日期等类型之外，还额外提供了单多选、附件、评分等实用的字段类型。

比如一个字段被设置成了单选值，那么他就可以被展示成为一个颜色胶囊，并且拥有一个有限的可选集合，方便快速的进行选择和填充数据。同时在筛选和排序的时候，都能够比单纯的文本形式更加的边界，还可以作为视图分组项从而展示类似看板视图这类更定制化的视图形式。

#### 电子表格风格的编辑能力

不仅可以直接双击在单元格上面编辑数据，你还拥有一个可以进行随意选区的强大光标，光标可以进行复制，清除操作。选中单元格之后再进行粘贴。粘贴目标区域会自动的进行不同数值类型的转换，并且还能直接创建新的行和列。

#### 实时协同

所有操作都是实时协同的，无需刷新页面所有的协作者修改都会无缝的实现实时同步。包括 API 对数据的修改也会及时的反映到所有协作者的页面上。

#### 无限滚动

Postgres 可以轻松地处理数百万的数据，传统的翻页形式在浏览效率上十分低下，所以我们将他变成了无需翻页的顺畅滚动，你可以无限制的滚动到数据库的任意位置，就像在 google sheet 中那样

{% file src="../../.gitbook/assets/unlimited-scroll.mp4" %}

#### 组合式筛选

你可以添加带多级分组的筛选条件，这使得你可以根据优先级来组合"与"和"或"条件，并且针对不同的字段类型有不同的操作符可以作为选择&#x20;

<figure><img src="../../.gitbook/assets/image 3.png" alt="" width="563"><figcaption></figcaption></figure>

#### 多级排序能力

针对不同的字段组合排序情况，你可以自动排序（也就是按照排序条件查询）此时数据变化后顺序会自动按照条件更新。 或者一次性排序，数据变化后不会影响现在的排序，排序后可以再次手动拖拽调整顺序（我们通过在数据库中增加额外的顺序索引来实现这个能力）。&#x20;

<figure><img src="../../.gitbook/assets/image 4.png" alt="" width="375"><figcaption></figcaption></figure>

#### 拖拽顺序

你可以自由的选中行，然后简单的拖动它们进行顺序调整。

#### 迷你图

单元格可以显示灵动的小图表，当你的数据包含数字的时候，可以转换成一个带有进度的进度条，或者环形图，&#x20;

<figure><img src="../../.gitbook/assets/image 2.png" alt=""><figcaption></figcaption></figure>

### 数据关联和计算

另一个使用数据库而不是传统电子表格的重要原因就是跨表关联。关系型数据库非常擅长去建立不同数据模型之间的关联关系。通过建表+关联+引用计算的组合操作。我们就可以完成非常复杂的业务模型的建立，这在电子表格中是十分困难的。

#### Link 字段

我们借鉴了 Airtable 处理数据关联的基本方式。并在此基础上提供了更进一步的高阶能力。 不同的表之间可以通过 Link 字段来可视化的进行关联的创建，并且指定关联模式为一对一，多对一，一对多或者多对多。清晰的关联模式对构建更健壮的应用拥有很大的好处。

在表格中建立关联的同时，数据库中会自动的建立对应的 foreignKey 给后续的进一步数据库查询提供了强大的基础。

#### Lookup 字段

Lookup 字段可以进一步通过已有的关联字段来查询并获取其他表的数据，使得你可以轻松地创建联动的数据模型。对于开发者来说，可以认为这是一种简化的数据库 join。

#### Rollup 字段

Rollup 字段提供了对关联数据的聚合功能，如求和、平均值等、字符串链接等数组公式，帮助你更为方便地进行统计和分析。

#### Formula 字段

公式字段允许你定义自己的逻辑和计算规则。无论是简单的数学运算还是复杂的字符串处理，都可以使用熟悉的电子表格式公式来解决。

在拥有了上面一众的基础能力之后，数据库就不再是一个藏在机柜里面的引擎，而是一个可以直接与用户直接交互的强大工具。普通人也可以像开发者一样，来去搭建出解决自己业务场景的应用了。当然还有很多基础能力没有提到，但我相信你已经建立了对 Teable 的第一映像。

### 不只是支持百万行的 Airtable

Airtable 在表格式数据库交互方面确实提供了革命性的创新，并激发了我们无限的灵感，但在某些关键领域，它仍然受限于自身的设计。这些限制不仅影响了它的性能和扩展性，而且也限制了它在企业级应用场景中的应用潜力。

与Airtable的单表10万行限制相比，Teable 的能力上限是建立在真实的物理关系型数据库之上的。这意味着，每一个在 Teable 上看似简单的表格，背后都是一个真正的数据库表。因此，查询和存储性能在理论上是接近数据库本身的能力，所以我们可以说 Teable 对数据行数没有上限。 更进一步，我们测试了一张包含一百万行、20列填充满数据的表格，在没有进行索引优化的情况下，复杂的筛选或统计查询能在大约 200 毫秒内完成。但是，最引人注目的是，Teable 也支持索引优化。这意味着，你可以进一步提升查询性能，让 Teable 支持千万行甚至更多的数据。

超高的性能也意味着 Teable 可以提供更高的API请求频率，更重要的是，它继承了主流数据库管理系统的核心能力，使得开发者完全可以将 Teable 作为一个可靠的应用数据库，而在此之上能够构建出真正强大而且健壮的应用程序，并且大幅降低应用的运营成本。显而易见，通过 Teable API 来搭建应用，意味着你不再需要自己创建数据库，不在需要开发任何运营管理后台，不在需要专门的数据分析工具，任何需求的变化都能快速响应，开发者只需要聚焦核心的创意。

同时，Teable 的架构还带来了另一重要的能力，原生数据库能力，或者说 SQL 能力。这使得用户可以能够无缝集成各类数据库工具，确保数据消费链路的通畅无阻。同时还可以具有一个非常重要的特性：选择在享有云端便利性的同时自己来保管自己的数据。

### 数据所有权

在当今SaaS产品的海洋中，数据所有权是一座灯塔，指引着企业避免进入供应商锁定的迷雾。不同于那些封闭式的 SaaS工具，我们立足于为用户提供一个无代码的应用层，同时保留了用户对其数据完全控制的能力。这种独特的设计选择允许用户既可以便捷地使用我们托管的 Postgres 实例，也可以选择与自己部署的数据库实例相连 —— 例如，[Supabase](https://supabase.com/) 提供了一种优秀的解决方案。这意味着，即便您不是编程专家，也可以轻松构建一个完全自有的、功能强大的数据库环境。

更重要的是，我们尊重并支持标准的数据库结构，确保用户的数据不仅仅困于一个特定的平台。在 Teable 中创建的表格结构将会与数据库中的结构完全一致，这样，您不仅可以使用任何标准的数据库工具来进行数据迁移，还能通过SQL语句轻松地对数据进行查询。这种透明性和兼容性意味着您的数据策略可以随着业务的成长而自如地扩展，不受任何单一服务供应商的束缚。

### 分享几个使用案例

#### 直接在数据库上协作

**场景描述**： 企业可以通过 Teable 直接在数据库上进行协作，管理其核心业务流程，如客户关系管理、订单处理、库存监控和项目管理。比如，销售团队可以实时更新客户信息和销售漏斗状态，而库存管理人员可以追踪产品库存并及时调整采购计划。

**案例应用**： 一家中型电商公司利用 Teable 构建了一个综合业务管理系统。通过这个系统，他们能够同步客户的订单记录，实时更新库存状态，并将这些数据与正在进行的市场营销项目关联起来。这种集中化的信息管理方式提高了团队协作效率，加快了决策过程，并显著提升了客户满意度。

#### 使用 Teable API 开发 2C 应用

**场景描述**： 开发者可以利用 Teable 的强大 API 功能开发面向消费者（2C）的应用程序。同时，使用 Teable 的表格界面作为运营后台，方便非技术人员管理和更新应用内容，如更新产品列表、调整定价策略等。

**案例应用**： 一家创业公司开发了一个个性化旅游推荐应用，使用 Teable API 处理用户数据和推荐算法。他们还创建了一个用于内容管理的 Teable 后台，让市场团队能够轻松更新旅游目的地信息、特价活动和用户反馈。这种方式大大简化了运营流程，并使得产品更新更加迅速和灵活。

#### 使用 API 导入数据到 Teable 进行数据分析

**场景描述**： 企业可以利用 Teable 的 API 功能，将外部数据源（如市场调研数据、社交媒体分析结果等）导入到 Teable。这些数据可以被进一步整合、汇总和分析，以支持更复杂的商业决策过程。

**案例应用**： 一家市场研究公司通过 API 将多个数据源（如调查问卷结果、社交媒体趋势等）导入到 Teable。在 Teable 中，他们使用自定义公式和数据视图来进行深度分析，如消费者行为模式分析和市场趋势预测。这种集成方式极大地提高了他们的数据处理能力和报告准确度。

### 最后

这是一个经过深思熟虑的项目，历时数年的开发和对数据库管理简化的持续热情。

我们的征途始于一个明确且深刻的目标：让每个人都能轻松地管理和操作复杂的数据库应用。为了实现这一目标，我们倾注了大量时间进行研究和开发，努力屏蔽数据库管理的复杂性，并为您提供一个简洁、高效的解决方案。

我们都有过这样的体验——你希望可以通过低代码工具快速的将自己的业务需求，这些工具虽然功能数量众多，但往往无法跟随者业务的增长进行进一步扩展。这正是 Teable 发挥作用的地方！我们从像 Airtable 这样的工具中汲取灵感。

无论您是资深的开发者还是不懂数据库但想要完成自己应用的新手，您都可以在 Teable 中轻松管理数据，并实现复杂的数据库应用。我们的目标是弥合您的需求与功能完备的定制数据库应用之间的差距，让您能够迅速实现数据管理和应用构建，无论您的技术背景如何。

打造 Teable 并非一件轻松的事情；事实上，我们在开发过程中不断调整和优化我们的构想。我们目前的版本是经过反复试验和精心打磨的成果，我们迫不及待地希望您能亲身体验。我们相信 Teable 将彻底改变您对数据库管理和应用开发的看法。
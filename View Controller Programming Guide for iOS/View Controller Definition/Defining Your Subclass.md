## Defining Your Subclass

定义你的子类。

你使用UIViewController的自定义子类来展示应用的内容。大多数自定义视图控制器是内容视图控制器，那就是说，它们拥有它们的所有视图并负责这些视图的数据。相反，容器视图控制器不拥有它的所有视图；它的一些视图由其他视图控制器管理。定义内容和容器视图控制器的大多数步骤是相同的，在以下部分讨论。

对于内容视图控制器，最常用的父类如下：

- 在你的视图控制器的主视图是一个表时，特别地使用UITableViewController。
- 在你的视图控制器的主视图是一个集合视图时，特别地使用UICollectionViewController。
- 为其他所有视图控制器使用UIViewController。

对于容器视图控制器，父类取决于你是否正修改一个存在的容器类或创建你自己的。对于存在的容器，无论选择哪些你想要修改的视图控制器都行。对于新的容器视图控制器，你通常创建UIViewController的子类。关于创建一个容器视图控制器的额外的信息，查看Implementing a Container View Controller。

###### 定义你的UI

为视图控制器在视觉上定义UI使用Xcode里的storyboard文件。尽管你也能通过编程创建你的UI，storyboards是一种使视图控制器内容形象化和为不同的环境自定义视图层次结构（如需要）的极好的方式。在视觉上构建UI让你快速做出改变并让你不用构建并运行你的应用就能看到结果。

图4-1显示了一个storyboard的例子。每个长方形区域表示一个视图控制器和它关联的视图。在视图控制器之间的箭头是视图控制器关系和segues。Relationships连接一个容器视图控制器和它的子视图控制器。Segues让你在界面里视图控制器之间导航。

![storyboard_bird_sightings_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/storyboard_bird_sightings_2x.png)

每个新项目有一个通常已经包含一个或多个视图控制器的主storyboard。你可以通过从库拖拽它们到你的画布来添加新的视图控制器到你的故事板。新的视图控制器没有初始关联的类，所以你必须使用Identity检查器分配一个。

###### 处理用户交互

###### 在运行时显示你的视图

###### 管理视图布局

###### 高效管理内存
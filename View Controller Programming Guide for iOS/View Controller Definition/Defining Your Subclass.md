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

使用故事板编辑器来做接下来的：

- 为视图控制器添加、安排和配置视图。
- 连接输出口和动作；查看Handling User Interactions。
- 在视图控制器之间创建关系和segues；查看Using Segues。
- 为不同的尺寸类自定义布局和视图；查看Building an Apdaptive Interface。
- 添加手势识别器来和视图一起处理用户交互；查看Event Handling Guide for iOS。

如果你是刚使用故事板来构建界面，你可以在Start Developing iOS Apps Today里找到为创建基于故事板的用户界面的手把手指导。

###### 处理用户交互

应用的响应者对象处理即将到达的事件并取得合适的动作。尽管视图控制器是响应者对象，它们几乎不直接处理触摸事件。相反，视图控制器通常用以下方式处理事件：

- 视图控制器为处理高级事件定义动作方法。动作方法响应：
  - 特定的动作。控件和一些视图调用一个动作方法来报告特定的交互。
  - 手势识别器。手势识别器调用一个动作方法来报告手势的当前状态。使用你的视图控制器来处理状态变化和响应完全的手势。
- 视图控制器观察由系统或其他对象发出的通知。通知报告变化并且是一种视图控制器更新它的状态的一种方式。
- 视图控制器扮演另一个对象的数据源或代理。视图控制器经常被用于为表和集合视图管理数据。你也可以使用它们作为一个对象的代理，例如一个发送更新的位置值到它的代理的CLLocationManager对象。

响应事件经常需要更新视图的内容，要求拥有这些视图的引用。你的视图控制器是一个为任何你稍后需要修改的视图定义输出口的好地方。使用清单4-1显示的语法作为属性声明你的输出口。清单里的自定义类定义了两个输出口（由IBOutlet关键字指定）和一个单一的动作方法（由IBAction返回类型指定）。输出口保存对故事板里的一个按钮和文本域的引用，而动作方法响应按钮的点击。

```objective-c
@interface MyViewController : UIViewController
@property (weak, nonatomic) IBOutlet UIButton *myButton;
@property (weak, nonatomic) IBOutlet UITextField *myTextField;

- (IBAction)myButtonAction:(id)sender;

@end
```

```swift
class MyViewController: UIViewController {
    @IBOutlet weak var myButton : UIButton!
    @IBOutlet weak var myTextField : UITextField!
    
    @IBAction func myButtonAction(sender: id)
}
```

在故事板里，记住连接视图控制器输出口和动作来响应视图。在故事板文件里连接输出口和动作确保在视图被加载时它们被配置。关于如何在界面构建器里创建输出口和动作连接的信息，查看界面构建器连接帮助。关于如何在应用里处理事件的信息，查看iOS事件处理指南。

###### 在运行时显示你的视图

故事板使加载并显示视图控制器视图的过程非常简单。UIKit在它们需要时自动地从故事板文件加载视图。作为加载过程的一部分，UIKit执行以下系列任务：

1. 使用故事板文件里的信息实例化视图。

2. 连接所有输出口和动作。

3. 分配根视图给视图控制器的视图属性。

4. 调用视图控制器的`awakeFromNib`方法。

   在这个方法被调用时，视图控制器的特性集合为空并且视图可能不在他们的最终位置。

5. 调用视图控制器的`viewDidLoad`方法。

   使用这个方法来添加或移除视图，修改布局约束并为视图加载数据。

在屏幕上显示视图控制器的视图之前，UIKit给你一些额外的机会在这些视图在屏幕上之前和之后准备它们。特别地，UIKit执行以下任务序列：

1. 调用视图控制器的`viewWillAppear:`方法来让你知道它的视图将会在屏幕上显示。
2. 更新视图布局。
3. 在屏幕上显示视图。
4. 在视图在屏幕上时调用`viewDidAppear:`。

在你添加、移除或修改视图的大小和位置时，记住添加和移除任何应用到这些视图的约束。对视图层次结构做布局相关的改变导致UIKit标记布局为dirty。在下一个更新循环期间，布局引擎使用当前的布局约束计算视图的大小和位置并应用这些改变到视图层次结构。

关于如何不使用故事板来创建视图的信息，查看UIViewController Class Reference里的视图管理信息。

###### 管理视图布局

在视图的大小和位置变化时，UIKit为视图层次结构更新布局信息。对于使用自动布局配置的视图，UIKit使用自动布局引擎来根据当前布局约束更新布局。UIKit也让其他感兴趣的对象，例如活跃的展示控制器，知道布局变化以便他们能做出相应的响应。

在布局处理期间，UIKit在几个点通知你，以便你能执行额外的布局相关的任务。使用这些同志来修改布局约束或在布局约束已经被应用后对布局做出最终的微调。在布局处理期间，UIKit为每个被影响的视图控制器做以下：

1. 如需要，更新视图控制器和它的视图的特性集合；查看When Do Trait and Size Changes Happen？

2. 调用视图控制器的`viewWillLayoutSubviews`方法。

3. 调用当前UIPresentationController对象的`containerViewWillLayoutSubviews`方法。

4. 调用视图控制器的根视图的`layoutSubviews`方法。

   这个方法的默认实现使用可用的约束计算新的布局信息。这个方法然后穿过视图的层次结构并为每个子视图调用`layoutSubviews`

5. 应用计算的布局信息到视图。

6. 调用视图控制器的`viewDidLayoutSubviews`方法。

7. 调用当前UIPresentationController对象的`containerViewDidLayoutSubviews`方法。

视图控制器可以使用`viewWillLayoutSubviews`和`viewDidLayoutSubviews`方法来执行可能影响布局处理的额外的更新。在布局之前，你可以添加或移除视图，更新视图的大小和位置，更新约束，或更新其他视图相关的属性。在布局后，你可以重新加载表数据，更新其他视图的内容，或对视图的大小和位置做出最终的调整。

这里是一些有效管理布局的技巧：

- 使用自动布局。你使用自动布局创建约束是一种灵活和容易的方式来在不同大小的屏幕上部署内容。
- 利用top和bottom布局指导的优势。布局内容到这些指导确保你的内容总是可见的。top布局指导因子和位置在状态栏和导航栏的高度里。类似地，bottom布局指导因子的位置在标签栏和工具栏和高度里。
- 在添加或移除视图时记得更新约束。如果你动态地添加或移除视图，记住更新相应的约束。
- 在动画视图控制器的视图时临时移除约束。在使用 UIKit Core Animation动画视图时，动画持续期间移除约束并在动画完成后把它们添加回来。如果在动画期间视图的位置和大小变化了记得更新约束。

关于展示控制器和他们在视图控制器结构体系里扮演的角色的信息，查看The Presentation and Transition Process。

###### 高效管理内存

尽管内存分配的大多数方面是为你做出决定的，表4-1列出了你最可能分配或取消分配内存的地方的视图控制器方法。大多数取消分配需要移除对对象的强引用。要移除对一个对象的强引用，设置那个对象的属性和变量为nil。


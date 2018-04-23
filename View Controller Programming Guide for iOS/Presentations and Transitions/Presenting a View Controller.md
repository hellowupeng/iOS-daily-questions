## Presenting a View Controller

呈现视图控制器。

有两种在屏幕上显示视图控制器的方式：嵌入它到容器视图控制器里或呈现它。容器视图控制器提供应用的主要导航，但是呈现视图控制器也是一种重要的导航工具。你使用直接的呈现来在当前视图控制器上显示一个新的视图控制器。典型地，在你想要实现模态界面时呈现视图控制器，但你也可以使用它们用于其他目的。

支持呈现视图控制器是内置于UIViewController类的并可用于所有的视图控制器对象。你可以从任何其他视图控制器呈现任何视图控制器，尽管UIKit可能使请求改道到不同的视图控制器。呈现视图控制器在原来的视图控制器器（作为呈现视图控制器而被熟知）和新的视图控制器（作为被展示视图控制器而被熟知）之间创建一种关系。这种关系形成视图控制器层次结构的一部分并保留下来知道被呈现视图控制器被解雇。

###### 展示和过渡过程

展示视图控制器是一种动画新内容到屏幕上的快速和简单的方式。内置于UIKit的展示机器让你使用内置或自定义动画显示新的视图控制器。由于UIKit处理所有的工作，内置展示和动画需要非常少的代码。你也可以用少量额外的努力创建自定义的展示和动画并和任何视图控制器一起使用它们。你可以通过编程或使用segues初始化视图控制器的展示。如果你在设计时知道应用的导航，segues是初始化展示的最简单的方式。对于更动态的界面，或者在没有专有控制初始化segue地方的情况，使用UIViewController的方法来呈现视图控制器。

**展示样式**

视图控制器的展示样式决定它在屏幕上的外观。UIKit定义了许多标准的展示样式，每个伴随特定的外观和意图。你也可以定义你自己的自定义展示样式。在设计应用时，为你正在尝试做的什么选择最有意义的展示样式，分配合适的常量到你想要呈现的视图控制器的`modalPresentationStyle`属性。

**全屏展示样式**

全屏展示样式覆盖整个屏幕，阻止与潜在内容的交互。在一个水平正常的环境里，只有一个全屏样式完全地覆盖底部内容。剩余部分纳入模糊或透明化视图来允许底部视图控制器的一部分持续显示。在水平紧凑环境里，全屏展示自动地采用`UIModalPresentationFullScreen`样式并覆盖所有底部内容。

图8-1说明了在水平正常环境里使用`UIModalPresentationFullScreen`，`UIModalPresentationPageSheet`和`UIModalPresentationFormSheet`样式的展示的外观。图中，左上方绿色的视图控制器呈现右上方蓝色的视图控制器，每个展示样式的结果在下面显示。对于一些展示样式，UIKit在两个视图控制器内容之间插入一个模糊视图。

![VCPG_PresentationStyles _fig_8-1_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG_PresentationStyles _fig_8-1_2x.png)

> ###### 注意
>
> 在使用UIModalPresentationFullScreen样式呈现视图控制器时，UIKit通常在过渡动画完成后移除底部视图控制器的视图。你可以通过指定UIModalPresentationOverFullScreen样式作为替代来阻止这些视图的移除。你可以在被呈现视图控制器有让底部内容显示的透明区域时使用那个样式。

在使用全屏展示样式时，初始化展示的视图控制器自己必须覆盖整个屏幕。如果展示视图控制器没有覆盖屏幕，UIKit遍历视图控制器层次结构直到它找到覆盖屏幕的一个视图控制器。如果他不能找到一个充满屏幕的中间视图控制器，UIKit使用窗口的根视图控制器。

**Popover样式**

`UIModalPresentationPopover`样式使用一个popover视图显示视图控制器。Popovers对于显示有关关注或选中对象的额外信息或条目列表是有用的。在水平正常的环境里，popover视图只覆盖部分屏幕，如图8-2所示。在水平紧凑环境里，popovers默认采用UIModalPresentationOverFullScreen展示样式。在popover视图外的点击自动清除popover。

![VCPG_popover-style_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG_popover-style_2x.png)

由于popovers在水平紧凑环境里采用全屏展示，你通常需要修改你的popover来处理这种适应。在全屏模式里，你需要一种清除被展示的popover的方式。你可以通过在可取消的容器视图控制器里添加一个按钮，嵌入popover来这样做，或者改变它自己的适应行为。

关于如何配置popover展示的技巧，查看Presenting a View Controller in a Popover。

**当前上下文样式**

`UIModalPresentationCurrentContext`样式在你的界面里覆盖一个指定的视图控制器。在使用与环境相关的样式时，你通过设置它的`definesPresentationContext`属性为YES来指定你想要覆盖的视图控制器。图8-3说明了只覆盖拆分视图控制器的一个子视图控制器的current context presentation。

![VCPG_CurrentContextStyles_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG_CurrentContextStyles_2x.png)

> ###### 注意
>
> 在使用`UIModalPresentationFullScreen`样式呈现一个视图控制器时，UIKit通常在过渡动画结束后移除底部视图控制器的视图。你可以通过指定UIModalPresentationOverCurrentContext样式作为替代来阻止这些视图的移除。你可以在被展示的视图控制器有让底部内容显示的透明区域时使用那个样式。

定义展示上下文的视图控制器也能在展示期间定义过渡动画来使用。

**自定义展示样式**

**过渡样式**

**呈现于显示视图控制器**

###### 呈现视图控制器

**显示视图控制器**

**模态地呈现视图控制器**

**用Popover呈现视图控制器**

###### 清除被呈现的视图控制器

###### 呈现定义在不同故事板里的视图控制器
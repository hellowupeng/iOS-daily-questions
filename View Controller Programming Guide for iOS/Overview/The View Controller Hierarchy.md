## The View Controller Hierarchy

视图控制器层次结构。

应用的视图控制器之间的关系定义了每个视图控制器需要的行为。UIKit希望你用规定的方式使用视图控制器。维护合适的视图控制器关系保证了在它们需要时自动的行为被递送到正确的视图控制器。如果你打破规定的控制和呈现关系，应用的一部分会如预期的停止运转。

###### 根视图控制器

根视图控制器是视图控制器层次结构的支柱。每个窗口（window）都有一个内容充满那个窗口的根视图控制器。根视图控制器定义用户看到的初始内容。图2-1展示了根视图控制器和窗口之间的关系。因为窗口没有自己的可见内容，视图控制器的视图提供了所有内容。

![VCPG-root-view-controller_2-1_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-root-view-controller_2-1_2x.png)

根视图控制器从UIwindow对象的rootViewController属性获取。在你使用storyboards来配置视图控制器时，UIKit在启动时自动设置rootViewController属性的值。你通过编程创建的窗口，你必须自己设置根视图控制器。

###### 容器视图控制器

容器视图控制器让你从更易操作和能重复使用的块组装复杂界面。容器视图控制器混合一个或多个子视图控制器内容和可选的自定义视图一起来创建它的最终界面。例如，UINavigationController对象从子视图控制器和导航栏和可选的工具栏一起显示内容。UIKit包含多个容器视图控制器，包括UINavigationController，UISplitViewController和UIPageViewController。

容器视图控制器的视图总充满给到它的空间。容器视图控制器经常被作为一个窗口里的根视图控制器来安装（如图2-2所示），但它们也能被模态地呈现或作为其他容器的子视图控制器来安装。容器负责合适地放置它的子视图。在下图里，容器并排着安置两个子视图。尽管它依赖于容器界面，子视图控制器可能有容器和任何兄弟视图控制器的最小知识。

![VCPG-container-acting-as-root-view-controller_2-2_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-container-acting-as-root-view-controller_2-2_2x.png)

由于容器视图控制器管理它的子视图控制器，UIKit为在自定义容器里如何设置这些子视图控制器定义了规则。关于如何创建自定义容器视图控制器的详细信息，查看实现容器视图控制器。

###### 被展示的视图控制器

展示一个视图控制器用新的视图控制器的内容替换当前视图控制器的内容，通常会隐藏之前的视图控制器的内容。Presentations 最常用于模态地显示新内容。例如，你可以展示一个视图控制器来收集用户输入。你也可以使用它们作为应用界面的通用构建块。

在展示一个视图控制器时，UIKit在展示视图控制器和被展示视图控制器之间创建一种关系，如图2-3所示。（也存在一种从被展示的视图控制器回到它的展示视图控制器的相反关系。）这些关系形成部分视图控制器层次结构和一种在运行时定位其他视图控制器的方式。

![VCPG-presented-view-controllers_2-3_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-presented-view-controllers_2-3_2x.png)

在容器视图控制器被调用时，UIKit可以修改展示链来简化你不得不写的代码。不同的展示样式对它们如何在屏幕上出现有不同的规则，例如，全屏展示总是覆盖整个屏幕。在你呈现一个视图控制器时，UIKit查找一个为展示提供合适的上下文的视图控制器。在许多情况下，UIKit选择最近的容器视图控制器但是他也可能选择窗口的根视图控制器。在一些情况下，你也能告诉UIKit哪个视图控制器定义了展示上下文并且应该处理这次展示。图2-4展示了为什么容器通常为展示提供上下文。在执行全屏展示时，新视图控制器需要覆盖整个屏幕。而不是要求子视图控制器知道它的容器的边界，容器决定是否处理展示。因为在这个例子里导航控制器覆盖整个屏幕，它扮做展示视图控制器并开始展示。

![VCPG-container-and-presented-view-controller_2-4_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-container-and-presented-view-controller_2-4_2x.png)

关于Presentations的信息，查看The Presentation and Transition Process。	
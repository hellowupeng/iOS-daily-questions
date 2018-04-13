## The View Controller Hierarchy

视图控制器层次结构。

应用的视图控制器之间的关系定义了每个视图控制器需要的行为。UIKit希望你用规定的方式使用视图控制器。维护合适的视图控制器关系保证了在它们需要时自动的行为被递送到正确的视图控制器。如果你打破规定的控制和呈现关系，应用的一部分会如预期的停止运转。

###### 根视图控制器

根视图控制器是视图控制器层次结构的支柱。每个窗口（window）都有一个内容充满那个窗口的根视图控制器。根视图控制器定义用户看到的初始内容。图2-1展示了根视图控制器和窗口之间的关系。因为窗口没有自己的可见内容，视图控制器的视图提供了所有内容。

![VCPG-root-view-controller_2-1_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-root-view-controller_2-1_2x.png)

根视图控制器从UIwindow对象的rootViewController属性获取。在你使用storyboards来配置视图控制器时，UIKit在启动时自动设置rootViewController属性的值。你通过编程创建的窗口，你必须自己设置根视图控制器。

###### 容器视图控制器

容器视图控制器让你从更易操作和能重复使用的块组装复杂界面。容器视图控制器混合一个或多个子视图控制器内容和可选的自定义视图一起来创建它的最终界面。例如，UINavigationController对象从子视图控制器和导航栏和可选的工具栏一起显示内容。UIKit包含多个容器视图控制器，包括UINavigationController，UISplitViewController和UIPageViewController。

容器视图控制器的视图总充满给到它的空间。容器视图控制器经常被作为一个窗口里的根视图控制器来安装（如图2-2所示），但它们也能被模态地呈现或作为其他容器的子视图控制器来安装。

###### 被展示的视图控制器
## Implementing a Container View Controller

实现一个容器视图控制器。

容器视图控制器是一种从多个视图控制器结合内容到单个用户界面的方式。容器视图控制器最常用于促进导航和创建新的基于存在的内容的用户界面类型。UIKit里的容器视图控制器例子包括UINavigationController，UITabBarController和UISplitViewController，所有的这些在不同的用户界面之间促进导航。

###### 设计一个自定义的容器视图控制器

在几乎每种方式里，容器视图控制器像其他任何内容视图控制器一样管理一个根视图和一些内容。差别是容器视图控制器从其他视图控制器取得部分内容。它获取的内容受限于嵌入到它自己的视图层次结构里的其他视图控制器的视图。容器视图控制器设置嵌入的视图的大小和位置，但是原来的视图控制器仍然管理这些视图里的内容。

在设计你自己的容器视图控制器时，总是理解容器和包含的视图控制器之间的关系。视图控制器的关系可以帮助通知他们的内容应该如何在屏幕上出现和你的容器如何在内部管理他们。在设计过程期间，询问你自己以下问题：

- 容器的角色是什么和它的子视图控制器们扮演什么角色？
- 多少子视图控制器同时显示？
- 兄弟视图控制器之间的关系是什么？
- 子视图控制器们如何从容器添加或移除？
- 子视图控制器的大小和位置能变化吗？这些变化在什么条件下发生？
- 容器提供任何它自己的装饰性的或导航相关的视图吗？
- 容器和它的子视图控制器之间要求的通信种类是什么？除了UIViewController类定义的标准事件外容器需要报告特定的事件给它的子视图控制器吗？
- 容器的外观能用不同的方式配置吗？如果可以，怎样做？

在你已经定义了各种各样的对象的角色后，容器视图控制器的实现是相当简单的。来自UIKit的唯一要求是在容器视图控制器和子视图控制器之间建立一种正式的父子关系。父子关系确保子视图控制器接收任何有关的系统消息。除此之外，大多数实际工作在包含的视图的布局和管理期间发生，对于每个容器都不同。你可以在容器的内容区域的任何地方放置视图并无论以你想要的任何方式调整这些视图。你也可以添加自定义视图到视图层次结构来在导航里提供装饰或辅助。

**示例：导航控制器**

UINavigationController对象支持通过一个层级数据集导航。一个导航界面一次展示一个子视图控制器。界面顶部的导航栏显示在数据层级里的当前位置并显示一个返回按钮来回退一级。进入数据层级的导航是左边的到子视图控制器并能影响表和按钮的使用。

视图控制器之间的导航由导航控制器和它的子视图控制器们共同管理。在用户和按钮或子视图控制器和表视图行交互时，子视图控制器要求导航控制器推一个新的视图控制器到视图。子视图控制器处理新的视图控制器的内容的配置，但是导航控制器管理过渡动画。导航控制器也管理显示一个返回按钮用于清除最高的视图控制器的导航栏。

图5-1显示了导航控制器和它的视图的结构。大多数内容区域由最高的子视图控制器填满，只有小部分由导航栏占用。

![VCPG_structure-of-navigation-interface_5-1_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG_structure-of-navigation-interface_5-1_2x.png)

在紧凑和正常环境里，导航控制器一次只显示一个子视图控制器。导航控制器改变它的子视图控制器的大小来适应可用的空间。

**示例：拆分视图控制器**

拆分视图控制器在一个主-从安排里显示两个视图控制器的内容。在这个安排里，一个视图控制器（主视图控制器）决定其他视图控制器显示什么详情。两个视图控制器的可见性是可配置的但是也是由当前环境决定的。在正常水平水平的环境里，拆分视图控制器能并排显示两个子视图控制器或者如果需要它能隐藏主视图控制器并显示它。在紧凑环境里，拆分视图控制器一次只显示一个视图控制器。

图5-2显示了在正常水平环境里拆分视图界面和它的视图的结构。拆分视图控制器默认只有它自己的容器视图。在这个例子里，两个子视图并排显示。子视图的大小和主视图的可见性一样是可配置的。

![VCPG-split-view-inerface_5-2_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/VCPG-split-view-inerface_5-2_2x.png)

###### 在界面构建器里配置容器

为了在设计时创建一个父子容器关系，添加一个容器视图对象到你的故事板场景，如图5-3所示。容器视图对象是一个代表子视图控制器内容的占位对象。使用那个视图来调整和安置容器里其他视图们关联的子视图控制器的根视图。

![container_view_embed_2x](/Users/andywu/Documents/iOS-daily-questions/View Controller Programming Guide for iOS/images/container_view_embed_2x.png)

在你用一个或多个容器视图加载一个视图控制器时，界面构建器也加载与这些视图关联的子视图控制器。子视图控制器必须和父视图控制器同时实例化，以便合适的父子关系能被创建。

如果你不使用界面构建器来设置你的父子容器关系，你必须通过编程添加每个子视图控制器到容器视图控制器创建这些关系，在Adding a Child View Controller to Your Content里描述。

###### 实现一个自定义的容器视图控制器

为了实现一个容器视图控制器，你必须在视图控制器和它的子视图控制器之间建立关系。在你尝试管理子视图控制器的视图之前建立这些父-子关系是必要的。这样做让UIKit知道你的视图控制器正在管理子视图控制器的大小和位置。你可以在界面构建器里创建这些关系或者通过编程创建它们。在通过编程创建父-子关系时，显示地添加和移除子视图控制器作为你的视图控制器设置的一部分。

**添加子视图控制器到内容**

为了通过编程合并子视图控制器到你的内容，通过做以下事情来在有关的视图控制器之间创建一种父-子关系。

1. 调用容器视图控制器的`addChildViewController:`方法。

   这个方法告诉UIKit你的容器视图控制器现在管理子视图控制器的视图。

2. 添加子视图控制器的根视图到容器的视图层次结构。

   总是记住设置子视图的frame的大小和位置作为这个过程的一部分。

3. 为管理子视图控制器的根视图大小和位置添加约束。

4. 调用子视图控制器的`didMoveToParentViewController:`方法。

清单5-1展示了一个容器如何在它的容器里嵌入一个子视图控制器。在建立父-子关系后，容器容器设置子视图的frame并添加子视图控制器的视图到它自己的视图层次结构。

设置子视图控制器的视图的frame size是重要的并且确保视图在容器里正确地显示。在添加视图后，容器调用子视图控制器的`didMoveToParentViewController:`方法来给子视图控制器一个机会响应视图所有权的改变。

```objective-c
- (void)displayContentController: (UIViewController *)content {
    [self addChildViewController: content];
    content.view.frame = [self frameForContentController];
    [self.view addSubview: self.currentClientView];
    [content didMoveToParentViewController: self];
}
```

在前面的例子里，注意你只调用子视图控制器的`didMoveToParentViewcontroller:`方法。那是因为`addChildViewController:`方法为你调用了子视图控制器的`willMoveToParentViewController:`方法。你必须自己调用`didMoveToParentViewController:`方法的原因是这个方法不能被调用直到在你嵌入子视图控制器的视图到你的容器视图控制器的视图层次结构里后。在使用自动布局时，在添加子视图控制器到容器的视图层次结构后在容器和子视图控制器之间设置约束。你的约束应该只影响子视图控制器的根视图的大小和位置。不要在子视图控制器的视图层次结构里更改根视图和任何其他视图的内容。

**移除子视图控制器**

为了从内容移除子视图控制器，通过做以下事情来在视图控制器之间移除父-子关系。

1. 以值nil调用子视图控制器的`willMoveToParentViewController:`方法。
2. 移除任何你配置的子视图控制器的根视图的约束。
3. 从容器的视图层次结构移除子视图控制器的根视图。
4. 调用子视图控制器的`removeFromParentViewController`方法来最后确定父子关系的结束。

移除子视图控制器永久地中断父-子间的关系。移除子视图控制器只在你不再需要引用它时。例如，导航控制器在一个新的视图控制器被推入到导航栈时不移除它当前的子视图控制器们。它只在它们被弹出栈时移除它们。

清单5-2向你展示了如何从它的容器移除子视图控制器。以值nil调用`willMoveToParentViewController:`方法给子视图控制器一个机会来为改变做准备。`removeFromParentViewController`方法也调用子视图控制器的`didMoveToParentViewController:`方法，传入那个方法一个nil值。设置父视图控制器为nil从你的容器最后确定子视图控制器的移除。

```objective-c
- (void)hideContentController: (UIViewController *)content {
    [content willMoveToParentViewController: nil];
    [content.view removeFromSuperView];
    [content removeFromParentViewController];
}
```

**在子视图控制器之间过渡**

在你想要动画一个子视图控制器用另一个接替时，合并子视图控制器的增加和移除到过渡动画过程。在动画之前，确保两个子视图控制器是内容的一部分但是让当前的子视图控制器知道它将要离开。在动画期间，移动新的子视图控制器的视图到位置并移除老的子视图控制器视图。在动画结束时，完成子视图控制器的移除。

清单5-3展示了一个用一个过渡动画用另一个交换子视图控制器的示例。在这个例子里，新的视图控制器被动画到被移动到屏幕外的存在的子视图控制器当前占用的矩形区域。在动画结束后，完成块从容器移除子视图控制器。在这个例子里，`transitionFromViewController:toViewController:duration:options:completion:`方法自动更新容器的视图层次结构，所以你不需要自己添加和移除视图。

```objective-c
- (void)cycleFromViewController: (UIViewController *)oldVC toViewController: (UIViewController *)newVC {
    [oldVC willMoveToParentViewController:nil];
    [self addChildViewController:newVC];
    
    newVC.view.frame = [self newViewStartFrame];
    CGRect endFrame = [self oldViewEndFrame];
    
    [self transitionFromViewController:oldVC toViewController:newVC duration:0.25 options:0 animations: ^{
        newVC.view.frame = oldVC.view.frame;
        oldVC.view.frame = endFrame;
    }
     completion:^(BOOL finished) {
         [oldVC removeFromParentViewController];
         [newVC didMoveToParentViewController:self];
     }];
}
```

**为子视图控制器们管理Appearance更新**

在添加子视图控制器到容器后，容器自动转发外观相关的消息给子视图控制器。这通常是你想要的行为，因为他确保了所有事件被正确地发送。然而，有时候默认的行为可能用一种对你的容器不合理的顺序发送这些事件。例如，如果多个子视图控制器同时改变它们的视图状态，你可能想要合并这些改变以便外观回调用一种更合理的顺序同时发生。

为了接管appearance回调的职责，在容器视图控制器里覆盖`shouldAutomaticallyForwardAppearanceMethods`方法并返回NO，如清单5-4所示。返回NO让UIKit知道你的容器视图控制器通知外观改变的子视图控制器们。

```objective-c
- (BOOL)shouldAutomaticallyForwardAppearanceMethods {
    return NO;
}
```

在一个appearance过渡发生时，合适的调用子视图控制器的`beginAppearanceTransition:animated:`或`endAppearanceTransition`方法。例如，如果容器有一个由一个`child`属性引用的单一子视图控制器，如清单5-5所示，容器会转发这些消息到子视图控制器。

```objective-c
- (void)viewWillAppear:(BOOL)animated {
    [self.child beginAppearanceTransition: YES animated: animted];
}

- (void)viewDidAppear:(BOOL)animated {
    [self.child endAppearanceTransition];
}

- (void)viewWillDisappear:(BOOL)animated {
    [self.child beginAppearanceTransition: NO animated: animated];
}

- (void)viewDidDisappear:(BOOL)animated {
    [self.child endAppearanceTransition];
}
```

###### 构建一个容器视图控制器的建议

设计、开发、测试一个新的容器视图控制器需要时间。尽管单个行为是简单的，但是控制器作为一整个会相当复杂。在实现容器类时考虑以下技巧。

- 只访问子视图控制器的根视图。容器只应该访问每个子视图控制器的根视图，那就是说，由子视图控制器的view属性返回的视图。它绝不应该访问子视图控制器的任何其他视图。
- 子视图控制器应该对它们的容器有最少的了解。子视图控制器应该专注它自己的内容。如果容器允许它的行为被子视图控制器影响，它应该使用代理设计模式来管理这些交互。
- 首先使用正常的视图设计容器。使用正常的视图（而不是来自子视图控制器的视图）给你机会在一个简化的环境里测试布局约束和动画过渡。在正常的视图如期工作时，用子视图控制器的视图交换它们。

###### 委托控制给子视图控制器

容器视图控制器可以委托它自己的appearance的某部分给它的子视图控制器。你可以用以下方式来委托控制：

- 让子视图控制器决定状态栏样式。为了委托状态栏appearance给子视图控制器，在容器视图控制器里覆盖`childViewControllerForStatusBarStyle`和`childViewControllerForStatusBarHidden`方法。
- 让子视图控制器指定它自己的偏好尺寸。拥有灵活布局的容器可以使用子视图控制器自己的`preferredContentSize`属性来帮助决定子视图控制器的大小。
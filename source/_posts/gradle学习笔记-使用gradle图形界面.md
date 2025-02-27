title: gradle学习笔记-使用gradle图形界面
date: 2016-07-22 15:06:55
tags:
  - Android
categories:
  - 工程开发
  - Android
comments: true
---

> 除了支持传统的命令行界面，Gradle提供了一个图形用户界面。可以通过`--gui`选项来开启。

**Example 10.1. Launching the GUI**

```
gradle --gui
```

> 请注意这条命令会阻塞进程直到关闭GUI，在*nix下，使用`gradle --gui&`这条命令，使得它运行在一个后台任务中是一个更好的选择。

如果你在你的gradle工作目录下运行了这个命令，你将看到一个task树。

**Figure 10.1. GUI Task Tree**

![](/img/gradle/guiTaskTree.png)

最好在你的gradle项目下运行这条命令，以便于用户界面的设置将存储在您的项目目录中。你也可以运行这条命令后通过用户界面中的“设置”选项卡更改工作目录。

用户界面顶部有4个tab，底部有一个输出窗口。

## 10.1. Task Tree

任务树显示了所有项目和它们的任务的分层显示。双击一个任务可以执行它

顶部有一个过滤器按钮，您可以通过过滤器按钮切换过滤器。编辑筛选器允许您配置任务和项目所显示的项目。隐藏的task出现在红色区域中。注意：新创建的task将默认出现（相对的默认被隐藏）。

任务树上下文菜单提供以下选项：

- 执行忽略依赖关系。不需要依赖的项目被重建（类似于`-a` 选项）。
- 将task添加到favorites中（见favorites tab）
- 隐藏选择的task。
- 编辑build.gradle文件。注意：这需要你的java版本>= 1.6，并且你有和系统相匹配的.gradle文件。

## 10.2. Favorites

Favorites tab是一个很好的用来存储经常被使用命令的地方。这里可以是一些很复杂的命令（只要是符合gradle规范的），并且你可以为他们提供一个显示名称。这可以用于：创建、说、自定义一个明确跳过测试的构建命令，文档，和样品，你可以称之为“快速构建”。

你可以对你列出的favorites进行重新排序，并且导出到磁盘上，并且被其他人引用。

## 10.3. Command Line

命令行界面你可以直接输入一条gradle命令。在你将命令添加到Favorites中，这也是一个让你尝试gradle命令的好地方。

## 10.4. Setup

设置选项卡允许一些一般设置的配置。

**Figure 10.2. GUI Setup**

![](/img/gradle/guiSetup.png)

- 当前目录

	设置你的gradle项目的根目录（通常是build.gradle文件的所在位置）

- 堆栈跟踪输出

	取决于当你的错误发生时，有多少信息被写入。提示：如果在命令行或“Favorites”选项卡上指定了堆栈跟踪级别，则将覆盖该堆栈跟踪级别。
	
- 只有当错误发生时才显示输出

	启用此选项隐藏任务执行时的任何输出，除非生成失败。

- 使用自定义gradle执行器 - 高级功能

	这为你提供了另一种方式推出Gradle的命令。如果你的项目需要一些额外的设置，这将会很有用，这是在一个批处理文件或脚本（如指定init脚本）内完成的。







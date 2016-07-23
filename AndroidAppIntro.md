1. Android应用程序的组成部分
Android 应用程序由松散耦合的组件构成，并使用应用程序Manifest绑定到一起；应用程序Manifest描述了每一个组件和他们之间交互的方式，还用于指定应用程序的元数据，其硬件和平台要求，外部库以及必需的权限。
以下几个组件提供了应用程序的基本结构模块：
	1. Activity 应用程序的表示层。应用程序中的每一个UI都是通过Activity类的一个或多个扩展实现的。Activity使用Fragment和视图来布局和显式信息，以及响应用户动作。在桌面开发环境中，Activity就相当于Form。
	2. Service 应用程序中不可见的工作者。Service组件在运行时没有UI，它们可以更新数据源和Activity，触发通知和广播Intent。它们被用来执行一些运行时间长的任务，或者不需要用户交互的任务（例如，即使当应用程序的Activity不是活动的或者可见的时候也需要继续进行的网络查找或其他网络任务）。
	3. Content Provider 一个可共享的持久数据存储器。Content Provider用来管理和持久化应用程序数据，通常会与SQL数据库交互。Content Provider是在应用程序之间共享数据的首选方法。可以通过配置自己的Content Provider来允许其他应用程序访问，也可以访问其他应用程序提供的Content Provider。 Android设备包含了多个本地Content Provider来提供有用的数据库，如媒体库和联系人信息等。
	4. Intent 一个强大的应用程序间的消息传递框架。Android中大量使用了Intent。Intent可以用来启动和停止Activity和Service。在系统范围内或向目标Activity，Service或Broadcast Receiver广播消息，以及请求对特定的一条数据执行操作。
	5. Broadcast Receiver Intent侦听器。Broadcast Receiver使应用程序可以监听那些匹配指定的过滤标准的Intent广播。Broacast Receiver会自动地启动应用程序来响应某个收到的Intent，这个特点使它们成为了事件驱动的应用程序的最佳选择。
	6. Widget 通常添加设备到设备的主屏幕的可视化应用程序组件。Widget是Broadcast Receiver的特殊变体，可用于创建动态的交互式应用程序组件，用户可以把这些组件添加到他们的主屏幕上。
	7. Notification Notification允许向用户发送信号，但却不会过分吸引它们的注意力或者打断它们当前的Activity。它们是应用程序不可见或者不活动（特别是Service 或者Broadcast Receiver）吸引用户注意的首选方法。

2. 应用程序的Manifest
	1. 每个Android项目都包含一个Manifest文件 ----- Android Manifest.xml，它存储在项目层次中的最底层。Manifest可以定义引用程序及其组件和需求的结构和元数据。
	2. 它包含了组成应用程序的每一个Activity，Services，Content Provider，和Broadcast Receiver的节点，并使用Intent Filter和权限来确定这些组件之间以及这些组件和其他应用程序是如何交互的。
	3. Manifest 文件还可以用来指定应用程序的元数据（如它的图标，版本号或者主题）以及额外的顶层节点，这些节点可以用来指定必需的安全权限和单元测试，以及定义硬件，屏幕和平台支持要求。
	4. Manifest 文件由一个根manifest标签构成，该标签带有一个被设为项目包的package属性。它通常包含一个xmlns:android属性来提供文件内使用的某些系统属性。
	5. 使用versionCode属性可将当前的应用程序版本号定义为一个整数，每次版本迭代时，这个数字都会增加。使用versionName可以定义一个显示给用户的公共版本号。
	6. 通过使用installLocaton属性，这可以指定是否允许将应用程序安装到外部存储器而不是内部存储器上。为此可以将其指定为preferExternal或auto，使用前者时，只要有可能就会把应用程序安装到外部存储器上，后者则要求系统决定。
	7. application节点： 
		一个Manifest只能包含一个application节点。它使用各种属性来指定应用程序的各种元数据(包括标题，图标和主题）。在开发时，应该包含一个设置为true的debuggable属性以启用调试，但是在发布时可以禁用该属性。
		application节点还可以作为一个包含了Activity，Service，Content Provider和Broadcast Receiver节点的容器，它包含的这些节点指定了应用程序的组件。使用android:name属性可以指定自定义Application类的名称。
	8. Activity 节点：
		应用程序内的每一个Activity都要求有一个activity标签，并使用android:name属性来指定Activity类的名称。必须包含核心的启动Activity和其他所有可以显示的Activity。启动任何一个没有在Manifest中定义的Activity时都会抛出一个运行时异常。每一个Activity节点都允许使用intent-filter子标签来定义用于启动该Activity的Intent。同样要注意，在指定Activity的类名时，可以使用句点号作为简写方式代替应用程序的包名。
	9. service 和activity标签一样，需要为应用程序中使用的每一个Service类添加一个service标签。service标签页支持使用intent-filter子标签来允许运行时迟邦定。
	10. provider 
		provider标签用来指定应用程序中的每一个Content Provider。 Content Provider用来管理数据库访问和共享。
	11. receiver 
		通过添加receiver标签，可以注册一个Broadcast Receiver，而不用事先启动应用程序。
		Broadcast Receiver就像全局事件监听器一样，一旦注册了之后，无论何时，只要与它相匹配的Intent被系统或应用程序广播出来，它就会立即执行。通过在Manifest中注册一个Broadcast Receiver，可以使这个进程实现完全自治。如果一个匹配的Intent被广播了，则应用程序就会自动启动，并且你注册的Broadcast Receiver也会开始运行。
		每个receiver节点都被允许使用intent-filter子标签来定义可以用来触发接收器的Intent。

3. 分离资源
	1. 把非代码资源(如图片和字符串常量)和代码分离开来始终是一种好的做法。Android支持各种资源与代码的分离，从简单的字符串和颜色这样的值到更复杂的资源，例如图片（drawable)，动画，主题和菜单。
	2. 也许可以分离的最复杂的资源就是布局。
	3. 通过将资源分离开来，可以使它们更容易维护，更新和管理。这也可以让你轻松地定义多种可选的资源值来支持国际化需求，以及包含不同的资源来支持硬件的变化，特别是屏幕尺寸和分辨率的变化。
	4. 应用程序资源存储在项目层次中的res文件夹下。在整个文件夹中，每一种可用的资源类型都存储在各自的子文件夹中。
	5. 每种资源类型存储在不同的文件夹中，这些资源类型分别是：简单的值，Drawable，颜色，布局，动画，样式，菜单，XML，文件（包括searchable）和原始资源。
	6. 当构建应用程序的时候，这些资源会被尽可能高效地编译和压缩，并包含到应用程序包中。这个过程中还创建一个R类文件，它包含了对加入到该项目中的每一个资源的引用，因此可以在代码中引用资源，其优势在于可以在设计时检查语法。
	7. 在所有的情况下，资源文件名都应该只包含小写字母，数字，点(.)和下划线(_)。



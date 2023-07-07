在 WPF (Windows Presentation Foundation) 中，AppDomain.CurrentDomain.UnhandledException 和 DispatcherUnhandledException 都是用于处理未捕获的异常的事件。

AppDomain.CurrentDomain.UnhandledException 事件是在应用程序域中的任何线程上引发的未捕获异常时触发的。当应用程序中的线程未能捕获并处理异常时，该事件提供了一个机会来执行全局异常处理逻辑。但请注意，该事件并不保证应用程序能够继续运行，因为异常可能已经导致应用程序的不可恢复状态。

DispatcherUnhandledException 事件是在应用程序的主 UI 线程上引发的未捕获异常时触发的。WPF 应用程序的 UI 线程是单线程的，该事件提供了一个机会来捕获并处理在 UI 线程上抛出的异常。通过处理这个事件，可以防止未处理的异常导致应用程序崩溃，并采取适当的措施来处理异常情况，例如记录错误信息、向用户显示错误消息等。

需要注意的是，AppDomain.CurrentDomain.UnhandledException 和 DispatcherUnhandledException 事件处理程序应该尽量只用于处理无法预料的异常，并在处理程序中执行最小的操作。如果能够预料到特定的异常情况，最好使用 try-catch 块来显式处理异常，而不依赖于这些全局异常处理事件。

# 属性更改通知功能的实现通常涉及以下几个主要步骤：

定义属性：在类中定义属性，通常具有公共的 get 和 set 方法，用于获取和设置属性的值。

触发属性更改事件：在属性的 set 方法中，比较新旧值，如果发生变化，则触发属性更改事件。

声明属性更改事件：在类中声明一个事件，通常命名为 PropertyChanged，它是一个 PropertyChangedEventHandler 类型的事件。

实现 INotifyPropertyChanged 接口：类需要显式地实现 INotifyPropertyChanged 接口，以指示它具有属性更改通知功能。这个接口定义了一个 PropertyChanged 事件。

事件触发：当属性的值发生变化时，在属性的 set 方法中触发 PropertyChanged 事件，并传递属性名作为参数。这将通知订阅该事件的其他对象属性的更改。

事件订阅和处理：其他对象可以订阅 PropertyChanged 事件，以便在属性更改时接收通知。当事件被触发时，订阅者可以执行相应的操作，例如更新界面或执行其他逻辑。

整个过程涉及属性的定义、比较、事件的触发和订阅者的处理。通过实现 INotifyPropertyChanged 接口，并在属性更改时触发相应的事件，可以实现属性更改通知的功能，以便其他对象能够感知属性的变化并作出相应的响应。

```
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="WPF App" Height="350" Width="500">
    <Grid>
        <TextBox Text="{Binding Name, Mode=TwoWay}" />
        <TextBlock Text="{Binding Name}" Margin="0 20" />
    </Grid>
</Window>

```
DataContext 存在可以不用在xaml 中定义bing 省略 ElementName=window,
like
```
<TextBox Text="{Binding PersonName,  ElementName=window,Mode=TwoWay}"
                 Margin="0 2"
                 Grid.Row="1" Grid.Column="2"/>
```
```
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        Person person = new Person();
        person.Name = "John Doe";

        DataContext = this;
    }
}
```


# WPF app 初始化顺序
在 WPF 中，资源的初始化是在界面初始化之前发生的。

当一个 WPF 应用程序启动时，它会首先加载并初始化应用程序级别的资源。这些资源定义在 <Application.Resources> 元素中，通常位于 App.xaml 文件中。这些资源可以包括样式、模板、数据、转换器、资源字典等。

在资源初始化完成后，WPF 框架会继续初始化应用程序的界面。这包括创建和显示窗口（Window）以及加载窗口中的 XAML 文件和相关资源。

在界面的 XAML 文件中，你可以使用资源字典（ResourceDictionary）来定义和引用各种资源。这些资源可以是应用程序级别的资源，也可以是特定窗口或控件级别的资源。在解析和加载 XAML 文件时，WPF 框架会先处理和注册资源，然后再创建和渲染界面元素。

所以，总结起来，在 WPF 中，资源的初始化发生在界面初始化之前。资源的初始化通过应用程序级别的资源和界面所引用的资源字典来定义和管理。这样可以确保在界面元素创建之前，所有需要的资源已经可用，并能够在界面中正确应用。

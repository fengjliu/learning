1. all desktop app framework has one GUI class which to be used as the top layer object. 
   when created.it will
   it will instantize all needed objects including GUI objects
   create all services such as IO.file. task. serilize/deserisze
   created data layer
   implement foundation of event/function binding
    for example:   QT and MFC
2. all desktop framework should provide a library of UI controls,including layout, edit, control..
   all GUI object has a hierarchy of objects like tree
3. top layer GUI object always need to be created and show by one application object in main.cpp
   main.cpp：

创建 QApplication 对象，用于管理应用程序的控制流和主设置。
创建 MainWindow 对象，并显示它。
调用 app.exec() 进入应用程序的事件循环。

4. 所有桌面app 框架的项目文件都有相似的智能： 组织项目文件系统，定义编译器和链接器， 定义editor 属性， 定义其他代码文件加工流水线的操作
5. 
   
   

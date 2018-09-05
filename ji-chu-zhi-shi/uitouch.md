## UITouch

当用户一根手指触摸屏幕时，会创建一个与手指相关联的UITouch对象
一根手指对应一个UITouch对象。
UITouch的作用
    - 保存着跟手指相关的信息，比如触摸的位置、时间、阶段。
    - 当手指移动时，系统会更新同一步UITouch对象，使之能够一直保存该手指在的触摸位置。
    - 当手指离开屏幕时，系统会销毁相应的UITouch对象。
事件传递示例
    - 触摸事件的传递是从父控件传递到子控件。
    ![](/assets/Snip20180905_125.png)
    点击了orange的View:UIApplication -> UIWindow -> 蓝色 -> orange
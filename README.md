# Android.Four-components-notes
Android之四大组件进入深入原理~  

## 开发艺术探索之四大组件的工作过程   
> 加深对四大组件工作方式的认识，有助于加深对Android整体的体系结构的认识。很多情况下，只有对Android的体系结构有一定认识，在实际开发中才能写出优秀的代码。  

### 1. 四大组件的运行状态
> Android的四大组件除了BroadcastReceiver以外，都需要在AndroidManifest文件注册，BroadcastReceiver可以通过代码注册。调用方式上，除了ContentProvider以外的三种组件都需要借助intent

* Activity
> 是一种展示型组件，用于向用户直接地展示一个界面，并且可以接收用户的输入信息从而进行交互，扮演的是一个前台界面的角色。Activity的启动由intent触发，有隐式和显式两种方式。一个Activity可以有特定的启动模式，finish方法结束Activity运行

* Service
> 是一种计算型组件，在后台执行一系列计算任务。它本身还是运行在主线程主线程主线程中的，所以耗时的逻辑仍需要单独的线程去完成。Activity只有一种状态：启动状态。而service有两种：启动状态和绑定状态。当service处于绑定绑定绑定状态时，外界可以很方便的和service进行通信，而在启动状态中是不可不可不可与外界通信的。Service可以停止, 需要灵活采用stopService和unBindService

* BroadcastReceiver
> 是一种消息型组件，用于在不同的组件乃至不同的应用之间传递消息    
> * 静态注册--不需要启动      
  在清单文件中进行注册广播, 这种广播在应用安装时会被系统解析, 此种形式的广播不需要应用启动就可以接收到相应的广播
> * 动态注册--需要启动    
  需要通过Context.registerReceiver()来实现 并且要自定义好<intent-filter>, 并在不需要的时候通过Context.unRegisterReceiver()来解除广播. 此种形态的广播要应用启动才能注册和接收广播. 在实际开发中通过Context的一系列的send方法来发送广播, 被发送的广播会被系统发送给感兴趣的广播接收者, 发送和接收的过程的匹配是通过广播接收者的<intent-filter>来描述的.但广播不适合来做耗时操作
  
* ContentProvider
> 是一种提供给别人CURD的数据共享型组件，用于向其他组件乃至其他应用共享数据。在它内部维持着一份数据集合, 这个数据集合既可以通过数据库来实现, 也可以采用其他任何类型来实现, 例如list或者map. ContentProvider对数据集合的具体实现并没有任何要求.要注意处理好内部的insert, delete, update, query方法的线程同步, 因为这几个方法是在Binder线程池被调用   

### 2. Activity的工作过程

![图片](https://hujiaweibujidao.github.io/images/androidart_activity.png)



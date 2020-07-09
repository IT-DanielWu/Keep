#### 说下Activity的生命周期？
| 生命周期 | 作用  |
| :------- | :---- |
| onCreate() | 表示Activity 正在创建，常做初始化工作，如setContentView界面资源、初始化数据|
| onStart() | 表示Activity 正在启动，这时Activity 可见但不在前台，无法和用户交互 |
| onResume() | 表示Activity 获得焦点，此时Activity 可见且在前台并开始活动|
| onPause() | 表示Activity 正在停止，可做 数据存储、停止动画等操作 |
| onStop() | 表示activity 即将停止，可做稍微重量级回收工作，如取消网络连接、注销广播接收器等|
| onDestroy() | 表示Activity 即将销毁，常做回收工作、资源释放 |
| onRestart() | 表示当Activity由后台切换到前台，由不可见到可见时会调用，表示Activity 重新启动 |

---

#### 屏幕旋转时生命周期？
> 屏幕旋转时候，如果不做任何处理，activity会经过销毁到重建的过程, 一般这种效果都不是想要的(比如视频播放器就经常会涉及屏幕旋转场景)

###### 第一种情况：当前的Activity不销毁【设置Activity的android:configChanges="orientation|keyboardHidden|screenSize"时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法】
```
<activity
    android:name=".activity.VideoDetailActivity"
    android:configChanges="orientation|keyboardHidden|screenSize"
    android:screenOrientation="portrait"/>
```

执行该方法
```
//重写旋转时方法，不销毁activity
@Override
public void onConfigurationChanged(Configuration newConfig) {
	super.onConfigurationChanged(newConfig);
}
```

##### 第二种情况：销毁当前的Activity后重建，这种也尽量避免。
> 不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，默认首先销毁当前activity,然后重新加载

---

#### 异常条件会调用什么方法？
* 当非人为终止Activity时，比如系统配置发生改变时导致Activity被杀死并重新创建、资源内存不足导致低优先级的Activity被杀死，会调用 onSavaInstanceState() 来保存状态。
该方法调用在onStop之前，但和onPause没有时序关系。
* onSaveInstanceState()与onPause()的区别: onSaveInstanceState()适用于对临时性状态的保存，而onPause()适用于对数据的持久化保存。
当异常崩溃后App又重启了，这个时候会走onRestoreInstanceState()方法，可以在该方法中取出onSaveInstanceState()保存的状态数据。

---
#### 什么时候会引起异常生命周期
资源相关的系统配置发生改变或者资源不足：例如屏幕旋转，当前Activity会销毁，并且在onStop之前回调onSaveInstanceState保存数据，在重新创建Activity的时候在onStart之后回调onRestoreInstanceState。其中Bundle数据会传到onCreate（不一定有数据）和onRestoreInstanceState（一定有数据）。
防止屏幕旋转的时候重建，在清单文件中添加配置：android:configChanges="orientation"

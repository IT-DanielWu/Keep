#### 如何避免配置改变时Activity重建？
> 为了避免由于配置改变导致Activity重建，可在AndroidManifest.xml中对应的Activity中设置android:configChanges="orientation|screenSize"。</br>
此时再次旋转屏幕时，该Activity不会被系统杀死和重建。只会调用onConfigurationChanged。</br>
因此，当配置程序需要响应配置改变，指定configChanges属性，重写onConfigurationChanged方法即可。

使用场景，比如视频播放器横竖屏切换播放视频，就需要设置这种属性。

#### 优先级低的Activity在内存不足被回收后怎样做可以恢复到销毁前状态？
> 优先级低的Activity在内存不足被回收后重新打开会引发Activity重建。</br>
Activity被重新创建时会调用onRestoreInstanceState（该方法在onStart之后），并将onSavaInstanceState保存的Bundle对象作为参数传到onRestoreInstanceState与onCreate方法。</br>
因此可通过onRestoreInstanceState(Bundle savedInstanceState)和onCreate((Bundle savedInstanceState)来判断Activity是否被重建，并取出数据进行恢复。</br>
但需要注意的是，在onCreate取出数据时一定要先判断savedInstanceState是否为空。

#### 如何判断activity的优先级？
> 除了在栈顶的activity,其他的activity都有可能在内存不足的时候被系统回收，一个activity越处于栈底，被回收的可能性越大。</br>
如果有多个后台进程，在选择杀死的目标时，采用最近最少使用算法（LRU）。

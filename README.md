# Android_In_Deep

  总结了一些在Android开发中关于内存管理和优化的方法。
  
# 前言
  
   在Android开发中大多数是使用Java的api，由于程序要在移动设备上运行，对于流畅度、性能和内存使用很苛刻。于是，Google提供了一套专门在android上开发的api，减少过多的内存消耗从而提高性能，所以建议在开发时尽量使用Google提供的api。
  
## 数组与列表的选择

*数组*<br>
  * 占用连续的内存空间
  * 实例后长度固定无法改变
  * 按索引查询速度较快
  * 插入和删除操作相对较慢

*列表*<br>
  * 占用的内存空间是散乱型
  * 长度可增长
  * 插入和删错操作速度较快
  * 查询速度较慢，需要一个个遍历去查找
  
  _总结：_<br>
1、在内存短缺的情况下，数组相比于列表表现得相对差一些;<br>
2、数组的查询速度比列表快;<br>
3、数组长度固定，而列表长度不固定灵活性更强;<br>
4、数组比较适合查询操作，插入操作比较麻烦;<br>
5、列表基于链表，在插入数据方面比数组灵活；<br>

## HashMap与ArrayMap

*HashMap*<br>
  * 散列型链表
  * 根据key计算出hash值来指定value的存放位置
  * 为了尽量避免hash冲突而创建一个较大的数组
  * hash值相同时以链表的形式存储

*ArrayMap*<br>
  * 使用两个小数组存储数据
  * 占用连续的内存空间
  * 功能上跟HashMap差不多
  
   _总结：_<br>
 1、ArrayMap的插入和删除操作稍微比HashMap慢一些，但是在数据量1000以内，可以忽略不计;<br>
 2、ArrayMap使用两个较小的数组，而HashMap需要建立一个较大的数组，所以在内存短缺情况下或优化性能下，建议使用ArrayMap;<br>
 
## 数据结构的建议

Google为Android量身定制的Android.util，包含SparseArray系列；<br>
尽量使用以下类替换使用HashMap（数据量在1000以内，性能跟HashMap差不多，但HashMap占用内存较大）<br>
  * SparseArray
  * SparseBooleanArray
  * SparseIntArray
  * SparseLongArray
  * LongSparseArray
  * ArrayMap
  * ArraySet

## Context上下文的使用

Context使用不切当，很容易发生内存泄漏。<br>
Context创建的个数是等于Activity的数量加1，即 Sum(Context) = Sum(Activity) + 1;<br>
Activity的Context被一个生命周期更长的对象持有，一旦Activity被销毁了，因为Context被长生命周期对象持有无法释放，所以无法被销毁，保留在内存中；<br>
Application的Context是伴随着整个应用而存在的，它的生命周期算是最长的，一旦应用退出了运行线程，这个Context与跟它联系的对象也会被释放掉；

1、跟UI相关的Context，使用由Activity提供的Context;<br>
2、除了UI相关的，建议使用由Application提供的Context;<br>

## 对象引用

四大引用：<br>
1、强引用（StrongRefernce）<br>
2、软引用（SoftReference）<br>
3、弱引用（WeakReference）<br>
4、虚引用（PhantomReference）<br>

我们多数的定义对象都是强引用，没有指定对象的引用类型时，它就默认是强引用；<br>
  
Activity发生内存泄漏的优化建议：<br>
一般Activity发生内存泄漏的原因是它被一个比它更长生命周期的对象引用而在销毁时无法被系统GC回收，我们可以使用弱引用（WeakReference）保存Activity的引用,在Activity销毁时在onDestroy方法中清空引用;如果遇到系统崩溃或忘记清理，当Activity销毁后，系统会扫描内存区域直到把它回收；<br>
```
//保存Activity
WeakReference<Activity> weakReference = new WeakReference<>(activity);

//获取Activity
Activity activity = weakReference.get();

//清理引用
weakReference.clear();
weakReference = null;
activity = null;
```


































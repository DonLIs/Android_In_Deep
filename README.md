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

## Context上下文的使用

Context使用不切当，很容易发生内存泄漏。<br>
Context创建的个数是等于Activity的数量加1，即 Sum(Context) = Sum(Activity) + 1;<br>
Activity的Context被一个生命周期更长的对象持有，一旦Activity被销毁了，因为Context被长生命周期对象持有无法释放，所以无法被销毁，保留在内存中；<br>
Application的Context是伴随着整个应用而存在的，它的生命周期算是最长的，一旦应用退出了运行线程，这个Context与跟它联系的对象也会被释放掉；

1、跟UI相关的Context，使用由Activity提供的Context;<br>
2、除了UI相关的，建议使用由Application提供的Context;<br>


































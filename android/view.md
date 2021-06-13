> A `View` usually draws something the user can see and interact with. Whereas a `ViewGroup` is an invisible container that defines the layout structure for `View` and other `ViewGroup` objects.

[Androiddev - Layout](https://developer.android.com/guide/topics/ui/declaring-layout)

`ViewGroup`包含`View`

`Layout`属于`ViewGroup`

`TextView`属于`View`

`ViewGroup`是`View`的容器，`ViewGroup`不可见，`View`可见。

![viewgroup](assets/viewgroup_2x-20210507163454743.png)



Android先读取`AndroidManifest.xml`，找到`activity`，`activity`中写有对应`layout`，找到对应`layout`然后渲染视图。
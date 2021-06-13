# calculator memory operation

mc m+ m- mr

## operators

mc:memory clear

m+:memory add

m-:memory minus

mr:memory recall

## usage

默认存储数值`m`为`0`
当对一个数进行记忆加减，将更新`m`。
更新规则为：
- 当对数值`a`使用`m+`时，`m`将加上`a`，并将结果更新到`m`；
- 当对数值`b`使用`m-`时，`m`将加上`-b`，并将结果更新到`m`。
使用`mr`查看当前`m`数值。

举例：
新打开的计算器，依次对对`11`使用`m+`，对`3`使用`m+`，对`13`使用`m-`
结果为`0+11+3+(-13)=1`
```xml
<fragment ...>
  	...
    <action 
            ...
            app:popUpTo="@id/title_screen"
            .../>
</fragment>
```

当这个`action`被触发后，这个回退栈将被修改。比如`action`表示`fragmentA`到`fragmentB`，当`action`被触发时，`fragmentA`进入`fragmentB`，默认情况下`Back`会使`fragmentB`退回到`fragmentA`，但由于设置了`app:popUpTo="@id/title_screen"`，那么`fragmentB`会退回到`title_screen`这个`fragment`。




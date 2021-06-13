# 抽象类

## 特点

- 抽象方法一定在抽象类中。
- 抽象方法和抽象类都必须被abstract关键字修饰。
- 抽象类不可以用new创建对象。因为调用抽象方法没意义。
- 抽象类中的抽象方法要被使用，必须由子类复写起所有的抽象方法后，建立子类对象调用。  
如果子类只覆盖了部分抽象方法，那么该子类还是一个抽象类。

## 提问

### 抽象类的存在意义

抽象类可以将设计和实现分离，你写你的抽象类，我写我的实现方法。

### 抽象类是否需要构造器

需要,供子类使用
# JPA

Java Persistence Api

所使用的方法是：**ORM**（Object Relational Mapping）

与JDBC与MyBatis相比，JPA可是使程序员几乎不用写SQL语句。

## 表与字段映射

实体类与表映射示例：

```java
@Data
@Entity
public class Article {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Column(nullable = false, length = 32)
    private String title;

    @Column(nullable = false, length = 32)
    private String author;

    private String content;

    private Date createTime;
}
```

### 表映射

`@Entity`表示这个实体类为表，不指定注解`name`参数的情况下，默认为类名。

提示：驼峰命名会转化为下划线命名。

### 字段映射

`@Entity`注解下的实体类的成员变量，默认为数据库字段；字段类型会根据成员变量类型推倒，其映射关系为：

| 成员变量类型   | 字段类型     |
| -------------- | ------------ |
| Integer        | int          |
| Long           | bigint       |
| String         | varchar(255) |
| java.util.Date | datetime(6)  |

由于数据库字段的复杂，有时候需要通过注解来完成映射。

通过传入`@Column`注解参数来可以设置`唯一索引`、`非空约束`等等；`@Id`指明主键，通常配合`@GeneratedValue`来实现主键自增。

此外还有一些指定数据库类型的注解，比如`@Temporal`可以指定时间，`@Lob`可以指定长文本类型。

## 表关系映射

表关系及其对应注解：

| 表关系 | 注解        |
| ------ | ----------- |
| 一对一 | @OneToOne   |
| 一对多 | @OneToMany  |
| 多对一 | @ManyToOne  |
| 多对多 | @ManyToMany |

`@xxxToOne`默认生成外键，`@xxxToMany`默认生成中间表。

`@JoinColumn`与`@JoinTable`放在关系映射注解下可以定义相关映射信息。一般`@xxxToOne`后只能放`@JoinColumn`，否则抛出Exception；`@xxxToMany`后可以放`@JoinTable`与`@JoinColumn`，但只有`@JoinTable`对中间表生效，`@JoinColumn`对任何表不产生作用。

- `@JoinColumn`用于指定外键相关信息，比如外键名称等；
- `@JoinTable`用于指定中间表相关信息，比如中间表名、中间表外键字段名称等；

由于表关系映射描述的的是两个表的关系，那么注解肯定是成对出现的，总共有三对：

- `@OneToOne`与`@OneToOne`
- `@OneTOMany`与`@ManyToOne`
- `@ManyToMany`与`@ManyToMany`

虽然是成对出现的，但是记录两者关系的只能有一个人，也是就说不能两个人都创建外键，或者两个人都个字创建一个中间表。当你确定两表之间的关系，并指派一个表来担此重任，那么你需要在另外一个表上指定`@xxxToxxx(mappedBy = "<该注解下成员变量所指向实体类的对应映射关系的成员变量名>")`，这样就视为其丧失其维持关系的能力。

`@ManyToOne`没有`mappedBy`参数，因为一旦`@ManyToOne`出现了`mappedBy`参数，就代表着它与好搭档`@OneToMany`配合使用，但是他两个组合注定了`@ManyToOne`用于创建外键，`@OneToMany`使用`mappedBy`参数来配合他。所以`@ManyToOne`没有`mappedBy`参数。

### 表名和字段名生成规则

以下皆为`@JoinColumn`和`@JoinTable`未指定名称的情况下。

#### 外键

`@JoinColumn注解下的成员变量名_@JoinColumn注解下的成员变量类型中指定称主键的成员变量名`

#### 中间表

表名：

`实体类名_@JoinTable下的成员变量名`

表中外键字段名：

`@JoinTable注解下的成员变量名_@JoinColumn注解下的成员变量类型中指定称主键的成员变量名`

`@xxxToxxx(mappedBy = "xxx")注解下的成员变量名_@xxxToxxx(mappedBy = "xxx")注解下的成员变量类型中指定称主键的成员变量名`

### `@OneToOne`与`@OneToOne`

采用外键方式，一个使用`mappedBy`放弃维护权利，一个持有维护权利。

### `@OneTOMany`与`@ManyToOne`

采用外键方式。

A对B的关系为一对多，即A中一条记录对应着B中多条记录；相应的B对A的关系自然而然就成为了多对一，即B中多条记录对应着A中一条记录。

通常处理这种问题的方法，用数据库语言描述就是在B表中创建A的外键；如果用ORM（对象关系映射，即用实体类映射表）怎么表示呢？

A实体类创建：

```java
@Data
@Entity
public class A {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    @OneToMany(mappedBy = "a")
    private List<B> bs;
}
```

B实体类创建：

```java
@Data
@Entity
public class B {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;
    @ManyToOne
    private A a;
}
```

创建出的表结构：
A含有`id`主键字段；B含有`id`主键字段和`int`类型的`a_id`字段。

### `@ManyToMany`与`@ManyToMany`

采用中间表的方式。

## 注解详解

PS：@GeneratedValue注解的strategy属性提供四种值：

- AUTO： 主键由程序控制，是默认选项，不设置即此项。

- IDENTITY：主键由数据库自动生成，即采用数据库ID自增长的方式，Oracle不支持这种方式。
- SEQUENCE：通过数据库的序列产生主键，通过@SequenceGenerator 注解指定序列名，mysql不支持这种方式。
- TABLE：通过特定的数据库表产生主键，使用该策略可以使应用更易于数据库移植。



As Forum has many topics, and a topic belongs to one and only Forum, you probably want to go with a Forum type attribute annotated with [`@ManyToOne`](http://docs.oracle.com/javaee/6/api/javax/persistence/ManyToOne.html)

```java
@ManyToOne
@JoinColumn(name = "forumId")
private Forum forum;
```

## Q&A

保留字导致的表无法创建问题

`desc`为保留字，会导致表无法创建。

可以通过添加注解进行转义解决这个问题。

```java
@Column(name = "\"desc\"")
```
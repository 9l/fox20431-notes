# Regular Expression

正则表达式

[wiki](https://en.wikipedia.org/wiki/egular_expression)

## Character classes

符号类型

```RegExp
[character partern]
```

## Quantifiers

量词:用于表达重复的次数

?:0或1
*:0以上
+:1以上
{n}:n
{min,}:大于min
{min,max}:大于min,小于max



## Greedy & Non-greedy

贪婪模式 和 非贪婪模式`?`

### 举例

```js
let str = 'a "witch" and her "broom" is one';
str.match( /".*"/g);
```

贪婪模式匹配`"witch" and her "broom"`

非贪婪模式匹配`"witch"`

## 总结

字符+数量+匹配模式 构成正则表达式
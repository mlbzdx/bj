# `mockjs`

> mockjs的官方文档 ： https://github.com/nuysoft/Mock/wiki

## 导入 mockjs

```
import Mock from 'mockjs';
```

导入`mockjs`之后，得到的Mock其实是一个对象，里面提供了或拦截数据或模拟各种数据的方法。

在Mock对象中，有一个mock方法，该方法可以传入一个符合语法规范的数据对象，该对象经该方法编译后会返回一个的虚假模拟数据。

在`Mock.mock()`方法中传入的数据对象需要符合语法规范，而这里的语法规范分为两种，数据模板规范（DTD）和数据占位符规范（DPD）。

## 数据模板规范



## 数据占位符规范

## mock方法

```
Mock.mock( rurl?, rtype?, template|function( options ) )
//rul 拦截的Ajaxs请求url地址
//rtype 拦截的Ajaxs请求类型
//tempalte 数据模板
//function(options) 执行后返回数据的函数。
options
指向本次请求的 Ajax 选项集，含有 url、type 和 body 三个属性
```

## setup方法

```
Mock.setup( settings )
// settings 为一个对象，对象中目前常见配置属性为timeout,设置拦截Ajaxs请求后数据的响应时间
```

## Random方法

Mock.Random 是一个工具类，用于生成各种随机数据。

**Mock.Random 的方法在数据模板中称为『占位符』，书写格式为 `@占位符(参数 [, 参数])` 。**

### 调用方法

```
var Random = Mock.Random
Random.email()
// => "n.clark@miller.io"
Mock.mock('@email')
// => "y.lee@lewis.org"
Mock.mock( { email: '@email' } )
// => { email: "v.lewis@hall.gov" }
```



### 占位符图表

Mock.Random 提供的完整方法（占位符）如下：

| Type          | Method                                                       |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | area, region                                                 |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |




# 摘要

主要有四种语言:Hamlet是HTML模板语言，Julius是JavaScript的模板，Cassius和Lucius都是CSS的模板。Hamlet和Cassius都是空白字符敏感的格式，使用缩进表示嵌套。相比之下，Lucius是CSS的超集，像CSS一样用大括号表示嵌套。Julius是一种简单的用于生成JavaScript的传递性(passthrough)语言;唯一增加的功能是变量插值。

> 注意: 事实上，Cassius只是Lucius的另一种语法。它们都使用相同的处理引擎，只是Cassius文件在处理前将缩进转换为大括号而已。两者之间的选择纯粹是一种语法偏好。

## Hamlet(HTML)
```html
$doctype 5
<html>
  <head>
    <title>#{pageTitle} - My Site
    <link rel=stylesheet href=@{Stylesheet}>
  <body>
    <h1 .page-title>#{pageTitle}
    <p>Here is a list of your friends:
    $if null friends
      <p>Sorry, I lied, you don't have any friends.
    $else
      <ul>
        $forall Friend name age <- friends
          <li>#{name} (#{age} years old)
    <footer>^{copyright}
```

## Lucius (CSS)
```css
section.blog {
    padding: 1em;
    border: 1px solid #000;
    h1 {
        color: #{headingColor};
        background-image: url(@{MyBackgroundR});
    }
}
```

## Cassius (CSS)
这等价于Lucius的例子:
```casius
section.blog
    padding: 1em
    border: 1px solid #000
    h1
        color: #{headingColor}
        background-image: url(@{MyBackgroundR})
```

## Julius (JavaScript)
```js
$(function(){
    $("section.#{sectionClass}").hide();
    $("#mybutton").click(function(){document.location = "@{SomeRouteR}";});
    ^{addBling}
});
```


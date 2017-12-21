---
layout: blog
istop: true
title: "Golang自动生成js(React＆Preact)DOM包"
background-image: https://user-images.githubusercontent.com/170299/33872480-b10be348-df49-11e7-80b1-06736c8298ae.png
date:  2017-08-1 00:36:14
category: golang
tags:
- golang
- React
- Preact
- Joy
---
# joy 官方介绍


在每个Chrome浏览器中,使用Go的类型系统生成简洁的``Javascript`` 建立大型的web 应用程序。

这句话好绕😅 算了我还是贴原文:

> Translate idiomatic Go into concise Javascript that works in every browser. Use Go's type system and world-class tooling to build large web applications with confidence.

介绍:

> Today I’m excited to share the Joy compiler with the developer community. Joy brings Go’s simple design and excellent tooling to the frontend. I believe Joy will become the most productive way to build large-scale, maintainable web applications. I hope you’ll find Joy delightful as I do.

- 首先安装 Joy 
- 
```
curl -sfL https://raw.githubusercontent.com/matthewmueller/joy/master/install.sh | sh
```
- 不过我还是喜欢用``go get``
```
go get -u -v github.com/matthewmueller/joy/... 从源代码安装编译器

go test -v 运行所有的测试
```
# 官方全部例子
访问https://mat.tm/joy/#examples或仔细阅读测试栗子。

编译进入``Javascript``：
```
joy <main.go>
```
编译并运行无头Chrome浏览器中的Go代码：
```
joy run <main.go>
```

构建代码的开发版本：
```
joy build --dev <main.go>...
```

构建代码的生产版本（即将推出！）：
```
joy build <main.go>...
```

使用``livereload``启动开发服务器：
```
joy serve <main.go>...
```

运行``joy help``其他细节。

- 使用golang生成JavaScript栗子
- 
```golang
package main

import (
  "github.com/matthewmueller/joy/dom/htmlbodyelement"
  "github.com/matthewmueller/joy/dom/window"
)

func main() {
  document := window.New().Document()

  a := document.CreateElement("a")
  println(a.NodeName())
  strong := document.CreateElement("strong")
  println(document.CreateElement("strong").OuterHTML())
  a.AppendChild(strong)

  strong.SetTextContent("hi world!")

  body := document.Body().(*htmlbodyelement.HTMLBodyElement)
  body.AppendChild(a)
  println(document.Body().OuterHTML())
}
```
- 生成的JavaScript代码如下
```
(function() {
  var pkg = {};
  pkg["52-basic-dom"] = (function() {
    function main () {
      var document = window.document;
      var a = document.createElement("a");
      console.log(a.nodeName);
      var strong = document.createElement("strong");
      console.log(document.createElement("strong").outerHTML);
      a.appendChild(strong);
      strong.textContent = "hi world!";
      var body = document.body;
      body.appendChild(a);
      console.log(document.body.outerHTML)
    };
    return {
      main: main
    };
  })();
  return pkg["52-basic-dom"].main();
})()
```

链接和提示：

我一直在使用这个来弄清楚如何构建JS树``https://astexplorer.net：``
简单的Go AST浏览器``http://goast.yuroyoro.net/``
ES3 AST格式这是在syntax.go``https://github.com/estree/estree/blob/master/es5.md``
需要参考这个来看看Go的AST中可能的类型``https://golang.org/ref/spec``
运行所有测试： ``go test -v``
运行个人测试：`` go test -v -run Test/08``
``pretty.Println(ast)``会漂亮的打印``JS AST``（需要这个包）
``ast.Print(nil, node)``会很漂亮的打印``Go AST``
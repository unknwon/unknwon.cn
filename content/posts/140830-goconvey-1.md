---
title: goconvey - 课时 1：优雅的单元测试
categories: ["教程"]
tags: ["go", "goconvey"]
date: 2014-08-30
lastmod: 2020-04-16
---

**注意事项**

本博客隶属于 [goconvey - 课时 1：优雅的单元测试](https://github.com/Unknwon/go-rock-libraries-showcases/tree/master/lectures/03-goconvey#%E8%AF%BE%E6%97%B6-1%E4%BC%98%E9%9B%85%E7%9A%84%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95) 请注意配套使用。

本博文为 [goconvey](https://github.com/smartystreets/goconvey) - Go 语言单元测试包的配套博客，旨在通过文字结合代码示例对该库的使用方法和案例进行讲解，便于各位同学更好地使用和深入了解。

## 库简介

Go 语言虽然自带单元测试功能，在 GoConvey 诞生之前也出现了许多第三方辅助库。但没有一个辅助库能够像 GoConvey 这样优雅地书写代码的单元测试，简洁的语法和舒适的界面能够让一个不爱书写单元测试的开发人员从此爱上单元测试。

## 下载安装

您可以通过以下方式下载安装 GoConvey：

```sh
go get github.com/smartystreets/goconvey
```

###  API 文档

请移步 [Go Walker](http://gowalker.org/github.com/smartystreets/goconvey)。

## 基本使用方法

[示例代码](https://github.com/Unknwon/go-rock-libraries-showcases/tree/master/lectures/03-goconvey/class1/sample)

### 书写代码

下面是一个能够实现整数基本四则运算（加、减、乘、除）的代码：

```go
package goconvey

import (
	"errors"
)

func Add(a, b int) int {
	return a + b
}

func Subtract(a, b int) int {
	return a - b
}

func Multiply(a, b int) int {
	return a * b
}

func Division(a, b int) (int, error) {
	if b == 0 {
		return 0, errors.New("被除数不能为 0")
	}
	return a / b, nil
}
```

在上面的代码中，我们实现了 4 个函数，因此需要为这 4 个函数分别书写单元测试：

```go
package goconvey

import (
	"testing"

	. "github.com/smartystreets/goconvey/convey"
)

func TestAdd(t *testing.T) {
	Convey("将两数相加", t, func() {
		So(Add(1, 2), ShouldEqual, 3)
	})
}

func TestSubtract(t *testing.T) {
	Convey("将两数相减", t, func() {
		So(Subtract(1, 2), ShouldEqual, -1)
	})
}

func TestMultiply(t *testing.T) {
	Convey("将两数相乘", t, func() {
		So(Multiply(3, 2), ShouldEqual, 6)
	})
}

func TestDivision(t *testing.T) {
	Convey("将两数相除", t, func() {

		Convey("除以非 0 数", func() {
			num, err := Division(10, 2)
			So(err, ShouldBeNil)
			So(num, ShouldEqual, 5)
		})

		Convey("除以 0", func() {
			_, err := Division(10, 0)
			So(err, ShouldNotBeNil)
		})
	})
}
```

首先，您需要使用官方推荐的方式导入 GoConvey 的辅助包以减少冗余的代码：`. "github.com/smartystreets/goconvey/convey"`。

每个单元测试的名称需要以 `Test` 开头，例如：`TestAdd`，并需要接受一个类型为 `*testing.T` 的参数。

使用 GoConvey 书写单元测试，每个测试用例需要使用 `Convey` 函数包裹起来。它接受的第一个参数为 string 类型的描述；第二个参数一般为 `*testing.T`，即本例中的变量 t；第三个参数为不接收任何参数也不返回任何值的函数（习惯以闭包的形式书写）。

`Convey` 语句同样可以无限嵌套，以体现各个测试用例之间的关系，例如 `TestDivision` 函数就采用了嵌套的方式体现它们之间的关系。需要注意的是，只有最外层的 `Convey` 需要传入变量 t，内层的嵌套均不需要传入。

最后，需要使用 `So` 语句来对条件进行判断。在本例中，我们只使用了 3 个不同类型的条件判断：`ShouldBeNil`、`ShouldEqual` 和 `ShouldNotBeNil`，分别表示值应该为 nil、值应该相等和值不应该为 nil。有关详细的条件列表，可以参见 [官方文档](https://github.com/smartystreets/goconvey/wiki/Assertions)。

### 运行测试

现在，您可以打开命令行，然后输入 `go test -v` 来进行测试。由于 GoConvey 兼容 Go 原生的单元测试，因此我们可以直接使用 Go 的命令来执行测试。

以下便是命令行的输出（Mac）：

```sh
=== RUN TestAdd

  将两数相加 ✔


1 assertion thus far

--- PASS: TestAdd (0.00 seconds)
=== RUN TestSubtract

  将两数相减 ✔


2 assertions thus far

--- PASS: TestSubtract (0.00 seconds)
=== RUN TestMultiply

  将两数相乘 ✔


3 assertions thus far

--- PASS: TestMultiply (0.00 seconds)
=== RUN TestDivision

  将两数相除
    除以非 0 数 ✔✔
    除以 0 ✔


6 assertions thus far

--- PASS: TestDivision (0.00 seconds)
PASS
ok  	github.com/Unknwon/go-rock-libraries-showcases/lectures/03-goconvey/class1/sample/goconvey	0.009s
```

我们可以看到，输出结果调理非常清晰，单元测试的代码写起来也非常优雅。那么，这就是全部吗？当然不是。GoConvey 不仅支持在命令行进行人工调用调试命令，还有非常舒适的 Web 界面提供给开发者来进行自动化的编译测试工作。

### Web 界面

想要使用 GoConvey 的 Web 界面特性，需要在相应目录下执行 `goconvey`（需使用 `go get` 安装到 `$GOPATH/bin` 目录下），然后打开浏览器，访问 http://localhost:8080 ，就可以看到下以下界面：

<img width="1616" alt="0009_web_ui" src="https://cloud.githubusercontent.com/assets/2946214/20509383/6119e896-b036-11e6-9302-92576d11e412.png">

在 Web 界面中，您可以设置界面主题，查看完整的测试结果，使用浏览器提醒等实用功能。

其它功能：

- 自动检测代码变动并编译测试
- 半自动化书写测试用例：http://localhost:8080/composer.html
- 查看测试覆盖率：http://localhost:8080/reports/
- 临时屏蔽某个包的编译测试

## 小结

到这里，大家应该对如何使用 GoConvey 包来书写测试用例有了基本的了解，并且也见识到了它是如何让书写测试用例这项枯燥的工作变得优雅，并让人充满乐趣。

# Swift Basics 01 - Basic Type

## 整型

Swift提供了有符号的`Int8`，`Int16`，`Int32`，`Int64`，与无符号的`UInt8`，`UInt16`，`UInt32`，`UInt64`共8种类型。类型的`min`属性与`max`属性记录了该类型的最小最大值。

Swift另提供了`Int` 与`UInt`两个类型。在32位机上，等同于`Int32`和`UInt32`；在64位机上，等同于`Int64`和`UInt64`。

> 除非确切要用到一定范围的数据，否则默认用Int，有助于提高代码的consistency与interoperability。

> 非必要情况下不要使用UInt，用Int，理由同上。

## 浮点型

Swift提供了32位浮点类型`Float`，和64位浮点类型`Double`。

> Float可以精确到小数点后6位，Double可以精确到小数点后15位。

> 两种类型都适用的场景下，优先使用Double。

> Swift的类型推断机制下，整型默认为Int，浮点型默认为Double。

## 布尔型

```swift
let a = true
print(type(of: a))		// Bool
```

> 非布尔型变量用于布尔判断会报编译错误

## 字符与字符串

### 初始化

字符使用`Character` 类型，字符串使用`String`类型。字符与字符串全部使用双引号初始化，在没有指定具体类型时，单个字符默认为`String`：

```swift
let a = "a"
let b: Character = "b"

print(type(of: a))		// String
print(type(of: b))		// Character
```

> `String`为值类型。

使用加号可直接组合多个`String`，使用`append()`方法在`String`末尾添加一个字符：

```swift
let string1 = "hello"
let string2 = " there"
var string3 = string1 + string2		// "hello there"

let ch: Character = "!"
string3.append(ch)		// "hello there!"
```

使用`\(expression)`的形式在字符串中插入内容：

```swift
let a = 32
let b = "a is \(a)"								// "a is 32"
let c = "3 times a is \(3 * a)"		// "3 times a is 96"
```

### 字符串索引

因为存在Unicode字符等复杂情况，不可以单纯的使用整型值用于字符串的下标索引，要使用`String`的关联类型`String.Index`。

所有`String`值都有两个`String.Index`类型的属性：`startIndex`与`endIndex`。`startindex`指向`String`值**第一个字符的位置**，`endIndex`指向**最后一个字符的后一个位置**（并非最后一个字符的位置），类似`C++ STL`中枚举器的`begin()`方法与`end()`方法。

> 空字符串的`startIndex`与`endIndex`相等。

在`startIndex`与`endIndex`的基础上，使用以下三个方法获取其余位置的索引：

- `func index(before i: String.Index) -> String.Index`
- `func index(after i: String.Index) -> String.Index`
- `func index(_ i: String.Index, offsetBy n: String.IndexDistance) -> String.Index`

```swift
let str = "leap of faith"

str[str.startIndex]		// l
str[str.index(before: ss.endIndex)]		// h
str[str.index(after: ss.startIndex)]	// e

let index = str.index(str.startIndex, offsetBy: 8)
str[index]		// f
```

字符串的`indices`属性包含该字符串的所有字符的索引：

```swift
for index in str.indices {
  print("\(str[index]) ", terminator: "")		// l e a p   o f   f a i t h 
}
```

> 访问非法位置的索引会产生运行时错误。
>
> 索引属性可用于所有遵守`Collection`协议的集合类型。

## 元组

随意组合已知类型为一个元组（tuple），可以使用下标取元组中的值：

```swift
let http404Error = (404, "Not Found")

print(type(of: http404Error))										// (Int, String)
print("the status code is \(http404Error.0)")		// the status code is 404
```

可以将元组分解为多个变量，使用"_"跳过不需要的变量：

```swift
let (statusCode, statusMessage) = http404Error
print("the status code is \(statusCode)")				// the status code is 404

let (_, statusMessage) = http404Error
print("the status message is \(statusMessage)")	// the status message is Not Found
```

也可以在初始化元组时直接定义变量名：

```swift
let http200Status = (statusCode: 200, description: "OK")
print("the status code is \(http200Status.statusCode)")		// the status code is 200
```

## 类型转换

使用类型的初始化方法`SomeType(ofInitialValue)`进行转换。

```swift
let a: Int8 = 1
let b: Int64 = Int64(a)
let c = Double(a)
```

> 如果需要转换初始化方法不支持的参数类型，使用Extensions自定义初始化方法实现。

## 类型别名

```swift
typealias AudioSample = UInt16
var m = AudioSample.min			// m = 0
```


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


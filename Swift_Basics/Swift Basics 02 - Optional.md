# Swift Basics 02 - Optional

## Optional

> You use optionals in situations where a value may be absent.

```swift
let a = "123"
let b = Int(a)
print(type(of: b))		// Optional<Int>
```

上述代码中，常量`a`在Swift类型推断机制下判定为`String`类型。常量`b`通过初始化方法将字符串类型转化为整型。但是，并不是所有的字符串类型都可以转为整型，因此该方法不能保证一定会得到一个合理的结果。

这种不能保证返回合理结果的情况，对于引用类型，如自定义的类，在OC里会通过返回`nil`来解决：

```objective-c
// OC
- (someClass *)func:(NSString *)input {
  // if 可以进行合理操作 
  	// 合理操作
  	// 返回结果
  // else 不能进行合理操作
  	// 返回nil
}
```

但是对于基础类型，并没有一个类似于`nil`的空类型来完成相同的逻辑，我们一般指明一个值为不合理标记返回，如在找不到一个元素在数组中的下标时，就返回`NSNotFound`，`NSNotFound`实际上是一个大整数。

Swift则通过`Optional`类型来更方便的解决这类问题。如上面代码中将`String`类型的常量`a`转化为`Int`的初始化方法，返回的常量`b`为`Optional<Int>`类型。`Optional<Int>`代表常量`b`**可能**为一个`Int`类型的值，也**可能**为一个无效的空值`nil`。

通常情况下，类型的`Optional`类型写作类型名加问号的形式，如`Optional<Int>`写作`Int?`。

## nil

不同于OC中的`nil`，Swift中的`nil`可以用于所有数据类型，包括基础类型。

`nil`只可赋值给`Optional`类型：

```swift
var num: Int? = 64
num = nil
var t: Int = nil		// error: 'nil' cannot initialize specified type 'Int'
```

> 对于没有被初始化的`Optional`类型变量，会自动被赋值为`nil`。

## 从Optional数据中获取值

### Force Unwrapping

对于一个非空的`Optional`数据，可以使用感叹号强制拆箱（unwrap）为普通类型数据：

```swift
var num: Int? = 7
let realNum = num!
print(type(of: realNum), realNum)	// Int 7
```

但如果一个`Optional`数据为空值，那么我们强制unwrap会触发**运行时错误**，因此，当我们要拆箱一个`Optional`数据时，**务必**保证其不为空值，如使用`if`语句进行判断：

```swift
var num: Int? = 7

if num != nil {
    let realNum = num!
    print(type(of: realNum), realNum)
}
```

### Optional Binding

除去强制拆箱，我们可以在`if`语句中直接使用`let/var dataName = someOptional`的形式尝试将一个`Optional`数据转化为一个临时数据：

```swift
var num: Int? = 7

if let realNum = num {
  print(type(of: realNum), realNum)	// Int 7
} else {
  // can't unwrap
}
```

在上述代码中，如果`num`不为空，那么就可以成功生成一个`realNum`临时常量，该常量的作用域只限定在`if`段中。如果`num`为空，那么系统会走`else`或直接跳过`if`段，不会产生任何错误。

`Optional Binding`可以在`if`语句中多次使用，也可以和布尔语句一同使用，使用逗号分隔：

```swift
var num: Int? = 7

if let realNum = num, var otherNum = Int("32"), otherNum <= 33 {
    print(type(of: realNum), realNum)		// Int 7

    otherNum += 1
    print(type(of: otherNum), otherNum)		// Int 32
} else {
  // any optional bindings are nil, or any boolean condition evaluates to false
}
```

### Implicitly Unwrapped Optionals

使用普通类型名加感叹号定义的数据，也是`Optional`类型，不过可以在需要时隐式强制拆箱：

```swift
var num: Int! = 7
print(type(of: num))	// Optional<Int>

var num2 = num
print(type(of: num2))	// Optional<Int>

var num3: Int = num
print(type(of: num3))	// Int
```

隐式拆箱的`Optional`数据只可用于定义不会为空的常量或变量。如果对空数据隐式拆箱，同样会触发运行时错误。

### Nil-Coalescing Operator

可以使用双问号`??`来对`Optional` 数据进行带默认值的拆箱操作：

```swift
var num: Int? = 7
var realNum = num ?? 9
print(type(of: realNum), realNum)	// Int 7

var num2: Int?
realNum = num2 ?? 22
print(type(of: realNum), realNum)	// Int 22
```

如上所示，如果`Optional`数据包含合理值，那么`??`会将数据拆箱赋给另一常量或变量。否则，会将双问号后面的默认值赋给数据。
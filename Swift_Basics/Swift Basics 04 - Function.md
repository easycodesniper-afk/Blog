# Swift Basics 04 - Function

## 更灵活的定义方式

C系风格函数有一个问题，参数一多，括号里跟一大串，参数名再乱写，函数的意思就比较难估摸：

```c++
int funcA(int a, int b, int c, float d);
```

OC风格函数的参数穿插在函数名间，一定程度上解决了函数意义不明的麻烦：

```objective-c
- (int)funAWithLength:(int)a weight:(int)b type:(int)c atTime:(float)d;
```

但是这种方式**不支持默认参数**。而且如果一个函数的参数过多，就必须在函数名里把每个参数修饰一下，这会导致函数名过长，缺少了C系函数的整体感。

Swift则取两家之长，设计了可以在括号中修饰参数的函数定义方法：

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    var greeting = ""
    
    if alreadyGreeted {
        greeting = "Hello again, " + person + "!"
    } else {
        greeting = "Hello, " + person + "!"
    }
    
    return greeting
}

print(greet(person: "Anna", alreadyGreeted: false))		// "Hello, Anna!"
```

你可以在每一个参数名前面加一个单词长度的**参数标签**，空格隔开。**如果参数标签存在，则在调用函数时不写参数名而使用参数标签**：

```swift
func greet(for person: String) -> String {
	greeting = "Hello, " + person + "!"
}

print(greet(for: "Anna"))		// "Hello, Anna!"
```

**如果参数标签是一个下划线**，则在调用函数时，对于该参数可以不写任何前缀修饰：

```swift
func greet(_ person: String) -> String {
	greeting = "Hello, " + person + "!"
}

print(greet("Anna"))	// "Hello, Anna!"
```

这种可选修饰的方式很好的解决了C风格函数参数意义不明的问题，也避免了OC风格中函数名过长的情况。

## 默认参数

```swift
func someFunc(_ a: Int, _ b: Int = 12) -> Int {
    return a + b
}

print(someFunc(2))			// 12
print(someFunc(3, 4))		// 7
```

> 所有带默认值的参数必须放在不带默认值参数的后面。

## 可变参数

在一个参数的参数类型后面加上`...`，来将一个参数设置为可变参数：

```swift
func getMean(_ numbers: Double...) -> Double {
    print(type(of: numbers))	// Array<Double>
    var total = 0.0
    for num in numbers {
        total += num
    }
    return total / Double(numbers.count)
}

getMean(1, 2, 3, 4, 5)	// return 3.0
getMean(2.2, 3, 7.4)	// return 4.2
```

> 每个函数最多只能有一个可变参数。
>
> 紧跟在可变参数之后的参数**必须要有参数名或参数标签来修饰**。

## In-Out 参数

函数的入参全部默认为常量，所有对入参的更改会触发编译时错误。

如果想对参数进行更改，并将更改结果影响到函数外的作用域，则要使用 In-Out 参数。在参数的类型前加上`inout`关键字，该参数就被定义为 In-Out 参数。

不可以将常量作为 In-Out 参数传递给函数。调用函数时，需要在 In-Out 参数前加`&`：

```swift
func swapInts(_ a: inout Int, _ b: inout Int) {
	let tmp = a
    a = b
    b = tmp
}

var aa = 3
var bb = 10
swapInts(&aa, &bb)
print(aa, bb)		// 10 3
```

> In-Out 参数不可以有默认值。
>
> In-Out 参数不可以是可变参数。

## 多返回值

得益于元组机制的存在，Swift可以轻松支持多个返回值的函数：

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var tmpMin = array[0]
    var tmpMax = array[0]
    
    for value in array[1..<array.count] {
        if value < tmpMin {
            tmpMin = value
        } else if value > tmpMax {
            tmpMax = value
        }
    }
    
    return (tmpMin, tmpMax)
}

let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// "min is -6 and max is 109"
```

## 隐式返回

如果整个函数只有一句表达式，那么会默认返回这个式子，不需`return`关键字。比如以下两个函数等价：

```swift
func greeting(for person: String) -> String {
	"Hello, " + person + "!"
}

func greeting(for person: String) -> String {
	return "Hello, " + person + "!"
}
```

## 函数类型

函数类型由其参数类型与返回类型决定：

```swift
func displayNum(_ a: Int) {
    print(a)
}

print(type(of: displayNum))		// (Int) -> ()

func nothing(_ a: Int = 12, _ b: Double..., c: inout String) -> (m: Int, n: Int) {
    return (2, 3)
}

print(type(of: nothing))		// (Int, Double..., inout String) -> (m: Int, n: Int)
```

有了函数类型，就可以像普通数据一样使用函数，也可以将函数作为函数的参数或返回值：

```swift
func stepForward(_ input: Int) -> Int {
    return input + 1
}

func stepBackward(_ input: Int) -> Int {
    return input - 1
}

func chooseStepFunction(backward: Bool) -> (Int) -> Int {
    return backward ? stepBackward : stepForward
}

var myFunc: (Int) -> Int = stepForward(_:)
print(myFunc(3))	// 4

myFunc = chooseStepFunction(backward: true)
print(myFunc(3))	// 2
```


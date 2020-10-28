# Swift Basics 05 - Closure

闭包这个东西大部分语言都有，像OC的block，C#、C++11中的Lambda等。通俗地讲，闭包就是**可以捕获上下文数据的匿名函数**。

以 Swift 中 Array 的排序函数`sorted(by:)`为例，该函数需要一个自定义的函数作为参数，自定义函数需要决定Array中的两个元素的排列顺序：

```swift
let names = ["AA", "BB", "CC", "DD", "EE"]

func backward(_ s1: String, _ s2: String) -> Bool {
	return s1 > s2
}

var newNames = names.sorted(by: backward)	// ["EE", "DD", "CC", "BB", "AA"]
```

上述例子中，`backward`决定字典序大的字符串排在前面，经过排序的新数组`newNames`是按字典序从大到小的顺序排列的。这里首先定义了一个函数，再将函数作为参数传递给`sorted(by:)`。如果使用闭包的话，则如下所示：

```swift
let names = ["AA", "BB", "CC", "DD", "EE"]

var newNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
	return s1 > s2                  
})		// ["EE", "DD", "CC", "BB", "AA"]
```

标准情况下的闭包定义语法如下：

```swift
{ (parameters) -> return type in
	statements
}
```

> 闭包中可以使用 In-Out 参数和可变参数，但不能使用默认参数。

如果闭包的参数与返回值的类型是已经明确的，则可以省略掉：

```swift
newNames = names.sorted(by: { s1, s2 in return s1 > s2 })
```

与单句函数的隐式返回相同，该闭包只有一句话，也可以隐式返回：

```swift
newNames = names.sorted(by: { s1, s2 in s1 > s2 })
```

对于上述用来当参数使用的内联闭包，Swift 提供了速记符`$0, $1, $2`等。速记符可以直接用来隐式代表内联闭包的参数：

```swift
newNames = names.sorted(by: { $0 > $1 })
```

## 尾随闭包

如果一个闭包是作为一个函数的**最后一个参数**被使用，那么可以在调用这个函数的括号之后来编写闭包的内容：

```swift
newNames = names.sorted() { $0 > $1 }
```

如果这个函数就这一个参数，那么括号也不必写了：

```swift
newNames = names.sorted { $0 > $1 }
```

尾随闭包的写法适用于内联闭包很长的情况。如 Array 的`map(_:)` 方法，需要一个内联闭包作为参数，来遍历每一个元素，并生成一个新的 Array：

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]

let numbers = [16, 58, 510]

let strings = numbers.map { (number) -> String in
	var number = number
	var output = ""
	repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
	return output
}
// ["OneSix", "FiveEight", "FiveOneZero"]
```

如果一个函数的参数中，后几个参数都是闭包，那么第一个闭包可以使用尾随闭包的写法，**之后的闭包使用参数标签隔开**，也使用尾随闭包的写法：

```swift
// 函数声明
func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
    if let picture = download("photo.jpg", from: server) {
        completion(picture)
    } else {
        onFailure()
    }
}

// 函数调用
loadPicture(from: someServer) { picture in
    someView.currentPicture = picture
} onFailure: {
    print("Couldn't download the next picture.")
}
```

## 捕获数据

后议

## 逃逸闭包

后议

## 自动闭包

后议








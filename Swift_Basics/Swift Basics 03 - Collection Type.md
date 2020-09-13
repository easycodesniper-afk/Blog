# Swift Basics 03 - Collection Type

Swift提供了Array，Set，Dictionary三种集合类型，不过Swift使用`let`和`var`来区分不可变与可变集合，不再像OC一样提供额外的可变类型。

## Array

### 初始化

标准情况下定义一个空Array要这么写：

```swift
var someInts = Array<Int>()
```

但通常情况大家都用简写：

```swift
var someInts = [Int]()
```

如果一个Array变量的元素类型是**已经明确**的，可以直接通过中括号来置空：

```swift
var someInts = [Int]()
someInts.append(3)
someInts = []		// someInts现在为空数组
```

如果想在初始化时设置默认值，可以这么写：

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)

var shoppingList = ["Eggs", "Milk"]
```

### 增删查改

**相同元素类型**的Array可以直接使用`+`，`+=`符号来进行组合操作：

```swift
var a = [1, 2, 3]
var b = [3, 4, 5]
var c = a + b			// [1, 2, 3, 3, 4, 5]
c += [7, 8]				// [1, 2, 3, 3, 4, 5, 7, 8]
```

其他增删改操作：

```swift
var sList = ["one", "two"]

sList.append("three")		// 在末尾增加数据。

sList[2] = "AA"		// 使用下标修改单一数据，不可过界。

sList[1...2] = ["A", "B"]		// 使用下标修改范围数据，不可过界。

sList.insert("zero", at: 0)		// 在某一位置插入数据，该位置及之后的原数据后移一位。位置不可大于原末尾加1

sList.remove(at: 0)		// 移除某一位置的数据，该位置之后的原数据前移一位

sList.removeLast()		// 移除最后一个元素
```

### 遍历

可以使用`for-in`循环进行遍历：

```swift
for item in sList {
		print(item)
}
```

如果需要下标，可以使用`for-in`循环遍历Array的`enumerated()`方法，元素类型为包含下标与值的元组：

```swift
for (index, value) in sList.enumerated() {
		print(index, value)
}
```

## Set

着实用的不多，先不讲。

## Dictionary

### 初始化

```swift
var dic = Dictionary<Int, String>()

var dic = [Int: String]()

let dic = [1: "one", 2: "two"]
```

### 增删查改

```swift
var dic = [Int: String]()

// 增改：
dic[3] = "three"

dic[3] = "Three"

dic.updateValue("four", forKey:4)

dic = [:]			// 已明确键值类型时，可直接置空，同Array

// 删：
dic[3] = nil

dic.removeValue(forKey: 4)

// 查：
let someValue = dic[3]

print(type(of: someValue))	// Optional<String>
```

`updateValue(_:forKey:)`方法，`removeValue(forKey:)`方法都返回一个`Optional`变量，代表该键旧值。若旧值不存在，则变量为空。

### 遍历

```swift
// 元组遍历键值
for (k, v) in dic {
  	print("\(k): \(v)")
}

// 只遍历键
for k in dic.keys {
  	print(k)
}

// 只遍历值
for v in dic.values {
		print(v)
}
```


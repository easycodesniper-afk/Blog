# 类和对象是什么

“万物皆对象”，似乎每一套面向对象语言都有着这样一句话。Objective-C也不例外，作为一门通用，高级，面向对象的编程语言，其也秉承着万物皆对象的大体规则，以NSObject为初始基类建立了一套高效易用的Foundation框架。但是在这之下，OC究竟是如何维护这套体系的运作，就要从Objective-C Runtime及类与对象的本质说起。

## objc_object

我们知道OC中，类为Class类型，对象为id类型。在Runtime源码中，可以找到这样几句：

```c
typedef struct objc_class *Class;
typedef struct objc_object *id;

struct objc_class : objc_object {
	//...
}
```

类实际上是`objc_class`结构体类型，并且继承自对象结构体`objc_object`，这说明类本身也是一种对象。`objc_object`的结构如下所示：

```c
struct objc_object {
private:
	isa_t isa;
public:
	//公有方法
private:
	//私有方法
}

union isa_t {
  	isa_t() { }
	isa_t(uintptr_t value) : bits(value) { }

	Class cls;
	uintptr_t bits;
#if defined(ISA_BITFIELD)
	struct {
	ISA_BITFIELD;  // defined in isa.h
	};
#endif
};
```

`objc_object`的唯一成员变量`isa`为联合体`isa_t`类型，其结构体值通过在不同的软硬件环境下定义宏`ISA_BITFIELD`的不同内容来填充。在64位iPhone中，`ISA_BITFIELD`的定义如下：

```c
#if SUPPORT_PACKED_ISA
# if __arm64__
#  define ISA_BITFIELD
	uintptr_t nonpointer        : 1;                                       \
    uintptr_t has_assoc         : 1;                                       \
    uintptr_t has_cxx_dtor      : 1;                                       \
    uintptr_t shiftcls          : 33; /*MACH_VM_MAX_ADDRESS 0x1000000000*/ \
    uintptr_t magic             : 6;                                       \
    uintptr_t weakly_referenced : 1;                                       \
    uintptr_t deallocating      : 1;                                       \
    uintptr_t has_sidetable_rc  : 1;                                       \
    uintptr_t extra_rc          : 19
```

各bit位含义如下：

|      变量名       | bit位 |                         含义                          |
| :---------------: | :---: | :---------------------------------------------------: |
|     nonpoint      |   1   | 为1时，代表isa为一个结构体。否则代表其为一个Class指针 |
|     has_assoc     |   1   |          为1时，代表该对象存在associated对象          |
|   has_cxx_dtor    |   1   |        为1时，代表该对象存在C++或ARC的析构函数        |
|     shiftcls      |  33   |          isa为结构体时，此部分代表Class指针           |
|       magic       |   6   |            通过此位判断对象是否已经初始化             |
| weakly_referenced |   1   |           为1时，代表该对象是否持有weak对象           |
|   deallocating    |   1   |               为1时，代表该对象正在析构               |
| has_sidetable_rc  |   1   | 为1时，代表该对象的引用计数过大，无法在extra_rc中保存 |
|     extra_rc      |  19   |               表示该对象超过1的引用计数               |

## objc_class

再来看一下`objc_class`的结构：

```C
struct objc_class : objc_object {
	Class superclass;
	cache_t cache;
	class_data_bits_t bits;
	//公有方法
}
```

`superclass`指向父类Class显而易见。而objc_class的`isa`指向的则是元类结构：

![img](http://yulingtianxia.com/resources/Runtime/class-diagram.jpg)


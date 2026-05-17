# 从 C 转向 C++

> 本文档面向**已有 C 语言基础**的开发者。
> 由于 C++的基础语法与 C 语言有相似之处，故仅整理二者核心差异与 C++的新增特性。
> 阅读后即可顺利进入 C++的类、模版、STL 等章节学习。
> 本文档仅涉及基础语法，仅为了快速进入 C++学习，部分概念和用法建议后续用到了再针对性补充

## 核心差异速查表

下表已经涉及了部分 C++语法，看不懂可先行跳过（下文有具体解释），**本表主要用于快速对比差异**。

| 常见任务        | C 写法                                | C++ 写法                                         |
| ----------- | ----------------------------------- | ---------------------------------------------- |
| 输出          | `printf("Hello\n");`                | `cout << "Hello" << endl;`                     |
| 输入          | `scanf("%d", &a);`                  | `cin >> a;`                                    |
| 字符串定义       | `char str[100];`                    | `string str;`                                  |
| 字符串拷贝       | `strcpy(dest, src);`                | `dest = src;`                                  |
| 动态分配单个对象    | `int* p = malloc(sizeof(int));`     | `int* p = new int(42);`                        |
| 释放单个对象      | `free(p);`                          | `delete p;`                                    |
| 动态数组        | `int* arr = malloc(n*sizeof(int));` | `int* arr = new int[n];` 或 `vector<int> v(n);` |
| 释放数组        | `free(arr);`                        | `delete[] arr;`                                |
| 函数参数传递（大对象） | 传指针 `void f(Struct* s)`             | 传引用 `void f(Struct& s)`                        |
| 空指针         | `NULL`                              | `nullptr`                                      |
| 布尔类型        | 用 `int` 模拟 (0/1)                    | `bool` / `true` / `false`                      |
| 常量定义        | `#define PI 3.14`                   | `constexpr double PI = 3.14;`                  |
| 类型别名        | `typedef int MyInt;`                | `using MyInt = int;`                           |

## 程序结构与命名空间

下面介绍 C++的 [C++基本程序结构](#Hello%20World)及其引入的[命名空间](#命名空间) 。

### Hello World!

我们都知道，程序员的第一个程序是亘古不变的 `Hello World`。C++版本如下：

```Cpp
#include <iostream>
using namespace std;

int main() {
	cout << "Hello World!" << endl;
	return 0;
}
```

**差异说明**：
1. `#include <iostream>`：显而易见，和 C 中的 `#include <stdio.h>` 类似，同为**头文件**，包含标准输入输出流。
2. `using namespace std;`：引入C++**标准命名空间**的语法，用于简化代码。
3. `cout << ... << endl`：C++标准输出流，`endl` 换行并刷新缓冲区。

**注意**：命名空间的引入不可滥用，**小型示例可用，大型项目推荐显示写法**（如 `std::cout`）

### 命名空间

#### 为什么需要命名空间

命名空间主要是为了**解决命名冲突**的问题。

你试想，C++有那么多第三方库，那不同的库中是否会包含相同名称的变量、函数或结构体。

这种情况在大型项目中经常出现。因此，C++引入命名空间来区分它们。

这时，你也不难发现，为什么大型项目不建议直接引入来简化。都引入了不就相当于没有命名空间嘛。

#### 构建命名空间

如果你需要为自己写的函数构建命名空间，可以 参考下面的语法：

```cpp
namespace Mylib {
	void print() {
		std::cout << "Mylib::print" << std:: endl; 
	}
}

namespace YourLib {
    void print() {
        std::cout << "YourLib::print" << std::endl;
    }
}

// 调用时必须加上命名空间前缀
MyLib::print();    // 输出 MyLib::print
YourLib::print();  // 输出 YourLib::print
```

不难发现，命名空间中可以使用其他命名空间的内容。

#### 标准命名空间 std

C++标准库的所有内容，都定义在 `std` 命名空间中，是最频繁使用的命名空间。

其使用方式如下：

1. **加前缀**：

```cpp
std::cout << "Hello" << std::endl;
```

2. **using 声明（引入单用法）**：

```cpp
using std::cout;
using std::endl;
cout << "Hello" << endl;
```

3. **using 指令（引入整个命名空间）**（慎用⚠️）：

```cpp
using namespace std;
cout << "Hello" << endl;
```

#### 嵌套与别名

此处简单介绍一下命名空间的嵌套用法和起别名的方式。

```cpp
namespace Outer {
	namespace Inner {
		void func()
	}
}
// 调用：Outer::Inner::func();

//起别名
namespace OI = Outer::Inner;
OI::func();
```

## 输入输出




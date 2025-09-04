<!--
 * @Author: Luian-
 * @Date: 2025-09-04 21:19:01
 * @LastEditors: Luian luxiangxiang21@gmail.com
 * @LastEditTime: 2025-09-04 23:23:38
 * @FilePath: \vscode.py\study-notes-Luian\C语言_note.md
 * @Description: 
 * 
 * Copyright (c) 2025 by error: git config user.name & please set dead value or install git , All Rights Reserved. 
-->
# 1. C++基础

## 1.1 内联函数

内联函数(Inline Function)是C++中的一种特殊函数，其定义直接在每个调用点展开，这意味着编译器会尝试将函数调用替换为函数本身的代码，这样可以减少函数调用的开销，尤其在小函数中。  

### 1.1.1 内联函数特点

- **减少函数调用开销**：内联函数通常用于优化小型、频繁调用的函数，因为它避免了函数调用的常规开销（如参数传递、栈操作等）  
- **编译器决策**：即使函数被声明为内联，编译器也可能决定不进行内联，特别是对于复杂或递归函数 
- **适用于小型函数**：通常只有简单的、执行时间短的函数适合做内联 
- **定义在每个点**：内联函数的定义(而非仅仅声明)必须对每个使用它的文件都可见，通常意味着将内联函数定义在头文件中 

### 1.1.2 使用方法

- **示例1**：通过在函数声明前添加关键字inline来指示编译器该函数适合内联
```c
inline int max(int x, int y) {
       return x > y ? x : y;
}
```
- **示例2**：

```c
#include <iostream>
 inline int add(int a, int b) {
       return a + b;
 }
 int main() {
       int result = add(5, 3); // 编译器可能会将此替换为：int result = 5 + 3;
       std::cout << "Result: " << result << std::endl;
 return 0;
 }
```
- **note**：在这个代码中，函数add被定义为内联函数，当它被调用的时候，编译器可能会将函数调用替换为函数体内的代码

### 1.1.3 注意事项

- **过度使用的风险**：不应滥用内联函数，因为这可能会增加最终程序的大小（代码膨胀）。对于大型函数或递归函数，内联可能导致性能下降  
- **编译器决策**：最终是否将函数内联是由编译器决定的，即使函数被标记为inline 
- **适用场景**：最适合内联的是小型函数和在性能要求高的代码中频繁调用的函数 
- **note**：内联函数是一种用于优化程序性能的工具，但需要合理使用，以确保代码的可维护性和性能的平衡 

---

## 1.2 Lambda 表达式

### 2.1.1 何为Lambda表达式

- Lambda表达式是C++引入的一种匿名函数的方式，他允许你在需要函数的地方内联地定义函数，而无需单独命名函数
- Lambda表达式的基本语法如下：

```c
[capture clause](parameters) -> return_type {
     // 函数体
    // 可以使用捕获列表中的变量
    return expression; // 可选的返回语句
}
```

### 2.1.2 Lambda表达式由以下几部分组成：

- **捕获列表(Capture clause)**：用于捕获外部变量，在Lambda表达式中可以访问这些变量，捕获列表可以为空，也可以包含变量列表 [var1, var2, ...]
- **参数列表(Parameters)**：与普通函数的参数列表类似，可以为空或包含参数列表 
(param1,param2 ...)
- **返回类型（Return type）**：：Lambda 表达式可以自动推断返回类型auto，也可以显式指定返回类型 > return_type。如果函数体只有一条返回语句，可以省略返回类型
- **函数体(Body)**：Lambda 表达式的函数体，包含需要执行的代码

Lambda 表达式最简单的案例是在需要一个小型函数或临时函数时直接使用它。以下是一个非常简单的例子，其中使用 Lambda 表达式来定义一个加法操作，并立即使用它来计算两个数的和。

### 2.1.3 示例：使用 Lambda 表达式进行加法

```c
#include <iostream>
    int main() {
     // 定义一个简单的 Lambda 表达式进行加法
    auto add = [](int a, int b) {
    return a + b;
    };
     // 使用 Lambda 表达式计算两个数的和
    int sum = add(10, 20);
    std::cout << "Sum is: " << sum << std::endl;
    return 0;
}

```
在这个例子中：

- 我们定义了一个名为add的Lambda表达式，他接受两个整数参数，并返回他们的值
- 然后，我们使用这个Lambda表达式来计算两个数字的(10和20)的和，并将结果存储在变量sum中
- 最后，我们打印了这个sum的和

这个例子展示了Lambda表达式的基本用法：作为一种简洁而快速的方式来定义小型函数

我们可以写一个例子，其中使用一个函数来找出两个数中的较大数，这个函数将接受一个lambda函数作为回调来比较这两个数，Lambda函数将直接在函数调用时定义，完全是匿名的

### 2.1.4 回忆回调函数

```c
#include <iostream>
 // 函数，接受两个整数和一个比较的 lambda 函数
bool myCompare(int a, int b){
     return a > b;
 }
 int getMax(int a, int b, bool(*compare)(int, int)) {
     if (compare(a, b)) {
         return a;
     } else {
         return b;
     }
 }

 int main() {
     int x = 10;
     int y = 20;
 
     // 回调函数
    int max = getMax(x, y, myCompare);
    std::cout << "The larger number is: " << max << std::endl;
    return 0
 }

```

### 2.1.4 示例：使用匿名 Lambda 函数来返回两个数中的较大数

```c
#include <iostream>
 // 函数，接受两个整数和一个比较的 lambda 函数
int getMax(int a, int b, bool(*compare)(int, int)) {
     if (compare(a, b)) {
        return a;
     } else {
         return b;
     }
}

 int main() {
     int x = 10;
     int y = 20;
     // 直接在函数调用中定义匿名 lambda 函数
    int max = getMax(x, y, [](int a, int b) -> bool {
        return a > b;
     });
 
    std::cout << "The larger number is: " << max << std::endl;

    return 0;
 }

```

在这个例子中：
- getMax函数接受两个整数a 和 b，以及一个比较函数compare。这个比较函数是一个指向函数的指针，它接受两个整数并返回一个bool值
- 在main函数里面，我们调用getMax，并直接在调用点定义了一个匿名的的lambda函数，这个lambda函数接受两个整数并返回一个表示第一个整数是否大于第二个整数的布尔值
- 这个lambda函数在getMax中被用作比较两个数的逻辑，根据lambda函数的返回值，getMax返回较大的数

这个例子展示了如何直接在函数调用中使用匿名 lambda 函数，使代码更加简洁和直接。这种方法在需要临时函数逻辑的场合非常有用，尤其是在比较、条件检查或小型回调中。

在 Lambda 表达式中，参数捕获是指 Lambda 表达式从其定义的上下文中捕获变量的能力。这使得 Lambda 可以使用并操作在其外部定义的变量。捕获可以按值（拷贝）或按引用进行。

让我们通过一个简单的示例来展示带参数捕获的 Lambda 表达式。

### 2.1.5 示例：使用带参数捕获的 Lambda 表达式

```c
#include <iostream>
 int main() {
    int x = 10;
    int y = 20;
    // 捕获 x 和 y 以便在 Lambda 内部使用
    // 这里的捕获列表 [x, y] 表示 x 和 y 被按值捕获
    auto sum = [x, y]() {
        // x++;
        // y++; 按值捕获，关注的是值本身，无法修改
        return x + y;
    };

    std::cout << "Sum is: " << sum() << std::endl;
    std::cout << "x is now: " << x << ", y is now: " << y << std::endl;
    // 捕获所有外部变量按值捕获（拷贝）
    int z = 30;
    auto multiply = [=]() {

    // x++;
    // y++; 按值捕获，关注的是值本身，无法修改
    return x * y * z;
     };

    count << x << "," << y << endl;
    std::cout << "Product is: " << multiply() << std::endl;
    std::cout << "x is now: " << x << ", y is now: " << y << std::endl;
    // 捕获所有外部变量按引用捕获
    auto modifyAndSum = [&]() {
        x = 15; // 修改 x 的实际值
        y = 25; // 修改 y 的实际值, 引用捕获可以修改
        return x + y;
    };

 
    std::cout << "Modified Sum is: " << modifyAndSum() << std::endl;
    std::cout << "x is now: " << x << ", y is now: " << y << std::endl;
    return 0;

 }

```

在这个例子中
- 第一个 Lambda 表达式 sum 按值捕获了 x 和 y（即它们的副本）。这意味着是在 Lambda 定义时的值的拷贝
- 第二个 Lambda 表达式 multiply使用 sum 内的 x 和 y [=] 捕获列表，
- 第三个 Lambda 表达式 modifyAndSum 使用 [&] 捕获列表，这表示它按引用捕获所有外部变量。因此，它可以修改 x 和 y 的原始值。

这个示例展示了如何使用不同类型的捕获列表（按值和按引用）来控制 Lambda 表达式对外部变量的访问和修改。按值捕获是安全的，但不允许修改原始变量，而按引用捕获允许修改原始变量，但需要注意引用的有效性和生命周期问题。

### 2.1.6  Lambda 函数和内联函数在 C++ 中的相似之处和区别

| 特性       | Lambda 函数                             | 内联函数                                   |
|------------|----------------------------------------|-------------------------------------------|
| 定义       | 一种匿名函数，通常直接在需要的地方定义   | 使用 `inline` 关键字修饰的普通函数         |
| 用途       | 常用于回调、STL 算法、简短逻辑处理       | 提高函数调用效率，避免频繁调用带来的开销   |
| 语法       | `[捕获列表](参数列表) -> 返回类型 { 函数体 }` | `inline 返回类型 函数名(参数) { 函数体 }` |
| 编译优化   | 编译器会尝试内联，但不保证一定内联       | 编译器会尽量内联，但也不保证               |
| 可否命名   | 通常匿名，也可以通过变量保存            | 必须有函数名                               |
| C++ 标准   | C++11 引入                              | C++98 就已支持                             |
| 使用场景   | **一次性的小逻辑**、回调函数、STL 算法   | **频繁调用的小函数**，如数学计算、工具函数 |



请注意，虽然 Lambda 函数和内联函数在某些方面有相似之处，如它们都可以被编译器优化以减少调用开销，但它们在设计和用途上有明显的不同。Lambda 函数的核心优势在于它们的匿名性和对外部变量的捕获能力，而内联函数则主要关注于提高小型函数的性能




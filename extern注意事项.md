# `extern` 与全局变量、函数声明

## 1. 全局变量中的 `extern`

假设在 `data.c` 中定义了一个全局变量：

```c
// data.c
int count = 100;
```

如果想在 `main.c` 中使用同一个 `count`，应该使用 `extern` 进行声明：

```c
// main.c
extern int count;

int main(void)
{
    count++;
    return 0;
}
```

可以理解为：

```text
data.c
int count = 100;
    ↑
    定义（definition）
    真正定义了这个变量
```

而：

```text
main.c
extern int count;
           ↑
           声明（declaration）
           表示 count 定义在其他地方
```

因此：

```c
extern int count;
```

只是告诉编译器：

> 存在一个名为 `count` 的 `int` 类型变量，它的定义位于其他地方。

它本身不会再次定义 `count`。

---

### 为什么会出现 `multiple definition of 'count'`？

假设 `data.c` 中已经定义了：

```c
// data.c
int count = 100;
```

但是在 `main.c` 中又写：

```c
// main.c
int count;
```

这里的：

```c
int count;
```

不是单纯的外部声明，而属于**暂定定义（tentative definition）**。

因此，在现代工具链中，两个翻译单元都参与了 `count` 的定义，可能在链接阶段出现：

```text
multiple definition of `count`
```

所以跨 `.c` 文件共享全局变量时，通常采用：

```c
// data.c
int count = 100;      // 定义一次
```

```c
// main.c
extern int count;     // 只声明
```

更常见的工程写法是把声明放到头文件中：

```c
// data.h
extern int count;
```

然后：

```c
// data.c
#include "data.h"

int count = 100;
```

```c
// main.c
#include "data.h"

int main(void)
{
    count++;
    return 0;
}
```

这样可以保证整个项目中的声明保持一致。

---

## 2. 函数声明中的 `extern`

对于通常的文件作用域函数声明：

```c
void foo(void);
```

和：

```c
extern void foo(void);
```

基本等价。

函数声明通常具有外部链接，因此 `extern` 一般可以省略。

所以实际代码中更常见的是：

```c
void foo(void);
```

而不是：

```c
extern void foo(void);
```

---

## 3. 简单记忆

### 函数

```c
void foo(void);
```

基本等价于：

```c
extern void foo(void);
```

即：

```text
void foo(void);
≈
extern void foo(void);
```

### 全局变量

```c
int count;
```

和：

```c
extern int count;
```

**不等价**：

```text
int count;
≠
extern int count;
```

其中：

```c
int count;
```

在文件作用域下属于**暂定定义**；

而：

```c
extern int count;
```

通常只是**声明**，表示变量定义在其他地方。

---

## 4. 一句话总结

> 跨 `.c` 文件使用全局变量时：**一个地方负责定义，其他地方使用 `extern` 声明。**

> 对于函数声明：通常不需要显式写 `extern`，因为普通的文件作用域函数声明本身就通常具有外部链接。

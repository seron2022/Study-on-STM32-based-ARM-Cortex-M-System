1.全局变量中extern

假设：

// data.c
int count = 100;

你想在 main.c 使用这个 count，
应该写：

// main.c
extern int count;

int main(void)
{
    count++;
}

可以理解成：

data.c:
    int count = 100;
        ↑
       定义
       真正分配存储空间

main.c:
    extern int count;
               ↑
              声明
              不在这里创建变量

这也是为什么现代编译器/链接器下，经常会出现：

multiple definition of `count`

2.在函数中，
对于文件作用域的函数声明：

void foo(void);

和：

extern void foo(void);

基本等价，extern 可以省略。

所以可以这样记：

函数：
void foo(void);
≈ extern void foo(void);

变量：
int count;
≠ extern int count;

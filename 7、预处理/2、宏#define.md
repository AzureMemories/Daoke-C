# 宏#define


>* 宏处理的是字符串，而不是实际的值；
>* 一般不要在宏中使用递增或递减运算符；
>* 适当使用圆括号表达替换体；
>* 用大写字母表示宏函数的名称；
>* 在程序中只使用一次的宏无法明显减少程序的运行时间。在嵌套循环中使用宏更有助于提高效率。
>* #define宏的作用域从它在文件中的声明处开始，直到用#undef指令取消宏为止，或延伸至文件尾。
>* 如果宏通过头文件引入，那么#define在文件中的位置取决于#include指令的位置。

<br>

### #undef 指令
取消已定义的#define指令【即使没有定义】
```
#undef 标识符
```
>* 如果标识符不是宏，则对于预处理器是未定义的；
>* 如果标识符是同一个文件中由前面的#define指令创建的宏名，而且没有用其他#undef指令关闭，那么该标识符是已定义的。

<br>

### 类对象宏【明示变量】
```
#define 类对象宏  替换体/替换列表
```
>* 类对象宏——命名不允许有空格，而且必须遵循C变量的命名规则。
>* 替换体/替换列表——当预处理器在程序中找到宏示例时，就会用替换体代替该宏【宏展开】，如果替换体/替换列表中还包含宏，则继续替换【除了双引号中的宏】

<br>

### 类函数宏
```
#define 类函数宏([参数])  替换函数体
```
>* 必要时要使用足够多的圆括号来确保运算和结合的正确顺序。
>* 在宏定义中的参数为形参，在宏调用中的参数为实参。
#### 1、#符号——将实参名转换为字符串输出
```
#define PX(x) printf("The square of " #x " is %d.\n",x)

int main(){
    int y = 5;
    PX(y);
    PX(2+4);
    return 0;
}
```
```
$ gcc <>.c
The square of y is 5.
The square of 2+4 is 6.
```

#### 2、##符号——将实参名转换为标识符的一部分
```
#define XNAME(n) x##n
#define PRINT_XN(n) printf("x" #n " = %d\n", x##n )

int main(void){
    int XNAME(1) = 14;      // 变成 int x1 = 14
    int x2 = 30;
    PRINT_XN(1);            // 变成 printf("x1 = %d\n",x1)
    PRINT_XN(2);            // 变成 printf("x2 = %d\n",x2)
    return 0;
}
```
```
$ gcc  <>.c
x1 = 14
x2 =30
```
#### 3、变参宏 ... 和 __VA_ARGS__
省略号只能代替最后的宏参数。
预定义宏 __VA_ARGS__ ，对应省略号。
```
#define PR(...) printf(__VA_ARGS__)  
#define PRR(X,...) printf("Message " #X ":" __VA_ARGS__)

int main(void){
    int x = 10;
    int y = 20;
    PR("Howdy");                   // 等于 printf("Howdy")
    PRR(1,"x=%d\n",x);             // 等于 printf("Message 1:x=%d\n",x)
    PRR(2,"x=%d,y=%d\n",x,y);      // 等于 printf("Message 2:x=%d,y=%d\n",x,y)
    return 0;
}
```






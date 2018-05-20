## 函数

    void startbar(voido)

第一个 void 是函数类型，表明函数没有返回值。第二个 void 表明该函数不带参数。

调用函数：

    startbar();

示例程序：

    #include <stdio.h>
    #include <string.h>
    void show_n_char(char ch, int num);
    int main (void) {
        show_n_char('-', 2);
        return 0;
    }

    void  show_n_char(char ch, int num)
    {
        int count;

        for (count = 0; count < num; count++)
            putchar(ch);
        printf("\n");
    }

查找地址：& 运算符
***指针***是 C 语言最重要的概念之一，用于储存变量的地址。如果 pooh 是变量名，那么 &poooh 是变量地址。

    pooh = 24;
    printf("%d %p\n", pooh, &pooh);

***地址运算符***:&、*
& 后面跟一个变量时，& 给出该变量的地址，&nurse 表示变量 nurse 的地址
*后面跟一个指针或地址时，*是给出存储在指针指向地址上的值

    nurse = 22;
    ptr = &nurse; // 指向 nursede 的指针
    val = *ptr;   // 把 ptr 指向的地址上值赋给 val

如何***声明指针***
*和指针名之间的空格可有可无。通常，程序员在声明时使用空格，在解引用变量时省略空格。

    int * pi; //pi 是指向 int 类型变量的指针
    char * pc; //pc 是指向 char 类型变量的指针
    float * pf, * pg; //pf、pg 都是指向 float 类型变量的指针

使用指针解决***函数间***通信问题。

    #include <stdio.h>
    void interchange(int * u, int * v);   // 声明为指针

    int main(void)
    {
        int x = 5, y = 10;

        printf("Originally x = %d and y = %d.\n", x, y);
        interchange(&x, &y); // 传递 x, y 的地址
        printf("Now x = %d and y = %d.\n", x, y);

        return 0;
    }

    void interchange(int * u, int * v)
    {
        int temp;
        temp = *u;  // 把 x 的值传递给 temp
        *u = *v;
        *v = temp;
    }

***变量***：名称、地址和值
地址是变量在计算机内部的名称。

## 数组 array 和指针
***数组***由一系列类型相同的元素构成，***数组声明 (array declaration)***中包括数组元素的数目和元素的类型。创建数组的例子：

    float candy[365];       //365 个浮点数的数组
    char code[12];          //12 个字符的数组
    int states[50];         //50 个整数的数组
    int powers[8] = {1, 2, 3, 4, 5, 6, 7, 8};  // 初始化数组
    const int day[MONTHS] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}; // 只读数组
    int arr[6] = {[5] = 212}; //c99 特性，把 arr[5] 初始化为 212
    arr[1] = 1;             // 下标赋值
    const int days[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31}; // 当使用空的 方括号对数组进行初始化时，编译器会根据列表中的数值树木来确定数组大小。
    sizeof days / sizeof days[0]   // 计算数组大小。整个数组的大小除以单个元素的大小就是数组中元素的数目。




数组元素和未初始化的普通变量一样，其中存储的是无用的数值；但是如果部分初始化数组，未初始化的元素则被设置为 0。
编译器不检查索引的合法性。

***多维数组***

    float rain[5][12];   //rain 是一个包含 5 个元素的数组






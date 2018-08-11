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

***指针和数组***
指针能够有效地处理数组。

    array1 == &array1[0]   // 数组名是该数组首元素的地址

在 c 中，对一个指针加 1 的结果是对该指针增加 1 个存储单元（storage unit）。这就是为什么在声明指针时必须声明它所指向的对象的类型。c 语言标准在描述数组时，借助了指针的概念。例如，定义 ar[n] 时，即“寻址到内存中的 ar，然后移动 n 个单位，再取出值”。

    array1 + 2 == &array1[2]   // 相同的地址
    *(array1 + 2) == array1[2] // 相同的值


## 字符串和字符串函数
***字符串 是以空字符(\0)结尾的char类型数组
puts()函数只显示字符串，而且自动在显示的字符串末尾加上换行符。
定义字符串的几种方法：

    #include <stdio.h>
    #define MSG "i am a symbolic string constant."     //字符串常量
    #define MAXLENGTH 81
    int main(void)
    {
        char words[MAXLENGTH] = "I am a string in an array."; //char类型数组
        const char * pt1 = "Something is pointing at me.";   //指向char的指针
        puts("Here are some string:");
        puts(MSG);
        puts(words);
        puts(pt1);
        words[8] = 'p';
        puts(words);
        return 0;

    }

+ 字符串常量 属于静态存储类别
+ 字符串数组 要确保数组的元素至少比字符串长度多1（为了容纳空字符数组）
+ 使用指针表示法创建字符串。

    const char * pt1 = "someting is pointing at me." //变量。
    const char ar1[] = "someting is pointing at me." //常量。和上面等价,带双引号的字符串本身决定预留给字符串的存储空间。
    两者都可以使用数组表示法：
    for (i = 0 ; i < 8 ; i++ )
        putchar(pt1[i]);
        putchar(ar1[i]);
    都可进行指针加法:
    for (i = 0 ; i < 8 ; i++ )
        putchar(*(pt1 + i));
        putchar(*(ar1 + i));
    只有指针能够进行递归操作:
    while (*(pt1) != '\0')
        putchar(*(pt1++));

    pt1 = ar1 //pt1指向数组ar1
    ar1 = pt1 //非法构造，赋值运算符左侧必须是变量。数组的元素是变量，但数组名不是变量。


***错误***的事例：
    char *name;
    scanf("%s", name);   //scanf()要把拷贝至参数指定的地址上，而此时该参数是个未初始化的指针

***gets()函数***，读取一整行的输入，遇到换行符丢弃换行符经常同puts()函数配对使用。




## 函数

    void startbar(voido)

第一个void是函数类型，表明函数没有返回值。第二个void表明该函数不带参数。

调用函数：

    startbar();

示例程序:

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

查找地址:&运算符
***指针***是C语言最重要的概念之一，用于储存变量的地址。如果pooh是变量名，那么&poooh是变量地址。

    pooh = 24;
    printf("%d %p\n", pooh, &pooh);

***地址运算符***:&、*
&后面跟一个变量时，&给出该变量的地址，&nurse表示变量nurse的地址
*后面跟一个指针或地址时，*是给出存储在指针指向地址上的值

    nurse = 22;
    ptr = &nurse; //指向nursede的指针
    val = *ptr;   //把ptr指向的地址上值赋给val

如何***声明指针***
*和指针名之间的空格可有可无。通常，程序员在声明时使用空格，在解引用变量时省略空格。

    int * pi; //pi是指向int类型变量的指针
    char * pc; //pc是指向char类型变量的指针
    float * pf, * pg; //pf、pg都是指向float类型变量的指针

使用指针解决***函数间***通信问题.

    #include <stdio.h>
    void interchange(int * u, int * v);   //声明为指针

    int main(void)
    {
        int x = 5, y = 10;

        printf("Originally x = %d and y = %d.\n", x, y);
        interchange(&x, &y); //传递x, y的地址
        printf("Now x = %d and y = %d.\n", x, y);

        return 0;
    }

    void interchange(int * u, int * v)
    {
        int temp;
        temp = *u;  //把x的值传递给temp
        *u = *v;
        *v = temp;
    }

***变量***：名称、地址和值
地址是变量在计算机内部的名称。

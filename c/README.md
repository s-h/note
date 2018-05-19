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




1. 声明变量：C语言声明一个变量时，编译器在内存中留出一个唯一的地址单元来存储变量。  
如下图，变量var初始化为100，编译器将地址为1004的内存单元留给变量，并将地址1004和该变量的名称关联起来。
![此处输入图片的描述][1]  
  

2. 创建指针：变量var的地址是1004，是一个数字，地址的这个数字可以用另一个变量来保存它，假设这个变量为p，此时变量p未被初始化，系统为它分配了空间，但值还不确定，如下图所示。
![此处输入图片的描述][2]


3. 初始化指针，将变量var的地址存储到变量p中，初始化后（`p=&var`），p指向var，称为一个指向var的指针。指针是一个变量，它存储了另一个变量的地址。
![此处输入图片的描述][3]


4. 声明指针：`typename *p`  其中typename指的是var的变量类型，可以是 short ,char ,float,因为每个类型占用的内存字节不同，short占2个字节，char占1个字节，float占4个字节，指针的值等于它指向变量的第一个字节的地址 。`*`是间接运算符，说明p是指针变量，与非指针变量区别开来。


5. `*p`和var指的是var的内容；p和`&var`指的是var的地址
![此处输入图片的描述][4]


6.既然指针`*p`的值等于var,p的值等于`&var`，为什么要多发明这一个指针符号增加记忆量呢。指针主要的功能有两个：避免副本和共享数据。  
指针的重要功能是函数之间传递参数。  
Talk is cheap, show me the code!

假设用c语言设计一个游戏，控制人物向前走的函数为 go_forward(),这个函数接收游戏人物的坐标（int x,int y) 两个变量，对这两个变量进行加减操作。

    #include <stdio.h>
    void go_forward(int position_x,int position_y)
    {
        position_x=position_x+1;
        position_y=position_y+1;
    }
    int main()
    {
        int x=0;
        int y=0;
        go_forward(x,y);
        printf("当前坐标为：%d,%d \n",x,y);
        return 0;
    }

你希望执行go_forward()函数后x,y坐标都+1，输出为(1，1)，但是结果还是(0，0)，原因是C语言调用函数的方式是按值传递参数。
以x参数为例，刚开始main函数中有一个x的局部变量，值为0，当计算机调用`go_forward()`函数时，它将变量x的值复制给了参数position_x，这只是一个赋值过程将变量x赋值给变量`position_x`，相当于`position_x=x`命令，而这个命令，x的值是不发生变化的，结果如下图所示，x的值仍为0，position_x的值变为1。
![此处输入图片的描述][5]

解决方法：传递指针，用指针告诉go_forward()函数参数x的值的地址，go_forward()函数就能修改对应地址中的内容。
所以用指针的主要原因是让函数共享存储器，一个函数可以修改另一个函数创建的数据，只要提供数据在内存中的地址，修改代码如下。

    #include <stdio.h>
    void go_foward(int *position_x,int*position_y)
    {
        *position_x=*position_x+1;
        *position_y=*position_y+1;
    }
    int main()
    {
        int x=0;
        int y=0;
        go_forward(&x,&y);
        printf("当前坐标为：%d,%d \n",x,y);
        return 0;
    }

![此处输入图片的描述][6]
运行结果：当前坐标为：1，1


  [1]: http://pic4.zhimg.com/462c8a3b87b8033a76ee7165d280ac73_b.jpg
  [2]: http://pic3.zhimg.com/45fcc3da148d3b57b07f110e5ed085a6_b.jpg
  [3]: http://pic4.zhimg.com/b294108263f367e310ece2d03b3f21a3_b.jpg
  [4]: http://pic1.zhimg.com/d7467c68809c04c6cc0401831bbbca6c_b.jpg
  [5]: http://pic3.zhimg.com/ce8519ba43376a5e1ebe8793cfb13e8a_b.jpg
  [6]: http://pic3.zhimg.com/008185b3765c2ad531bd83d7426315fa_b.jpg

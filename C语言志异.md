1.

```c
   #include <stdio.h>

   int main() {
       int a = 1,2;
       printf("a : %d\n",a);
       return 0;
   }
```

**这个程序会得到编译错误（语法），逗号表达式没有错，但是在初始化和声明变量时，逗号不是逗号表达式的意义，如果要达到目的应该为（1，2）**

2.

```c
   #include <stdio.h>
   int main() {
       int i=43;
       printf("%d\n",printf("%d",printf("%d",i)));
       return 0;
   }
```

**程序会输出 4321，这是因为 printf 函数的返回值为输出字符的个数**

3.

```c
   #include <stdio.h>
   #include <unistd.h>
   int main() {
       while(1) {
           fprintf(stdout,"hello-std-out");
           fprintf(stderr,"hello-std-err");
           sleep(1);
       }
       return 0;
   }
```

**stdout 和 stderr 是不是同设备描述符。stdout 是块设备，stderr 则不是。对于块设备，只有当下面几种情况下才会被输入，1）遇到回车，2）缓冲区满，3）flush 被调用。而 stderr 则不会**

4.  file1.c

```c
 int arr[80];
```

file2.c

```c
extern int *arr;
int main() {
    arr[1] = 100;
    printf("%d\n", arr[1]);
    return 0;
}
```

**该程序可以编译通过，但运行时会出错，在另一个文件声明 extern int\* arr 一个数组时并不能得到实际的期望值，因为他们的类型不匹配，所以导致指针实际并没有指向那个数组，注意：一个指向数组的指针并不等于一个数组，应修改为 extern int arr[]**

5.

```c
   #include <stdio.h>
   int main() {
       int a=1;
       switch(a) {
           int b=20;
           case 1:
               printf("b is %d\n",b);
               break;
           default:
               printf("b is %d\n",b);
               break;
       }
       return 0;
   }
```

**我们可能以为进入 switch 后会对变量 b 进行初始化，起始不然，case 语句会跳过这条语句，所以，程序会输出一个随机内存值**

6.

```c
   #include <stdio.h>
   int main() {
       int i;
       i = 10;
       printf("i : %d\n",i);
       printf("sizeof(i++) is: %d\n",sizeof(i++));
       printf("i : %d\n",i);
       return 0;
   }
```

**你可能会认为会输出 10，4，11，其实不然，错在第三个，第一个是 10，没有任何问题，第二个是 4，是因为 32 位机器上 int 为 4 个字节，而第三个呢？sizeof 不是一个函数，是一个操作符，i++类型的 size 是一件在程序运行前（编译）完成的事情，所以，sizeof（i++）完全被 4 取代了，在运行时也就不会有 i++这个表达式了**

7.

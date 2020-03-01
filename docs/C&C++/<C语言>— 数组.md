> 书籍使我变成了一个幸福的人，使我的生活变成轻松而舒适的诗。——高尔基

<p align="center">本文已经收录至我的GitHub,欢迎大家踊跃star 和 issues。</p>
<h4 align="center"><a  href="https://github.com/midou-tech/articles" target="_blank">https://github.com/midou-tech/articles</a></h4>
<p><h4   style="color:red;text-align:center">点关注，不迷路！！！ </h4></p>
### 前言

&emsp;我本来准备C语言章节就写个指针就ok了，在我看来C语言的精华部分就是指针了。但是有很多同学就开始在群里各种拉扯C语言的其他问题，没办法，我是龙叔嘛，想想还是整理一下，把一些重要的C语言知识点都一一更新了吧。C语言指针的内容请点击 [指针（上）](https://mp.weixin.qq.com/s/tdyCTqH9WYMrS0HmRtVPng) 和 [指针（下）](https://mp.weixin.qq.com/s/-C_PHEk0ZUf7JUN0Bd80rQ)，**记得点关注，不迷路**

### 数组的基本概念

&emsp;我们把一组数据的集合称为**数组（Array）**，它所包含的每一个数据叫做**数组元素**（Element），所包含的数据的个数称为**数组长度**（Length），数组中的每个元素都有一个序号，这个序号从0开始，而不是从我们熟悉的1开始，称为**下标**（Index），所包含数组的里面元素的类型叫做**数组类型**（Type）。

&emsp;一句话就说清楚了数组的基本概念，就是这么简单，^_^。

### 数组底层结构探析

```c
int array[5];
```

**内存布局图**

![](https://tva1.sinaimg.cn/large/0082zybply1gc0twah5dmj30jm06ujrj.jpg)

&emsp;不要看这个图简单，底层就是这样的。数组是一个整体，它的内存是**连续**的；也就是说，数组元素之间是相互挨着的，彼此之间没有一点点缝隙。

&emsp;**这一点很重要,连续的内存为[指针](https://mp.weixin.qq.com/s/tdyCTqH9WYMrS0HmRtVPng)操作（通过指针来访问数组元素）和内存处理（整块内存的复制、写入等）提供了便利，这使得数组可以作为缓存使用。**

&emsp;有同学估计要说什么叫做指针操作，听龙叔絮叨下。

```c
int arr[5] = {1,2,3,4,5};
printf("%d\n",arr[3]);
```

&emsp;看上面的代码，学过数组都知道arr[3]是取第三个元素的值，**那我就要问你了，怎么取到值的呢？？？**

&emsp;不要慌，龙叔告诉你其实就是指针操作。当我们声明并定义数组`int arr[5] = {1,2,3,4,5}`，此时数组被分配了5个int大小的空间在**栈上**，并初始化了数组元素。我们都知道数组名代表数组的首元素的首地址，那么很明显就可以得到`arr[3] = arr + 3`。指针的加减操作详情请看龙叔公号，微信搜索 **龙跃十二** 即可订阅喔。

![](https://tva1.sinaimg.cn/large/0082zybply1gc0ubjecudj30vo0cmgmi.jpg)

### 数组的运算

```c
int a[] = {1,2,3,4};
printf("%d\n",sizeof(a));
printf("%d\n",sizeof(a+0));
printf("%d\n",sizeof(*a));
printf("%d\n",sizeof(a+1));
printf("%d\n",sizeof(a[1]));
printf("%d\n",sizeof(&a));
printf("%d\n",sizeof(&a+1));
printf("%d\n",sizeof(&a[0]));
printf("%d\n",sizeof(&a[0]+1));
//字符数组
char arr[] = {'a','b','c','d','e','f'};
printf("%d\n", sizeof(arr));
printf("%d\n", sizeof(arr+0));
printf("%d\n", sizeof(*arr));
printf("%d\n", sizeof(arr[1]));
printf("%d\n", sizeof(&arr));
printf("%d\n", sizeof(&arr+1));
printf("%d\n", sizeof(&arr[0]+1));
printf("%d\n", strlen(arr));
printf("%d\n", strlen(arr+0));
printf("%d\n", strlen(*arr));
printf("%d\n", strlen(arr[1]));
printf("%d\n", strlen(&arr));
printf("%d\n", strlen(&arr+1));
printf("%d\n", strlen(&arr[0]+1));
char *p = "abcdef";printf("%d\n", sizeof(p));
printf("%d\n", sizeof(p+1));
printf("%d\n", sizeof(*p));
printf("%d\n", sizeof(p[0]));
printf("%d\n", sizeof(&p));
printf("%d\n", sizeof(&p+1));
printf("%d\n", sizeof(&p[0]+1));
printf("%d\n", strlen(p));
printf("%d\n", strlen(p+1));
printf("%d\n", strlen(*p));
printf("%d\n", strlen(p[0]));
printf("%d\n", strlen(&p));
printf("%d\n", strlen(&p+1));
printf("%d\n", strlen(&p[0]+1));
//二维数组
int a[3][4] = {0};
printf("%d\n",sizeof(a));
printf("%d\n",sizeof(a[0][0]));
printf("%d\n",sizeof(a[0]));
printf("%d\n",sizeof(a[0]+1));
printf("%d\n",sizeof(a+1));
printf("%d\n",sizeof(&a[0]+1));
printf("%d\n",sizeof(*a));
printf("%d\n",sizeof(a[3]));
```

就这几个运算，估计会难倒很多同学的，不信你可以把答案写出来之后在去跑一遍，**全对找我拿红包**。

![](https://tva1.sinaimg.cn/large/0082zybply1gc0x87p5iuj30730730sq.jpg)

**sizeof(数组名)，代表整个数组的字节数；&数组名，代表取得整个数组的地址。**

### 数组的一些特性

1. 严格上说数组只有一维数组。n维数组是在一维数组里面存放一个(n-1)维数组，掌握以为数组即可。
2. 数组的长度指的是数组的元素个数不是数组空间长度。sizeof()关键字即可获取数组总的字节数，在除以元素类型的字节数即可得到数组长度。
3. C语言并不会判断数组访问越界，需要程序员判断越界访问。eg: `int arr[5] = {1,2,3,4,5}; int b = arr[10];`，这样访问也是可以拿到元素的，天知道你访问的是谁的数据。
4. 数组底层内存结构是连续的。正是由于数组结构的连续性便诞生了内存的友好性，数组分配内存是整块分配的，堆内存很友好；连续的内存是的访问内存效率高。
5. 数组大小是固定不变的。需要改变大小就需要新开一块大内存的数组，把之前的元素拷贝过来，释放之前的内存。
6. 数据根据下标随机访问的时间复杂度为 O(1)
7. 数据的插入和删除很低效：
   - 如果删除数组末尾的数据，最好情况时间复杂度为 O(1)
   - 如果删除开头的数据，则最坏情况时间复杂度为 O(n)
   - 平均情况时间复杂度也为 O(n)。



### 数组常见问题

#### 数组长度是一个非常量。

```c
int b;
scanf("%d",&b);
int arr[3*b];
```

&emsp;不知道你曾经有没有写过这样的代码，反正我写过。数组的长度和内存是在程序**编译时**就已经确定了的。b的值是在**运行时**才确定的。有两个新名词，**程序编译时&程序运行时**。

#### 数组越界访问

```c
int arr[5] = {1,2,3,4,5};
printf("%d\n",arr[-1]);
printf("%d\n",arr[1]);
printf("%d\n",arr[4]);
printf("%d\n",arr[5]);
printf("%d\n",arr[6]);
```

肉眼可见的错误，编译器竟然没报错。

![](https://tva1.sinaimg.cn/large/0082zybply1gc0w5d75saj31ba0j0gon.jpg)



### 数组相关笔试题目

1. 给你一个数组，求一个k值，使得前k个数的方差 + 后面n-k个数的方差最小 ，时间复杂度可以到O(n)。
2. 给定一个n个整型元素的数组a，其中有一个元素出现次数超过n / 2，求这个元素。
3. 给定一个含有n个元素的数组，找出数组中的两个元素X和Y使得abs（x-y）最小。
4. 在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
5. 在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。
6. 给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];）

利用数组可以出很多笔试题目，当然这些题目很多并不是考验数组本身特性大多是考算法基础的。本节就到这里了，有什么不清楚的问题欢迎留言喔，也可私信或mail。

![](https://tva1.sinaimg.cn/large/0082zybply1gc0x96ukkrj30730730sn.jpg)

<h4   style="color:blue;text-align:center">「转发」是明目张胆的喜欢，「在看」是偷偷摸摸的爱。</h4>
往期精彩回顾：

- [初学编程，该如何选择编程语言？](http://mp.weixin.qq.com/s?__biz=MzI5MTMxMDk1Nw==&mid=2247483745&idx=1&sn=0a6864ce6fef4efd16b514a6391c50ae&chksm=ec13dd63db6454757c4563ec214d836bf0258a87bf5be17fdcad7cc29235b402c224c4ed6323&scene=21#wechat_redirect)
- [学习linux命令，看这篇2w多字的命令详解就够了](http://mp.weixin.qq.com/s?__biz=MzI5MTMxMDk1Nw==&mid=2247483692&idx=1&sn=8a3174f5ad83ab3ec585f13980f059f7&chksm=ec13dd2edb64543889732d7e3791791b27fc2fd7f96f4f6c85693afa9f419c2a3748bf07de30&scene=21#wechat_redirect)
- [带你重新认识指针(上)](http://mp.weixin.qq.com/s?__biz=MzI5MTMxMDk1Nw==&mid=2247483777&idx=1&sn=51ab1db4d842edabf2be4dc142530281&chksm=ec13dd83db645495fae7ade69dcb37fc020fd4db5287cef9c27b978e35fe86704e89e2170787&scene=21#wechat_redirect)
- [指针(下)](http://mp.weixin.qq.com/s?__biz=MzI5MTMxMDk1Nw==&mid=2247483796&idx=1&sn=edbceaf47ee3eb60b197376f6bbd1aea&chksm=ec13dd96db645480d307349adcf8b2a0384f301593a7acd7124bcf3b3c84aa1fa8530181e546&scene=21#wechat_redirect)

![](https://tva1.sinaimg.cn/large/0082zybply1gc1gn6x0rkj30p00dwn02.jpg)
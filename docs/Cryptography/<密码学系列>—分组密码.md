> 我无论做什么，始终在想着，只要我的精力允许我的话，我就要首先为我的祖国服务。——《巴甫洛夫选集》

<p align="center">本文已经收录至我的GitHub,欢迎大家踊跃star 和 issues。</p>
<h4 align="center"><a  href="https://github.com/midou-tech/articles" target="_blank">https://github.com/midou-tech/articles</a></h4>
<p><h4   style="color:red;text-align:center">点关注，不迷路！！！ </h4></p>

&emsp;继上一期的流密码之后，我们就趁热打铁赶紧来看看**分组密码**是怎么一回事呢？

&emsp;在常用的一些密码系统中，分组密码在维护系统安全中仍然扮演着一个重要角色，同流密码一样，分组密码的使用也有许许多多需要我们注意的问题。

&emsp;分组密码是什么呢？分组分组顾名思义就是将明文消息分成**组**来进行加密，也就是说，加密器每次只能处理特定长度的一组数据，这里的"一组数据"就被称之为**分组**。我们也将每一个分组的比特数就称为**分组长度**。

&emsp;<font face="宋体" color=blue size=4>*噔噔噔噔！画重点来喽！*</font>

![](https://img-blog.csdnimg.cn/20200215191035414.png)

&emsp;看完分组密码的解释，你就能明白流密码和分组密码的加密器中是否含有记忆元件的解释了：对于分组密码，它处理完一个分组就结束一次加密进程，因此不需要通过内部状态来记录加密的进度；相反的是，对于流密码来说，它是对一个数据流进行连续的加密处理，因此需要加密其中的记忆元件来记录加密器内部的状态。

##### 分组密码：

&emsp;分组密码，就是将明文消息编码表示后的数字序列 x0 , x1 , …, xi , …，将其划分成长为 n 的组 x = ( x0 , x1 , …, xn - 1 )，各一个长度为 n 的分组都分别在密钥 k = ( k0 , k1 , …, kt - 1 ) 控制下，变换成长度为m的等长的输出数字序列 y = ( y0 , y1 , …, ym - 1 )，其加密函数 E: Vn× K→ Vm，Vn和Vm分别是 n 维和 m 维矢量空间，K为密钥空间。

![](https://img-blog.csdnimg.cn/20200215191357860.png)

&emsp;在图中可以看到，输入一个长度为n的明文分组，经过加密器后输出一个长度为m的密文，但是在一般情况下，我们取m=n，如果遇到n>m，则说明在数据加密中存在数据压缩，若n<m,则在数据加密中有数据扩展。

&emsp;下面来看看几种简单的设计分组密码常用的方法。

##### 代换：

&emsp;如果明文和密文的分组长度都为 n 比特, 则明文的每一个分组都有2的n次方个可能的取值。

&emsp; 为保证加密后得到的密文可以通过解密运算还原成为明文消息，明文的每一个分组都应产生惟一的一个密文分组，我们把这样的变换称为可逆的, 称明文分组到密文分组的可逆变换为代换。

&emsp;我们以n=4为例，来看看分组密码到底数怎样加密的？

![](https://img-blog.csdnimg.cn/20200215192944566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

&emsp;对于分组长度为4的代换结构，我们可以依据代换表给出代换以后的密文。

![](https://img-blog.csdnimg.cn/20200216165800868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

&emsp;但是，这种代换结构在实际应用中还有一些问题需要考虑。

&emsp;如果分组长度太小，如 n = 4，系统则等价于古典的代换密码，容易通过对明文的统计分析而被攻破。

&emsp;这个弱点不是代换结构固有的，只是因为分组长度太小。如果分组长度n足够大，而且从明文到密文可有任意可逆的代换，明文的统计特性就不会太过明显，这样以来，利用代换结构就不容易被攻破。

##### feistel结构：

&emsp;**Feistel**，基本上使每一个刚接触密码学的小伙伴们最头疼的部分了，别怕别怕，今天龙叔跟你细说Feistel结构。

&emsp;其实对于很多分组密码来说，它们的结构从本质上说都是基于一个称为 Feistel 网络的结构。Feistel 提出利用**乘积密码**可获得简单的代换密码，目的是为了使最后结果的密码强度高于每个基本密码系统产生的结果。

**乘积密码**：依次使⽤两个或两个以上基本密码。

下来我们看看真正的feistel结构。

![](https://img-blog.csdnimg.cn/2020021617164359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

&emsp;上图为整个feistel的n轮结构，但其实每一轮都是进行同样的操作，接下来我们就分析在一轮中到底都做了些什么？

![](https://img-blog.csdnimg.cn/20200216170110308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

- Feistel**加密**1轮的迭代过程

​      -  明⽂2w⽐特，被分为等⻓的两部分

​      -  第i轮⼦密钥由初始密钥K推导出的

​      -  ⼀般来说，每轮⼦密钥与K不同，也互不相同

​      -  F称为轮函数（每轮都⼀样）

![](https://img-blog.csdnimg.cn/20200216171012978.png)

&emsp;在每一轮中，都要进行左右的一次运算，并将运算结果传递给下一轮。

&emsp;再来看看我们的feistel解密过程。

![](https://img-blog.csdnimg.cn/20200216171321381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

- Feistel**解密**1轮迭代过程

​     -   被解密的数据左右交换

​     -   解密过程按照与加密过程相反顺序使⽤密钥

![](https://img-blog.csdnimg.cn/20200216171934674.png)

&emsp;解密同加密一样，也需要进行左右消息分组的运算，并且要将运算结果传递给下一轮。

&emsp;通过feistel结构的加密和解密，我们可以发现在明文消息在经过加密以后的密文，可以通过解密算法将其还原为原始的明文，也就是说该加密算法是可逆的。

&emsp;分组密码的应用非常广泛，它易于构造伪随机数生成器、流密码、消息认证码(MAC)和杂凑函数等，还可进而成为消息认证技术、数据完整性机制、实体认证协议以及单钥数字签字体制的核心组成部分。

&emsp;实际应用中对于分组密码可能提出多方面的要求，除了安全性外，还有运行速度、存储量(程序的长度、数据分组长度、高速缓存大小)、实现平台 (硬件、软件、芯片)、运行模式等限制条件。 这些都需要与安全性要求之间进行适当的折中选择。

&emsp;今天就先和大家说到这里，下一期呢，本应按照计划讲讲DES算法，但是DES算法在之前我们就已经说过啦，还有不明白的小伙伴可以翻回去再看看，链接给大家，请尽情暴击！ [聊聊密码学中的DES算法](https://mp.weixin.qq.com/s/tpupz8T5Ei-xB2pKdfKQQQ)

### 历史文章：

- [懂点密码学](https://mp.weixin.qq.com/s/kcvm79m1-3SflYUo56idsg)

- [信息安全威胁](https://mp.weixin.qq.com/s/W0HN44O1YI6UcfeOKj9N-g)
- [聊聊密码学中的DES算法](https://mp.weixin.qq.com/s/tpupz8T5Ei-xB2pKdfKQQQ)
- [《密码学系列》—— 流密码](https://mp.weixin.qq.com/s/XCi27yPXNNQkBGtNflUJqg)

<h4   style="color:red;text-align:center">求点赞👍  求关注❤️ </h4>
<h4   style="color:blue;text-align:center">「转发」是明目张胆的喜欢，「在看」是偷偷摸摸的爱。</h4>
![](https://img-blog.csdnimg.cn/20200119220000969.gif)

`如果有人想发文章，我这里提供`<font face="宋体" color=blue size=4>**有偿征文**</font>`(具体细则微信联系)，欢迎投稿或推荐你的项目。提供以下几种投稿方式：`

- `去我的github提交 issue:` https://github.com/midou-tech/articles

- `发送到邮箱: 2507367760@qq.com 或者 longyueshier@163.com  或者 longyueshier@gmail.com`

- `微信发送: 扫描下面二维码，公众号里面有作者微信号。`

`精选文章都同步在公众号里面，公众号看起会更方便，随时随地想看就看。微信搜索` **`龙跃十二`**`或者扫码即可订阅。`

<p align="center"><image src="https://tva1.sinaimg.cn/large/006tNbRwly1galsp9a07kj30p00dwae3.jpg" ></image></p>
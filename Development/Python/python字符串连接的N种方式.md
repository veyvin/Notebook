python中有很多字符串连接方式，今天在写代码，顺便总结一下：

-   最原始的字符串连接方式：str1 + str2
-   python 新字符串连接语法：str1, str2
-   奇怪的字符串方式：str1 str2
-   % 连接字符串：‘name:%s; sex:%s ’ % ('tom', 'male')
-   字符串列表连接：str.join(some\_list)

     第一种，想必只要是有编程经验的人，估计都知道，直接用 “+” 来连接两个字符串：

1.  'Jim' + 'Green' = 'JimGreen'

复制代码

    第二种比较特殊，如果两个字符串用“逗号”隔开，那么这两个字符串将被连接，但是，字符串之间会多出一个空格：

1.  'Jim', 'Green' = 'Jim Green'

复制代码

   第三种也是 python 独有的，只要把两个字符串放在一起，中间有空白或者没有空白：两个字符串自动连接为一个字符串：

1.  'Jim''Green' = 'JimGreen'
2.  'Jim'  'Green' = 'JimGreen'

复制代码

   第四种功能比较强大，借鉴了C语言中 printf 函数的功能，如果你有C语言基础，看下文档就知道了。这种方式用符号“%”连接一个字符串和一组变量，字符串中的特殊标记会被自动用右边变量组中的变量替换：

1.  '%s, %s' % ('Jim', 'Green') = 'Jim, Green'

复制代码

   第五种就属于技巧了，利用字符串的函数 join 。这个函数接受一个列表，然后用字符串依次连接列表中每一个元素：

1.  var\_list = \['tom', 'david', 'john'\]
2.  a = '\#\#\#'
3.  a.join(var\_list) =  'tom\#\#\#david\#\#\#john'

复制代码

   其实，python 中还有一种字符串连接方式，不过用的不多，就是字符串乘法，如：

1.  a = 'abc'
2.  a \* 3 = 'abcabcabc'

复制代码

来源： <http://www.openstack.org.cn/bbs/forum.php?mod=viewthread&tid=506>



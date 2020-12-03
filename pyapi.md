## 切片

切片的目的在于一个列表，取从x坐标到y坐标，不包括y坐标的一段值。

比如 L=[1,2,3,4,5]

L[0:3] 就是 [1,2,3]  // index 0 ,1,2的元素

L[:3]就是把0省略的样子

L[0:10:2] //每2个取一个元素 //[0, 2, 4, 6, 8]

倒数第一个元素是-1坐标。

原样复制 L[:]

字符串和列表的反转： L[::-1]


## 字符串

'xxx'.join([]) 是把数组中元素用xxx连接起来。

str.split('x')  以x为分隔符 分割为数组

字符串可以直接像数组一样按切片进行分类。


## 列表

列表的赋值不能简单地使用a=b

要用a = list(b)这样的操作。

### 列表生成器

[x * x for x in range(1, 11)]


## None

None如果作为对象传入函数会带来意外的错误。因此一般来说，我们都需要额外提防None传入参数。

python中很多意想不到的错误都是None导致的。


## 除法

/ 不是向下取整

// 是向下取整

取余 使用x&1 加快速度


## 基本方法

max

## 最大值

float('inf')


## 语法

内置函数想要访问外置变量，需要在内部函数内强调nonlocal。

没有关键字的时候
```
a = 10  # a1 当前模块全局变量
def outer():
    a = 9  # a2 当前outter作用域局部变量
    def inner():
        a = 8  # a3 当前inner作用域局部变量
        print(a)  # a3 8, 在inner的局部作用域中找到了a3
    inner()  # inner()函数结束，a3作为inner局部变量被释放
    print(a)  # a2 9,在outer局部作用域中找到a2
outer()  # outer()函数结束，a2作为outer局部变量被释放
print(a)  # a1 10, 在当前模块全局作用域中找到了a1
```

nonlocal的作用是找外层定义，但不找全局，即找到最外层函数为止。

global是直接和最外层定义的全局变量相关联，不是外层定义的变量。


参考链接：
https://blog.csdn.net/qw_sunny/article/details/80972357


## str

python的str

比如 s = 'abc'

我们不能通过s[0]修改原值。

a = s[0]

可以对a进行修改，因为a和原先的s已经没有关系了，str在python中目前来看很像基本类型。

字符串的替换可以使用切片和拼接

比如

```
s[:3]+x+s[4:] #相当于用x替换了坐标为3的字符
```
也可以将字符转为数组进行替换然后再转回字符串。


## 排序

s1 = ''.join(sorted(s))  
这样的排序不会对s造成影响

高级排序：
sorted(s,cmp,key,reverse)
L.sort(cmp,key,reverse)

key的作用是比较的关键字

```
student = [['Tom', 'A', 20], ['Jack', 'C', 18], ['Andy', 'B', 11]]
student.sort(key=lambda student: student[2])

//按成绩由小到大进行了排序。

student = [['Tom', 'A', 20], ['Jack', 'C', 18], ['Andy', 'B', 11]]
student.sort(cmp=lambda x, y: x[2] - y[2])

//使用cmp达到一样的效果。

//sorted的适用范围非常广，不仅仅是数组。
```

假如一个长度为2的数组的数组需要排序，但是第一个数要降序，如果第一个数字相同，第二个数字升序排列。这种情况下该如何排序呢？

```
L.sort(key=lambda x:(x[0],-x[1])) //后面返回括号的形式就是比较第一个相同时再比较第二个。
```
## lambda

g = lambda x:x+1
表示定义了函数g。收到x返回x+1。

lambda x:x+1(1) 表示 2





## 二维数组

二维数组的生成

```
for index in range(0,len(s)):
    dp.append([-2 for j in range(0,len(s))])
```



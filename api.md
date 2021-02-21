# 基本类型

## python

### 基本类型
Number（数字）

String（字符串）

List（列表）l=[]

Tuple（元组）a=()

Set（集合）s=set()

Dictionary（字典） d={}

None

### 基本计算
/ 不是向下取整

// 是向下取整

取余 %

平方 x**2

开平方 x**0.5

向上取整 math.ceil(x)

向下取整 math.floor(x)

max(a,b)

min(a,b)

float('inf')

-float('inf')

## java

### 基本类型
byte 1字节

short 2字节

int 4字节

long 8字节

float 4字节

double 8字节

boolean 内存中至少1字节

浮点除以0变为NAN NAN不等于NAN

整数除以0报错

null

### 基本计算

java.lang.Math

向上取整用Math.ceil(double a)

向下取整用Math.floor(double a)

开平方 Math.sqrt()

Math.max

Math.min

java.lang.Long.MAX_VALUE

java.lang.Long.MIN_VALUE


# 数组


## java

int[] 是一种引用

一维数组的初始化：

int[] a = new int[5];

二维数组的初始化：

int[][] a = new int[4][];

a[0] = new int[2];

java5之后的遍历方式：

for(String s: books){} //foreach方法

int[] res = new int[2]; //初始化

nums.length 求长度

## python

用List进行替代

a=[]

a=[x * x for x in range(1, 11)] //(1,11]

### 切片

切片的目的在于一个列表，取从x坐标到y坐标，不包括y坐标的一段值。

比如 L=[1,2,3,4,5]

L[0:3] 就是 [1,2,3]  // index 0 ,1,2的元素

L[:3]就是把0省略的样子

L[0:10:2] //每2个取一个元素 //[0, 2, 4, 6, 8]

倒数第一个元素是-1坐标。

原样复制 L[:]

字符串和列表的反转： L[::-1]


# 字符串的处理

## java

String 是否可以修改单个内容？如何修改？

String str

str.charAt(i) //取值



字符串分割？子串？连接？

str.substring(beginIndex,endIndex)

str.trim() 

String的修改和插入没有方便记忆的办法，采用和python一致的拼接方法。


String到数字的相互转化？ 只要记住valueOf就可以了。

Integer-->String

String str = String.valueOf(a);

String --> Integer

Integer i = Integer.valueOf(str);


## python

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


L = ['x','y','z'] ----> 'xyz'

''.join(L)

'xyz' --->  ['x','y','z'] 

s.split('') ?

L = ['0','1','2'] ---> '012' 

L = [0,1,2] 不能变成'012'的字符串


'012' ---> 12
i = int(s)

12 ---> '12'
s = str(i)


# 高级排序与lambda

## java

java 的比较相当麻烦

```
int [][]a = new int [5][2];

Arrays.sort(a, new Comparator<int[]>() { //int[] 作为比较的类型
@Override
public int compare(int[] o1, int[] o2) {
if (o1[0]==o2[0]) return o1[1]-o2[1];
return o1[0]-o2[0];
}
});


lambda则是
xxx -> {} 的形式

对于ArrayList则是
Collections.sort(list, comp);
public class Mycomparator implements Comparator {

    public int compare(Object o1, Object o2) {
        Person p1 = (Person) o1;//强转然后进行比较
        Person p2 = (Person) o2;
        if (p1.age < p2.age) return 1;
        else return 0;
    }

}


```

## python
假如一个长度为2的数组的数组需要排序，但是第一个数要降序，如果第一个数字相同，第二个数字升序排列。这种情况下该如何排序呢？

```
a.sort(key=lambda x:(-x[0],x[1])) //第一个数字倒序 第一个数字相同时第二个数字升序进行排列
```


# stl操作


## 序列


### python

a = []

//add
a.append(x) //队尾
a.insert(index,x)

//delete
a.pop() //删队列
a.pop(index)

//edit
a[index] = x

//get
a[index]

### java
https://docs.oracle.com/javase/8/docs/api/ 快速查询网址

```
a = new ArrayList<T>()
```

//add

a.add(x)
a.add(index,x)

//get

a.get(index)

//delete

a.remove(index)

//edit

a.set(index,x)



## 集合

### java

s = new HashSet

//和python达成一致
s.add(k)

s.remove(k)

//get
s.contains(k) //return boolean

//迭代 直接使用迭代器迭代 和Map里面的方法类似

set中存储元素先判断hashCode是否重复，如果重复再进行equals的比较，equals默认比较的是内存的地址。

如果set中存储Person对象，Person想要靠比如name属性来判断是否是一致的元素，这个时候就需要重新写equals的逻辑来比较对象。

默认的hashCode方法是把对象内存地址进行hash。因此为了让set可以使用name标准进行Person存储，需要重写hashCode。
```
        //重写equals()
        @Override
        public boolean equals(Object obj) {
            if (obj == null || !(obj instanceof Person)) {
                return false;
            }
            //地址相同必相等
            if (obj == this) {
                return true;
            }
            Person person = (Person) obj;
            //地址不同比较值是否相同
            return person.name.equals(this.name);
        }

        //重写hashCode()
        @Override
        public int hashCode() {
            return Objects.hash(name);
        }

        重写equals()必须重写hashCode() //原则！
```

### python
s =set()

s.add(k)

s.remove(k)

k in s //return False or True

## 字典

### java
```
m = new HashMap<T>()
```
//add
m.put(k,v)

//del
m.remove(k)
m.remove(k,v)

m.containsKey(key) //return boolean  //Map下的T必须是类而不是基本类型

//get
m.get(k) //return v //先判断有无

//foreach
for (k : m.keySet()){ //keySet返回set集合

}
//原理性 foreach
```
for(Map.Entry<k,v> e:m.entrySet()){
    e.getKey() //Entry方法
    e.getValue()
}
```

//迭代器
```
Iterator<T> it = 集合<T>.iterator()
while(it.hasNext())
    T = it.next()
//由此推之
Iterator<Map.Entry<k,v>> it = m.entrySet().iterator() //Entry作为内部类

```

T必须是类，不能是基本类型。



### python

d = {}

//add or edit

d[key] = value

//foreach

for k,v in d.items():

//del
d.pop(key)

//get
d.get(key) == None
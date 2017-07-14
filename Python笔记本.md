#### Python locale error: unsupported locale setting

CMD 下输入：

    export LC_ALL=C

#### 控制台颜色输出

print "\033[1;32;1m==2s==\033[0m"  // 绿色

print "\033[1;31;1m==2s==\033[0m"  // 红色

#### import 语句的模块顺序

1、Python 标准库模块
2、Python 第三方模块
3、应用程序自定义模块
使用一个空行分隔这三类模块的导入顺序

#### 变量名风格：

文件名 小写单词用 _ 分割
class 名 大驼峰模式
method 名 小驼峰模式
attr 名 小写单词用 _ 分割
常量 全大写下划线分开
有简写得单词就用简写的单词

#### Python 的 语法糖：

filter(func,seq) 对 seq 的item 依次执行func操作，将执行结果为 True 的item 组成一个 List/String/Tuple（取决于 seq 的类型 ）返回
map(func, seq) 对 seq 的item 依次执行func操作，将执行结果组成一个List
reduce(func, seq[, startVal]) 对 seq 的 item 迭代调用 func，startVal 作为初始值调用
lambda 允许你快速定义单行的最小函数，类似C 语言的宏
    # coding:utf-8

    def filterfunc(x):
        return x % 3 == 0

    def filter_map_test():
        mult3_filter = filter(filterfunc,[1,2,3,4,5,6,7,8,9])
        mult3_map = map(filterfunc,[1,2,3,4,5,6,7,8,9])
        print  '[filter : ]',mult3_filter
        print  '[map : ]',mult3_map

    def add(x,y): return x+y

    def reduce_test():
        print reduce(add,range(1,10))

    def lambda_test():
        """
        @ x 输入参数
        @ x * 2 返回值
        @ l_func 单行函数名
        """
        l_func = lambda x: x *2
        print l_func(3)

    if __name__ == '__main__':
        lambda_test()


#### 对象的独白：
 1.变量 None 和 任何变量类型比较返回的都是 False

#### REG:
多行模式下 re.S ： 
.  可以匹配换行符， \w 不匹配汉字 


#### Queue:
在 py3 中 Queue 包已经重命名为 queue


#### requests 库：
通过 socket.setdefaulttimeout() 设置 timeout 对 request.head()请求的延时不起作用，需要手动指定


#### 并行编程：
python有一个全局锁，一个pyhton程序无论何时只能占用一个cpu，当然，在这个cpu上，你是可以跑满100%的，对于现在的多cpu系统，想要充分利用cpu资源，你只能选择使用多进程技术(多线程也不行哦)。
因为GIL的存在，Python很难充分利用多核CPU的优势。但是，可以通过内置的模块multiprocessing实现下面几种并行模式：
多进程：对于CPU密集型的程序，可以使用multiprocessing的Process,Pool等封装好的类，通过多进程的方式实现并行计算。但是因为进程中的通信成本比较大，对于进程之间需要大量数据交互的程序效率未必有大的提高。
多线程：对于IO密集型的程序，multiprocessing.dummy模块使用multiprocessing的接口封装threading，使得多线程编程也变得非常轻松(比如可以使用Pool的map接口，简洁高效)。
分布式：multiprocessing中的Managers类提供了可以在不同进程之共享数据的方式，可以在此基础上开发出分布式的程序。
不同的业务场景可以选择其中的一种或几种的组合实现程序性能的优化。

创建线程的时间远远小于创建进程的时间，每个线程执行字数越多，每个任务所分担的线程本身的开销越小。



#### 性能优化：
参考 http://www.ibm.com/developerworks/cn/linux/l-cn-python-optim/
频繁查询操作，把 list 转换成 dict 可以提高效率， dict 使用了 hash table，查询复杂度 O（1）
List to dict：list = dict.fromkeys(list,True)
这样 list 就会转换成 key 是 list中的值， value 是 True 的数组。

求交集转换成 set 在进行操作，效率会快很多。

判断使用 if  xxx and yyy: 若 xxx 不成立，真正的判断条件 yyy 就不必执行，在这里 xxx 条件一般都比较简单，不消耗计算资源。

使用生成器创建列表的效率比直接 for 循环 快很多。
a = [w for w in list]

对代码优化的前提是需要了解程序的性能瓶颈在什么地方。
Time python a.py  Unix 系统上输入程序运行时间
real - 程序实际运行的时间
user - 用户空间话费的cpu时间
sys - 内核空间话费的cpu时间
如果sys和user之和的时间远远小于real的时间，那么说明程序大部分的时间很可能花费在IO等待。

#### sys.stdout.flush()
刷新


#### 使用socks代理报错 'module' object has no attribute 'inet_pton
解决方案：https://github.com/hickeroar/win_inet_pton

#### 文件的write 函数只接受一个参数，所以需要把输入的变量用“+”连接起来，不能和 print 一样用”,”连接。


#### Flask 使用 threading 模块的开启的多线程无法调用本身的数据库session 对数据库操作：
报错：application not registered on db instance and no application bound to current context
原因：开启的新的线程没有注册 DB 实例或者当前内容没有绑定 APP
解决方案： 使用 MySQLdb 库来操作数据库，把新线程和原本的Flask 环境分离


#### Python 类变量与实例变量
类变量：类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
可以直接使用类名+变量名的方式使用例如 Employee.count
实例变量：定义在方法中的变量，只作用于当前实例的类。
私有属性和私有方法会以 两个下划线为开头，在外部调用会报错。


#### datetime to string and reverse
datetime.strptime(end_time,'%y-%m-%d %H:%M:%S')  # 把end_time 字符串转换成制定格式的datetime。
dt = datetime.now()
string  = dt.strftime('%y-%m-%d %H:%M:%S')
把 datetime 变量转换成制定格式的字符串

#### 查看Python 第三方包安装位置：

```python
    from distutils.sysconfig import get_python_lib
    print get_python_lib()
```

#### 得到当前的线程并输出

```python
t = threading.current_thread()
print t.getName
```


#### Python 读文件是有游标的
使用 read 读文件，在重新打开文件的前提下，再次使用 read 函数读文件会接着读取文件内容。

#### MySQLdb
 在Python数据库编程中，当游标建立之时，就自动开始了一个隐形的数据库事务。
commit()方法游标的所有更新操作，rollback（）方法回滚当前游标的所有操作。每一个方法都开始了一个新的事务。

#### 类方法 vs 静态方法
类方法 classmethod ：需要 cls 参数
静态方法staticmethod：相当于一个独立的方法，放在类的作用域而已。

#### Python在 Ubuntu 下 使用pip安装的第三方库存放位置
/usr/local/lib/python2.7/dist-packages/

#### 设置 PIP 源 为 豆瓣的源

```bash
进入用户主目录下 .pip 文件夹，若没有创建 pip.conf 文件
[global] 
index-url = http://pypi.douban.com/simple/ 
```

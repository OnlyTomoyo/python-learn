# python-learn
## 大类知识
### print里面有没有r的关系
1. s2 = 'Hello, \ 'Adam\ '' (普通字符串)  
这里使用了转义字符 \'。因为字符串本身是用单引号包裹的，如果你想在内容里写一个单引号（比如 Adam's），就必须在前面加 \ 逃逸它，否则 Python 会误以为字符串在这里结束了。  
输出：Hello, 'Adam'  
2. s3 = r'Hello, "Bart"' (原始字符串)  
这里虽然用了 r，但字符串内部没有 \ 字符，只有双引号。对于双引号，普通字符串不需要转义，所以 r 在这里没有产生明显区别。  
输出：Hello, "Bart"  
3. s4 = r'''Hello, Bob!''' (多行原始字符串)  
这里使用了三引号，且前面加了 r。这意味着即使内容里有换行符 \n 或者路径中的反斜杠，它们都会被原样输出，不会被解释。  
输出：  
Hello,  
Bob!
当你处理文件路径（Windows 风格）或正则表达式时，r 前缀非常有用。  
场景一：文件路径  
不加 r：你需要写双反斜杠 \\ 来表示一个反斜杠。
````
path = "C:\\Users\\Name\\Documents\\file.txt"
````
加 r：直接写单个反斜杠即可，代码更清爽。  
````
path = r"C:\Users\Name\Documents\file.txt"
````
场景二：正则表达式
正则表达式中包含大量 \d, \w, \s 等转义序列。如果不加 r，Python 会先尝试解释这些 \，可能导致错误或需要写很多 \。
```
# 推荐写法
pattern = r"\d{3}-\d{2}-\d{4}"

# 不推荐（容易出错且难读）
pattern = "\\d{3}-\\d{2}-\\d{4}"
```
### b开头的内容
Python对bytes类型的数据用带b前缀的单引号或双引号表示：  
x = b'ABC'  
要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。  
以Unicode表示的str通过encode()方法可以编码为指定的bytes，例如：    
'ABC'.encode('ascii')  
b'ABC'  
'中文'.encode('utf-8')  
b'\xe4\xb8\xad\xe6\x96\x87'  
'中文'.encode('ascii')  
Traceback (most recent call last):  
  File "<stdin>", line 1, in <module>  
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)  
纯英文的str可以用ASCII编码为bytes，内容是一样的，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。  
在bytes中，无法显示为ASCII字符的字节，用\x##显示。  
反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法：  
b'ABC'.decode('ascii')  
'ABC'  
b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')  
'中文'  
如果bytes中包含无法解码的字节，decode()方法会报错：  
b'\xe4\xb8\xad\xff'.decode('utf-8')  
Traceback (most recent call last):  
  ...  
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte  
如果bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节：  
b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')  
'中'  
## 零碎知识
1. 在Python中，通常用全部大写的变量名表示常量：  
PI = 3.14159265359  
但事实上PI仍然是一个变量，Python根本没有任何机制保证PI不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量PI的值，也没人能拦住你。
2. 在Python中，有两种除法，一种除法是/：  
10 / 3  
3.3333333333333335  
/除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数：  
9 / 3  
3.0  
还有一种除法是//，称为地板除，两个整数的除法仍然是整数：  
10 // 3  
3  
3. Python的整数没有大小限制，而某些语言的整数根据其存储长度是有大小限制的，例如Java对32位整数的范围限制在-2147483648-2147483647
4. ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符
```
ord('中')  
20013  
chr(66)  
'B'
```

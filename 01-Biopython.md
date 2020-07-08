
**《Python 生物信息学数据管理》学习笔记| 每日更新**
![20200708202642](https://raw.githubusercontent.com/Achuan-2/Passage/master/images/20200708202642.png)
[TOC]


2020/5/2 
## 计算ΔG   
import math 这个模块
math.log(要取log的数字， 底数)底数默认为1  
```python
ATP = 3.5
ADP=1.8
Pi=5.0
R=0.00831
T=298
deltaG0=-30.5
 
import math
deltaG0+R*T*math.log(ADP*Pi/ATP)
```
>说老实话，其实俺不懂ΔG是啥意思……  

##  计算胰岛素序列中的氨基酸频率  

Tips：这里使用字符串的count来计每个氨基酸的频率

```python
insulin="GTVEQCCTSICSLYQLENYCNFVNQHLCGSHLVEAKYKVCGERGFFYTOKT"

for amino_acid in "ACDEFGHIKLMNPQRSTVWY":
	number=insulin.count(amino_acid)
	print (animo_acid, number)
```
 ## python字符串和数组反转

-  两者都使用切片， 简单粗暴！
```python
reserse_string=string[::-1]
reverse_array=array[::-1]
```
- 列表使用reverse函数  
```python
test.reverse()
print(test)
```
- 使用reversed（），返回的是迭代器对象，所以要用list转一下
```python
li =[1, 2, 3, 4, 5, 6]  
a = list(reversed(li))  
print (a)  
```
## Python从键盘输入多行文本数据的方法

从键盘直接输入多行文本数据,使用ctrl+Z表示行尾输入完成
```python
test=[]
while True:
    # 使用异常处理
    try:
            test.append(input())
    except:
            break
```

---

2020/5/3
## fasta 数据处理
计算fasta中序列的长度,并计算GC含量, 分三列输出序列名,序列长度, GC含量, 并在末尾统计一列之和

编程中出现的问题,
- 在使用字典嵌套`result[key] ={'length': length, 'GC_content':GC_content} `的时候, 把大括号写成了[], 于是提示不可哈希
- 因为要把python写入文件时是需要字符串, 所以使用format格式化反而麻烦, 改为了用`%`格式化
- 写入文件时, 好像`\t`没有正常的宽度,好像只是一个空格
- 把字典的values(), items()写混
- 在访问字典元素的时候,把key写成了ket, 导致输出的序列名都是最后一个

```python
aDict = {}
#处理fasta序列
for line in open('3.fasta_seq.txt'):
    if line[0] == '>':
        key = line.split('|')[3].strip()  #切分字符串取序列名
        aDict[key] = []
    else:
        aDict[key].append(line.strip())
output3 = open("3-3output.txt", 'w')
result = {}
output3.write("name\tlength\tGC_content\n")
for key, value in aDict.items():
    sequence = ''.join(value)
    length = len(sequence)
    G_num = sequence.count("G")  #使用字符串的count方法分别计算GC数
    C_num = sequence.count("C")
    GC_content = (G_num + C_num) / length
    result[key] ={'length': length, 'GC_content':GC_content} # 将序列长度和GC长度,放入以序列名为关键字的新列表中
    output3.write("%s \t %d \t %f \n" % (key, length, GC_content))

output3.write("Total \t ")
sum_length = 0
sum_GC = float(0)
for name , value in result.items():
    sum_length += value['length']
    sum_GC += value['GC_content']
output3.write("%d \t %f" % (sum_length, sum_GC))
```
输出结果:
![Image](https://pic4.zhimg.com/80/v2-ecaf13b6f26df8d17a22fc06ef788ca2.png)

---

2020/05/04

## 求DNA的互补序列

学习到:
- 使用str.startswith('>')来判断fasta数据第一行
- 使用str.maketrans来制作映射表, 使用seq.translate()来根据映射表进行字符转换
![Image](https://pic4.zhimg.com/80/v2-ce419fe754b81729772c25ee2887200c.png)

```python
fDict = {}
# 处理fasta文件，创建序列字典
for line in open(r'data\3-5.fa'):
    if line.startswith('>'):
        key = line.strip()
        fDict[key] = []
    else:
        fDict[key].append(line.strip())

output = open('3-5output.fasta', 'w')
for name, seq in fDict.items():
    seq = ''.join(seq)  # 将字典value的列表合并为字符串
    reverse_seq = seq[::-1] # 倒置
    trantab = str.maketrans(
        'ACGTacgtRYMKrymkVBHDvbhd',
        'TGCAtgcaYRKMyrkmBVDHbvdh')  # 使用maketrans 制作translate 的映射表
    complement_seq = reverse_seq.translate(trantab)  # 再用translate根据映射表来进行字符转换
    output.write('%s\n%s\n' % (name, complement_seq))
output.close()
```

## 统计多条序列上不同位点的碱基频率

学习到:
- 因为要统计每条序列同一位置的碱基个数, 所以干脆就根据列来创建序列字典, 在第一行初始化字典(使用head来标记),之后每行扫描,进行元素的添加
- 格式化字符串时, 使用 `%-(宽度)s`可以左对齐,从而实现输出的整齐
- [python格式化字符串的文章](https://www.jianshu.com/p/5895b8cc8355)本来很想使用format, 但是总是不如人意~~~还是先放着吧
  
问题: 
- 使用字典经常报错, 今天报 `KeyError` , 排查后发现是同一个key重复创建
- 不知道为什么格式化字符中`"\t"` 制表符会弱化为一个空格,必须在前后加空格才能正常显示
  
```python
column_dict = {}
head = 1
for line in open(r'data\3.6donorseq.txt'):
    line = line.strip()
    if head:
        column_dict = {position: []
                       for position in range(-9, 12)}  # 只在第一行创建各个位点的字典
        head = 0
    i = 0
    for base in line:
        column_dict[-9 + i].append(base)
        i = i + 1

seq_num = len(column_dict[-9]) # 随意取一列计算序列总数
print("%-8s \t %-6s \t %-6s \t %-6s \t %-6s" %('position', 'A', 'T', 'C', 'G')) # 打印第一行数据说明，使用%-s来实现左对齐
for position, col_seq in column_dict.items():
    col_seq = ''.join(col_seq)
    # 统计各个位点的碱基频率
    A_amount = col_seq.count('a') / seq_num
    T_amount = col_seq.count('t') / seq_num
    C_amount = col_seq.count('c') / seq_num
    G_amount = col_seq.count('g') / seq_num
    print("%-8s \t %.4f \t %.4f \t %.4f \t %.4f" %(position, A_amount, T_amount, C_amount, G_amount)) #打印ATCG频率
```

输出结果:
![Image](https://pic4.zhimg.com/80/v2-8b9c303b2ad2aa87ad99de6d9d43d492.png)
![Image](https://pic4.zhimg.com/80/v2-f1ba9c590891a7ec20d103d4293064f9.png)
![UTOOLS1594208410881.png](http://yanxuan.nosdn.127.net/4ab293d78ae7d84184fe8b2098612c30.png)
![](https://github.com/youyou-579/123/blob/master/2.8.jpg?raw=true)

2020/05/05

## python中如何把str转float并控制位数

使用float() 来把str转成float
使用round( float， 控制位数的个数)来进行元整， 即控制位数， 多余的位四舍五入
```python
    line_list[1] = round(float(line_list[1]), 5)
    line_list[2] = round(float(line_list[2]), 5)
```
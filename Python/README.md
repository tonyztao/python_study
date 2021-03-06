Python学习记录，字典速查。
 import seaborn as sns
sns.set(style="ticks")
exercise = sns.load_dataset("exercise")
g = sns.factorplot(x="time", y="pulse", hue="kind", data=exercise)
## 常用语法篇

### 文件读写
#### 文件读取
``` python
#普通文件读取 
file_read=open(r"test.txt",'r')
for u in file_read:
    print (u)
file_read.close
```
``` python 
#从excel读取数据
df = pd.read_excel('test.xls')
```

``` python
import csv
csv_reader = csv.reader(open('data.csv', encoding='utf-8'))  #csv文件读入，利用csv模块
for row in csv_reader:    #csv_reader把每一行数据转化成了一个list，list中每个元素是一个字符串,进行数值操作时，需要进行字符串到数值类型的强制转换
	print(row)
```
``` python	
import pandas as pd 
df_txt = pd.read_table(r'file.txt',sep=';')  # txt文件写入,sep设定分隔符 
df_csv = pd.read_csv(r'file.txt',sep=';',skiprows = 1,names=[],index_col=columns) #skiprows跳过表头 names定义表头 index_col 指定某列为行索引，否则自动索引0, 1, .....
```

#### 文件写入
```python
#普通文件写入
file=open(r"test.txt",'w')
#写入内容
file.write(content)
file.close()
```
``` python
#写入excel文件某个sheet下
df.to_excel('test.xlsx', sheet_name='sheet1')
```

``` python	
#csv文件写入
csvfile = open('test.csv', 'w',encoding='utf-8',newline='') #newline必须有 否则会有一行空行
writer = csv.writer(csvfile)
writer.writerow(list)  #list形式写入  写入一行用writerow  多行用writerows
csvfile.close()
```
``` python	
import pandas as pd
#dataframe写入csv文件
df.to_csv('lte_4g_result.csv') 
```

### 文件以及文件夹等系统路径
python中经常需要处理文件和文件夹，python中os模块用于处理和文件以及文件夹的相关操作。import os

命令 | 操作 
----|----
判断是否是个目录 | os.path.isdir() 
判断是否是个文件 | os.path.isfile() 
判断文件是否存在 | os.path.exists() 
获取当前工作目录 | os.getcwd() 
获取文件大小 | os.path.getsize(pathfile) #单位字节
获取文件名 |os.path.basename()
执行shell | os.system("./test.sh")
改变工作目录 | os.chdir() #os.chdir("D:\\")
列出目录下的文件 | os.listdir()　
创建单个目录 | os.mkdir()　　# os.mkdir("test_mkdir")
创建多级目录 | 　os.makedirs() # os.makedirs("test_mkdir\\test1")
删除空文件夹 | os.rmdir()  #要求必须为空文件夹
删除单一文件 | os.remove() 
重命名文件 | os.rename(oldfileName, newFilename)
复制文件，都只能是文件 | shutil.copyfile("old","new") 
复制文件/文件夹 | shutil.copy("old","new") #new既可以为文件夹也可以为文件，文件夹时候必须为存在的文件夹
复制整个目录 | shutil.copytree("old","new") #都只能是目录，且new必须不存在
移动文件/文件夹至 | shutil.move("old","new") # 移动文件/文件夹至 new 文件夹中
将path分割成目录和文件名 | os.path.split(path) #二元组返回
多个路径组合后返回 |os.path.join(path1,path2)


### 其他常用操作

- 字符串格式化 .format()
```python
'my name is {name},age is {age}'.format(name='hoho',age=19)
#另外一种类似用法
usernum_all='result_%s_user_all_operator'%city_name
```

- 匿名函数lambda  
匿名函数本质上是一个函数,更易读
``` python
#将一个 list 里的每个元素都平方：
map( lambda x: x*x, [y for y in range(10)] )
# lambda if else用法
map(lambda x: str(x)[0:-1] if len(str(x))==15 else x)
```

- 同时处理多个列表
``` python
ta = [1,2,3]
tb = [9,8,7]
tc = ['a','b','c']
for (a,b,c) in zip(ta,tb,tc):
    print(a,b,c)
```
- enumerate用法  
利用enumerate可以同时获得索引和值
```python
# 对一个列表，既要遍历索引又要遍历元素时,使用enumerate
list1 = ["这", "是", "一个", "测试"]
for index, item in enumerate(list1):
    print index, item
>>>
0 这
1 是
2 一个
3 测试
```
- 推导式  
列表的解析式
``` python
#10个元素放进列表的方法对比：
#第一种，普通方法：
a=[]
for i in range(1,11):
    a.append(i)
#第二种列表解析式法：
b =[i for i in range(1,11)]
```


### 作图
在python里面常用的作图模块有，matplotlib和seaborn
引入方法
``` python
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline  #notebook模式下要加入，用于图片内嵌在交互窗口
```
- matplotlib
##### plt.figure()
可以多次使用figure命令来产生多个图，其中图片号按顺序增加。  
plt.subplot(2,3,1)表示把图标分割成2*3的网格。也可以简写plt.subplot(231)。其中，第一个参数是行数，第二个参数是列数，第三个参数表示图形的标号
```python 
plt.figure(1)                # 第一张图
plt.subplot(211)             # 第一张图中的第一张子图
plt.plot([1,2,3])
plt.subplot(212)             # 第一张图中的第二张子图
plt.plot([4,5,6])
 
plt.figure(2)                # 第二张图
plt.plot([4,5,6])            # 默认创建子图subplot(111)
plt.figure(1)                # 切换到figure 1 ; 子图subplot(212)仍旧是当前图
```
```python
#subplot可以规划figure划分为n个子图，但每条subplot命令只会创建一个子图
import matplotlib.pyplot as plt
x = np.arange(0, 100)  
#作图1
plt.subplot(221)  
plt.plot(x, x)  
#作图2
plt.subplot(222)  
plt.plot(x, -x)  
 #作图3
plt.subplot(223)  
plt.plot(x, x ** 2)  
plt.grid(color='r', linestyle='--', linewidth=1,alpha=0.3)
#作图4
plt.subplot(224)  
plt.plot(x, np.log(x))  
plt.show()  

#方法二：
#划分子图
fig,axes=plt.subplots(2,2)
ax1=axes[0,0]
ax2=axes[0,1]
ax3=axes[1,0]
ax4=axes[1,1]

#作图1
ax1.plot(x, x)  
#作图2
ax2.plot(x, -x)
 #作图3
ax3.plot(x, x ** 2)
ax3.grid(color='r', linestyle='--', linewidth=1,alpha=0.3)
#作图4
ax4.plot(x, np.log(x))  
plt.show() 
```
##### plt.text()
text()可以在图中的任意位置添加文字，并支持LaTex语法  
xlable(), ylable()用于添加x轴和y轴标签  
title()用于添加图的题目
```python
import matplotlib.pyplot as plt

mu, sigma = 100, 15
x = mu + sigma * np.random.randn(10000)

# 数据的直方图
n, bins, patches = plt.hist(x, 50, normed=1, facecolor='g', alpha=0.75)

plt.xlabel('Smarts')
plt.ylabel('Probability')
#添加标题
plt.title('Histogram of IQ')
#添加文字
plt.text(60, .025, r'$mu=100, sigma=15$')
plt.axis([40, 160, 0, 0.03])
plt.grid(True)
plt.show()
```
##### 其他常用
```python
plt.legend(loc='upper left') #添加图例  
savefig("exercice_2.png",dpi=72) #保存图片    
pl.plot(x,y,linestyle='--',marker='*',color='red',linewidth=2)#参数说明linestyle 线条种类，marker 折点 ，color 颜色， linewidth 线条宽度(越大越粗)， label 该线条的标签，需要legend函数定位才能显示。
plt.title('About as simple as it gets, folks')#图的标签
ax.set_ylabel(u'成交量(套)')   #设置坐标轴名称
ax.set_xlabel(u'交易日') 
```
##### 画图种类以及对应的函数
```python
# '.'标明画散点图，每个散点的形状是个圆
plt.plot(x, y, '.')

# scatter可以更容易地生成散点图
plt.scatter(x, y)

# plot函数默认画连线图
plt.plot(x, y)

# plot画柱状图
import matplotlib.pyplot as plt
import numpy as np

n = 12
X = np.arange(n)
Y1 = (1 - X/float(n)) * np.random.uniform(0.5, 1.0, n)
Y2 = (1 - X/float(n)) * np.random.uniform(0.5, 1.0, n)

plt.figure(figsize=(12, 8))
plt.bar(X, +Y1, facecolor='#9999ff', edgecolor='white')
plt.bar(X, -Y2, facecolor='#ff9999', edgecolor='white')

for x, y in zip(X,Y1):
    # ha: horizontal alignment水平方向
    # va: vertical alignment垂直方向
    plt.text(x, y+0.05, '%.2f' % y, ha='center', va='bottom')

for x, y in zip(X,-Y2):
    # ha: horizontal alignment水平方向
    # va: vertical alignment垂直方向
    plt.text(x, y-0.05, '%.2f' % y, ha='center', va='top')

# 定义范围和标签
plt.xlim(-.5, n)
plt.xticks(())
plt.ylim(-1.25, 1.25)
plt.yticks(())

plt.show()


# plot画直方图
# bins是分段，显示数据在各个区间的个数
import matplotlib.pyplot as plt
population_ages = [22,55,62,45,21,22,34,42,42,4,99,102,110,120,121,122,130,111,115,112,80,75,65,54,44,43,42,48]
bins = [0,10,20,30,40,50,60,70,80,90,100,110,120,130]

plt.hist(population_ages, bins, histtype='bar', rwidth=0.8)
plt.xlabel('x')
plt.ylabel('y')
plt.title('Interesting Graph\nCheck it out')
plt.legend()
plt.show()


#画饼图
import matplotlib.pyplot as plt
slices = [7,2,2,13]
activities = ['sleeping','eating','working','playing']
cols = ['c','m','r','b']
plt.pie(slices,
        labels=activities,
        colors=cols,
        startangle=90,
        shadow= True,
        explode=(0,0.1,0,0),
        autopct='%1.1f%%')

plt.title('Interesting Graph\nCheck it out')
plt.show()
```
- seaborn
```python
#distplot( )为hist加强版
import matplotlib.pyplot as plt  
import seaborn as sns  
from sklearn import datasets
import pandas as pd 


data = datasets.load_iris()
df_iris = pd.DataFrame(data.data,columns=data.feature_names)
name =[data.target_names[x]  for x in data.target]
df_iris['class']= pd.DataFrame(name)
fig=plt.figure(1) 
sns.distplot(df_iris['sepal length (cm)'], kde = True)# kde 密度曲线  rug 边际毛毯                      
plt.show()  

#示例
import numpy as np  
import seaborn as sns  
import matplotlib.pyplot as plt  
sns.set( palette="muted", color_codes=True)  
rs = np.random.RandomState(10)  
d = rs.normal(size=100)  
f, axes = plt.subplots(2, 2, figsize=(7, 7), sharex=True)  
sns.distplot(d, kde=False, color="b", ax=axes[0, 0])  
sns.distplot(d, hist=False, rug=True, color="r", ax=axes[0, 1])  
sns.distplot(d, hist=False, color="g", kde_kws={"shade": True}, ax=axes[1, 0])  
sns.distplot(d, color="m", ax=axes[1, 1])  
plt.show()  



#boxplot
import matplotlib.pyplot as plt  
import seaborn as sns  
import matplotlib.pyplot as plt  
import seaborn as sns  
from sklearn import datasets
import pandas as pd 


data = datasets.load_iris()
df_iris = pd.DataFrame(data.data,columns=data.feature_names)
name =[data.target_names[x]  for x in data.target]
df_iris['class']= pd.DataFrame(name)
sns.boxplot(x = df_iris['class'],y = df_iris['sepal length (cm)'])  
plt.show()  

# 按照某一特征进行分类统计，如下示例通过性别进行统计
import seaborn as sns
%matplotlib inline 
sns.set(style="ticks")
# Load the example tips dataset
tips = sns.load_dataset("tips")
# Draw a nested boxplot to show bills by day and sex
sns.boxplot(x="day", y="total_bill", hue="sex", data=tips)

#联合分布 两个变量之间的相关性 皮尔森相关系数
sns.jointplot("total_bill", "tip", tips)  
sns.jointplot("total_bill", "tip", tips, kind='reg')  
plt.show() 

#heatmap
#correlation matrix 相关矩阵，考察变量两两相关性
corrmat = df_train.corr()
f, ax = plt.subplots(figsize=(12, 9))
sns.heatmap(corrmat,square=True, linewidths=.5, annot=True)

#热度图，先操作一个透视汇总，然后通过热度图观察分布
pt = df.pivot_table(index='kind', columns='region', values='values', aggfunc=np.sum)
sns.heatmap(pt, linewidths = 0.05)


#pairplot
import matplotlib.pyplot as plt  
import seaborn as sns  
import matplotlib.pyplot as plt  
import seaborn as sns  
from sklearn import datasets
import pandas as pd 
data = datasets.load_iris()
df_iris = pd.DataFrame(data.data,columns=data.feature_names)
name =[data.target_names[x]  for x in data.target]
df_iris['class']= pd.DataFrame(name)
sns.pairplot(df_iris,hue="class")   #hue 选择分类列  
 # 选择两个参数进行作图 参数vars进行控制
sns.pairplot(df_iris, vars=["sepal width (cm)", "sepal length (cm)"],hue='class',palette="husl") 
plt.show()  

#factorplot x轴为分类
sns.factorplot('workclass','income', data=adult_data_train_df,size=8)
sns.factorplot(x="year", data=planets, kind="count",size=6, aspect=1.5, order=years)
#示例
 import seaborn as sns
sns.set(style="ticks")
exercise = sns.load_dataset("exercise")
g = sns.factorplot(x="time", y="pulse", hue="kind", data=exercise)
```


### 数据库连接
#### mysql数据库
```python
#方法一pymysql
import pymysql
db='phone_data_chifeng_0228'
conn = pymysql.connect(host='10.100.26.201', port=3306, user='root', passwd='ChinaUnicom!#24', db=db)
cursor = conn.cursor()
sql = 'select * from result_sample_all_num'
cursor.execute(sql)
rows = cursor.fetchall()
cursor.close()
```
```python
#方法二MySQLdb
import MySQLdb
cxn = MySQLdb.connect(host = 'localhost', port=3306,user = 'root', passwd = 'ChinaUnicom!#24',db='quater_app')
cur = cxn.cursor()
sql_load = '''LOAD DATA LOCAL INFILE '%s' INTO TABLE app_data_http FIELDS TERMINATED BY '\t'  OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n';'''%(DestDir)
cur.execute(sql_load)
cur.close()
cxn.commit()
cxn.close()
```

#### mongodb数据库
```python
from pymongo import MongoClient
host = "localhost"
port = 27017
client = MongoClient(host,port)
db = client['test']
sheet = db['sheet_test']

for i in range(1001):
    print (i)
    sheet.insert_one({
        'name': "name" + str(i),
        "age": i,
    })
```
	
#### impala数据库
```python
from impala.dbapi import connect
conn = connect(host='10.100.28.161', port=21050, user='wangyf', password='unicom2016', database='wangyftest')
cursor = conn.cursor()
sqlsamplenum = 'select * from result_sample_all_num'
cursor.execute(sqlsamplenum)
rows1 = cursor.fetchall()
```

#### 读取urllib api等，返回json数据
```python
import urllib
url=r'https://api.imaoda.com/'
req = urllib.request.Request(url) 
response = urllib.request.urlopen(req) 
page = response.read()
page = page.decode('utf-8')
```
## 数据处理篇

### Numpy

Numpy引入进行科学技术的好处，总体介绍：  
标准安装的Python中用列表(list)保存一组值，可以用来当作数组使用，不过由于列表的元素可以是任何对象，因此列表中所保存的是对象的指针。这样为了保存一个简单的[1,2,3]，需要有3个指针和三个整数对象。对于数值运算来说这种结构显然比较浪费内存和CPU计算时间。

此外Python还提供了一个array模块，array对象和列表不同，它直接保存数值，和C语言的一维数组比较类似。但是由于它不支持多维，也没有各种运算函数，因此也不适合做数值运算。

NumPy的诞生弥补了这些不足，NumPy提供了两种基本的对象：ndarray（N-dimensional array object）和 ufunc（universal function object）。ndarray(下文统一称之为数组)是存储单一数据类型的多维数组，而ufunc则是能够对数组进行处理的函数。  

#### 常用函数

#### 创建
创建数组的方式
``` python
import numpy as np
a=np.array([1,2,3,4]) #一维
c=np.array([[1,2,3,4],[1,2,3,4]]) #二维 
c.shape  #查看矩阵维度
a=np.arange(12)  #arange函数类似于python的range函数，通过指定开始值、终值和步长来创建一维数组，注意数组不包括终值
a = np.linspace(0, 1, 12) #linspace函数通过指定开始值、终值和元素个数来创建一维数组
```
#### 常用函数
数组常常用于数值运算，numpy库中包括了常见的数学运算函数。
``` python
np.sin()
np.add() #计算两个数组相加可以简单地写为a+b，而np.add(a,b,a)则可以用a+=b来表示
np.dot() #数组相乘
np.max()
np.floor()
a.transpose() #等价于 a.T  数组转置
a.reshape  #调整矩阵形状 
a.flatten() #flatten函数将高维数组转化为向量
np.vstack((a,b)) #数组竖向连接  等价于 np.concatenate((a, b), axis=0)
np.hstack((a,b.T))  #数组横向连接 等价于 np.concatenate((a, b.T), axis=1)
```

#### Numpy中matrix 和 array的区别 
1. Numpy matrices必须是2维的,但是 numpy arrays (ndarrays) 可以是多维的（1D，2D，3D····ND）. Matrix是Array的一个小的分支，包含于Array。所以matrix 拥有array的所有特性。
2. 在numpy中matrix的主要优势是：相对简单的乘法运算符号。例如，a和b是两个matrices，那么a*b，就是矩阵积。而不用np.dot()，若都为数组，a*b为对应位置元素乘积
3. 通过下面的两条命令轻松的实现两者之间的转换：np.asmatrix和np.asarray


## Pandas
### Pandas的数据结构
Series：一维数组，与Numpy中的一维array类似。二者与Python基本的数据结构List也很相近，其区别是：List中的元素可以是不同的数据类型，而Array和Series中则只允许存储相同的数据类型，这样可以更有效的使用内存，提高运算效率。  
DataFrame：二维的表格型数据结构,其本质是**Series的容器**。  
DataFrame可以包含一个索引以及与这些索引联合在一起的Series，由于一个Series中的数据类型是相同的，而不同Series的数据结构可以不同。因此对于DataFrame来说，每一列的数据结构都是相同的，而不同的列之间则可以是不同的数据结构。或者以数据库进行类比，DataFrame中的每一行是一个记录，名称为Index的一个元素，而每一列则为一个字段，是这个记录的一个属性。
### 创建DataFrame
以下两种方法创建DataFrame，思路略有不同，前者是以列为单位构建，每列转为一个Series；后者是以行为单位构建，将每条记录转化为一个字典，使用这种方式，如果不通过columns指定列的顺序，那么列的顺序会是随机的。
``` python 
d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),'two' : pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)

d = [{'one' : 1,'two':1},{'one' : 2,'two' : 2},{'one' : 3,'two' : 3},{'two' : 4}]
df = pd.DataFrame(d,index=['a','b','c','d'],columns=['one','two'])
```
### DataFrame数据选取
``` python
df['one'] #通过columns 名字选取数据
df[0:2]   #返回df的前三行
df['20130102':'20130104'] #通过索引index的范围进行数据选取 

df.loc df.iloc df.ix用法比较
# df.loc的第一个参数是行标签，第二个参数为列标签（可选参数，默认为所有列标签），两个参数既可以是列表也可以是单个字符，如果两个参数都为列表则返回的是DataFrame，否则，则为Series。
df.loc['a':'b'] #选取ab两行数据
df.loc[:,['one','two']] #选取one、two两列数据
df.loc[:,'one'] #选取one列的数据
df.loc['a','one']  #选取a行(index)one列(columns)的单个数据

# df.iloc[行位置,列位置] i代表integer，输入的参数只能为整数类型
df.iloc[1,1] #选取第二行，第二列的值，返回的为单个值
df.iloc[[0,2],:] #选取第一行及第三行的数据
df.iloc[0:2,:] #选取第一行到第三行（不包含）的数据

# df.ix是一种广义用法，它自动根据你给到的索引类型判断是使用位置还是标签进行切片
df.ix[1,1]
df.ix['a':'b']

#通过逻辑运算进行数据筛选
df[df.A > -0.5] 

#query()方法筛选数据
df.query('A > C')  #A列的值大于C列的值
df.query('A > 0 & A < 1') # A列的值在0和1之间


```

### DataFrame常用函数

命令 | 操作 | 备注
----|------|----
设置索引 | df.set_index('one') | 以某个列名作为df的新索引，一般用于表关联时，如先设置为以城市名字为索引，再进行df关联
将索引作为df的某一列| df.reset_index(level=0, inplace=True) | index作为新的一列
重命名列 | df.rename(columns={'one':'1'}, inplace=True) | 对colums进行重命名
改变数据类型 | df['one'].astype(float) |
计算某列中每个值的频率 | df['one'].value_counts() |
描述性统计 | df.describe() | 计算各个列的基本描述统计值。包含计数，平均数，标准差，最大值，最小值及4分位差
表情况预览| df.info ()| 显示df的大小，数据类型，行列数、名称
去重复项 | df.drop_duplicates() | 默认是所有列都相同才为重复
删除某列 | data.drop('animal2', axis='columns', inplace=True) | axis 默认为列 
替换某列某值 | data['k1'].replace('one','three',inplace=True) | 前面为要替换的，后面为替换为的，inplace为True表示原表替换
df转置 | df.transpose() | index和columns互换
新索引进行重排 | df2.reindex([8,5,3,4],fill_value=0) | 常用于修改索引的顺序


#### 对数据进行排序  
```python 
df.sort(columns='diff') #对列diff进行排序  
df.sort(columns=['Bonus_Percent','diff'],ascending=[0,1])    #对两列进行排序，第一列为降序，第二列为升序
df.sort_index(axis=1, ascending=False)  #对索引进行排序，降序排列
df.sort_values(by=['group','ounces'], ascending=[False, True], inplace=True) #方法雷同
```

#### 对数据进行分段  pd.cut(要处理数据,分段区间)
``` python
ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
bins = [18, 25, 35, 60, 100]
cats = pd.cut(ages, bins) #分割区间默认为左开右闭，如(18, 25]
cats.labels  #查看每个数据所属的区间标号，第一个区间从0开始，此例中(18, 25] 为 0
cats.levels #Levels (4): Index(['(18, 25]', '(25, 35]', '(35, 60]', '(60, 100]']
pd.value_counts(cats) #每个区间包含的数据个数

group_names = ['Youth', 'YoungAdult', 'MiddleAged', 'Senior']
pd.cut(ages, bins, labels=group_names)
pd.value_counts(pd.cut(ages, bins, labels=group_names)) #labels 为每个区间取一个别名，如按年龄分组，可以分为少年、青少年、中年、老年等
```


#### 对数据进行分组处理，groupby
``` python
#goupby可以对df中的一个或多个键（可以是数组或DataFrame列名）拆分pandas对象。计算分组后的数据统计，如计数、平均值、标准差，或用户自定义函数等。
grouped = df['data1'].groupby(df['key1'])
grouped.mean()
# 变量grouped是一个GroupBy对象，它实际上还没有进行任何计算，只是含有一些有关分组键df['key1']的中间数据而已，然后我们可以调用GroupBy的mean方法来计算分组平均值

#也可传入多个键对数据进行分组
means = df['data1'].groupby([df['key1'], df['key2']]).mean()

#以下两个式子等价：
df.groupby('key1')['data1'] 
df['data1'].groupby([df['key1']])

#可以对分组对象进行迭代操作：
for name, group in df.groupby('key1'):
    print(name)
    print(group)
# 可以将这些数据片段做成一个字典，通过字典访问分组后的数据
pieces = dict(list(df.groupby('key1')))
pieces['b']
```

#### 数据透视表 Pivot table 
```python
# values为要计算的列名，index为分组的列名，aggfunc计算方法
data.pivot_table(values='ounces',index='group',aggfunc='count')
#多维透视表时，以index和columns构成一个二维分组透视表
by_rep = pd.pivot_table(data=df, index='Rep', columns='Product', values='Quantity', aggfunc='count', fill_value=0)
```

#### df.map() 和 df.apply ()用法以及区别
```python 
#apply()是一种让函数作用于列或者行操作，applymap()是一种让函数作用于DataFrame每一个元素的操作，而map是一种让函数作用于Series每一个元素的操作。示例如下：

# apply
In [116]: frame = DataFrame(np.random.randn(4, 3), columns=list('bde'), index=['Utah', 'Ohio', 'Texas', 'Oregon'])

In [117]: frame
Out[117]: 
               b         d         e
Utah   -0.029638  1.081563  1.280300
Ohio    0.647747  0.831136 -1.549481
Texas   0.513416 -0.884417  0.195343
Oregon -0.485454 -0.477388 -0.309548

In [118]: f = lambda x: x.max() - x.min()

In [119]: frame.apply(f)
Out[119]: 
b    1.133201
d    1.965980
e    2.829781
dtype: float64

#想让方程作用于DataFrame中的每一个元素，可以使用applymap().用法如下所示
In [120]: format = lambda x: '%.2f' % x

In [121]: frame.applymap(format)
Out[121]: 
            b      d      e
Utah    -0.03   1.08   1.28
Ohio     0.65   0.83  -1.55
Texas    0.51  -0.88   0.20
Oregon  -0.49  -0.48  -0.31

#map()只要是作用将函数作用于一个Series的每一个元素
n [122]: frame['e'].map(format)
Out[122]: 
Utah       1.28
Ohio      -1.55
Texas      0.20
Oregon    -0.31
```

#### 多表操作，合并 连接
* merge
* concat
* join
* append

##### merge
pandas.merge可根据一个或多个键将不同DataFrame中的行合并起来。
``` python
merge(left, right, how='inner', on=None,left_on=None, right_on=None, left_index=False, right_index=False, sort=True,suffixes=('_x', '_y'), copy=True, indicator=False)  

参数说明：
left与right：两个不同的DataFrame
how：指的是合并(连接)的方式有inner(内连接),left(左外连接),right(右外连接),outer(全外连接);默认为inner；
on : 指的是用于连接的列索引名称。必须存在左右两个DataFrame对象中，如果没；
left_on：左则DataFrame中用作连接键的列名；
right_on：右则DataFrame中用作 连接键的列名；
left_index：使用左则DataFrame中的行索引做为连接键；
right_index：使用右则DataFrame中的行索引做为连接键；
sort：默认为True，将合并的数据进行排序。在大多数情况下设置为False可以提高性能；
suffixes：字符串值组成的元组，用于指定当左右DataFrame存在相同列名时在列名后面附加的后缀名称，默认为('_x','_y')；
copy：默认为True,总是将数据复制到数据结构中；大多数情况下设置为False可以提高性能；

# 在未指定连接键的情况下，merge会将重叠列的列名当做键
pd.merge(left, right)

# 指定“on”作为连接键，left和right两个DataFrame必须同时存在“on”列，连接键也可N对N（少用）
pd.merge(left, right, on="key")
pd.merge(left, right, on=["key1", "key2"])

# 指定left的连接键为“lkey”，right的连接键为“rkey”
pd.merge(left,  right,  left_on="lkey",  right_on="rkey")

# suffixes:用于追加到重叠列名的末尾，默认为("_x", "_y")
pd.merge(left,  right,  on="key",  suffixes=("_left",  "_right"))

# 指定连接方式：“inner”（默认），“left”，“right”，“outer”
pd.merge(left, right, how="outer")
```
##### concat
concat函数可以将数据根据不同的轴作简单的融合。
``` python
pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,keys=None, levels=None, names=None, verify_integrity=False)

参数说明: 
objs: series，dataframe或者是panel构成的序列lsit 
axis: 需要合并链接的轴,0是按照行横向连接，1是按照列纵向连接 
join：连接的方式 inner，或者outer，如果为’inner’得到的是两表的交集，如果是outer，得到的是两表的并集
join_axes:参数传入，可以指定根据那个轴来对齐数据

#使用方法，先将DataFrame表构成list，然后在作为concat的输入
frames = [df1, df2, df3]
result = pd.concat(frames)

#默认 axis=0 上下拼接，列column重复的会自动合并
pd.concat([df1, df2], axis=0)

# axis=1 左右拼接，行index重复的会自动合并
pd.concat([df1, df2], axis=1)

# 忽略df1和df2原来的index，重新给新的DataFrame设置从0开始的index
pd.concat([df1,df2],  ignore_index=True)

# 横向连接两表，得到两表的交集
result = pd.concat([df1, df2], axis=1, join='inner')

#根据df1表对齐数据，就会保留指定的df1表的轴，然后将df4的表与之拼接
result = pd.concat([df1, df4], axis=1, join_axes=[df1.index])

#增加分组键，在索引前增加一列，标识原始数据来自于哪个DataFrame表
 result = pd.concat(frames, keys=['x', 'y', 'z']
```
##### join
join方法提供了一个简便的方法用于将两个DataFrame中的不同的列索引合并成为一个DataFrame,join方法默认为左外连接how=left
``` python
#基于index进行合并
result = df1.join(df2)
result = df1.join(df2, how='outer')

#基于index与某表的某列进行合并,df1中其中有一columns名称为key，而df2中的index为key，可将以上两个df进行合并;以下两种方法等价
result = pd.merge(df1, df2, left_on='key', right_index=True,how='left', sort=False)
result = df1.join(df2, on='key')
```

##### append 
``` python
#表头一致的多张表，进行连接（上下连接）
df1.append(df2).append(df3)

#append方法可以将series和字典数据作为dataframe的新一行插入
s2 = pd.Series(['X0', 'X1', 'X2', 'X3'], index=['A', 'B', 'C', 'D'])
result = df1.append(s2, ignore_index=True) 
```







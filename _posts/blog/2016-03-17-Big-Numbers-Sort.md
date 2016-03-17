###问题描述

系统磁盘中有文件Number.txt,这个文件至多有10^7个无序排列的数,每个数的数据范围是[0, 10^7),且每个数都不重复.现需要的你的帮忙,将其排序并输出至原文件.

要求与限制:

-  最多用1M的内存可用
-  有足够多的磁盘空间可用
-  所用时间越少越好

(题目来源:<<编程珠玑>>)

###输入

输入为一个文件Number.txt(无序).

###输出

输出为一个文件Number.txt(有序).

---
###分析
数的范围是[0, 10^7), 所以每个数可用4个字节存储表示(**你懂的:4个字节能表示数的数据范围是[0, 4294967296)**),可估计总文件的大小10^7 * 4B 约38M,所以不能直接将数据一次性读入内存处理.

每个数都不重复,可以用hash,每个数用一个字节表示,花费的内存是10^7B 约9.5M,明显超过了1M.

由于有足够多的磁盘空间可用,可利用磁盘资源.如果将38M大小的数划分为40个文件,对每个文件单独处理,此时就可以一次性将划分的小文件读入内存,每个文件就有38 / 40M大小 < 1M,所以可以考虑用40个文件来分别处理,分别将这40个文件的数排序.

将取值范围[0,10^7)划分为40个区间,每个区间数的个数就是10^7/40=250000,第一个区间就是[0, 250000), 第i个区间i * 250000 + [0, 250000) (0<=i<40).将第一个区间的数划分到第一个文件,第i个区间的数划分到第i个文件.

分别对这40个文件排序,由于每个文件的取值区间是没有交集的,且每个区间有序,所以可依次从最小的区间到最大的区间输出至Number.txt文件,即可保证Number.txt中所有元素是有序的.

当然,你也可以考虑划分50个乃至400个区间,这可由你决定.
###参考代码
假设此处自行产生输入文件.

流程: 

1. 产生10^7个随机数

2. 划分到40个不同的文件

3. 统计划分是否正确

4. 对每个文件排序并追加到Number.txt文件

(注意1,3步不是必须的!)

```

#!/usr/bin/env python
#_*_ coding:utf-8 _*_
from random import randint

#输入数据文件名
FILENAME = 'Number.txt'
#划分为多少个文件
SIZE = 40
#划分出10个小文件的文件名
SPLIT_FILENAME = 'Split'

#生成10^7个范围在[0, 10^7)的数
def Output():
    #清空原文件
    FILENAME_fd = open(FILENAME, 'wb')
    FILENAME_fd.write('')
    FILENAME_fd.close()
    #10^7个数分为1K次产生,每次产生1W个数,这样减少内存的同时使用情况
    for i in xrange(1000):
        FILENAME_fd = open(FILENAME, 'a')
        #1w个数的链表
        L = [x for x in xrange(10000)]
        for j in xrange(10000):
            #随机产生[0, len(L))的下标,将数写入文件,并从链表中删除
            index = randint(0, len(L) - 1)
            FILENAME_fd.write(str(i * 10000 + L[index]))
            FILENAME_fd.write('\n')
            del L[index]
        FILENAME_fd.close()

#将原文件划分为SIZE个文件
def Split():
    FILENAME_fd = open(FILENAME, 'r')
    SPLIT_FILENAME_fd = [open(SPLIT_FILENAME + str(i) + '.txt', 'wb') for i in xrange(SIZE)]
    for i in FILENAME_fd.readlines():
        number = int(i.strip())
        index = number / (10000000 / 40)
        SPLIT_FILENAME_fd[index].write(str(number))
        SPLIT_FILENAME_fd[index].write('\n')
    FILENAME_fd.close()
    for fd in SPLIT_FILENAME_fd:
        fd.close()

#统计每个小文件数的个数
def Count():
    SPLIT_FILENAME_fd = [open(SPLIT_FILENAME + str(i) + '.txt', 'r') for i in xrange(SIZE)]
    for fd in SPLIT_FILENAME_fd:
        print '%s有[%s]个数' % (fd.name, sum([ 1 for j in fd.readlines()]))
        fd.close()

#对每个小文件分别排序并输出至原文件
def Sort():
    FILENAME_fd = open(FILENAME, 'wb')
    SPLIT_FILENAME_fd = [open(SPLIT_FILENAME + str(i) + '.txt', 'r') for i in xrange(SIZE)]
    for fd in SPLIT_FILENAME_fd:
        #将fd的所有数读进来并排序
        L = [int(x) for x in fd.readlines()]
        L.sort()
        for x in L:
            FILENAME_fd.write(str(x))
            FILENAME_fd.write('\n')
        fd.close()
    FILENAME_fd.close()

if __name__ == '__main__':
    Output()
    Split()
    Count()
    Sort()
    print 'DONE'
    
```

Outout:

```

Split0.txt有[250000]个数
Split1.txt有[250000]个数
Split2.txt有[250000]个数
Split3.txt有[250000]个数
Split4.txt有[250000]个数
Split5.txt有[250000]个数
Split6.txt有[250000]个数
Split7.txt有[250000]个数
Split8.txt有[250000]个数
Split9.txt有[250000]个数
Split10.txt有[250000]个数
Split11.txt有[250000]个数
Split12.txt有[250000]个数
Split13.txt有[250000]个数
Split14.txt有[250000]个数
Split15.txt有[250000]个数
Split16.txt有[250000]个数
Split17.txt有[250000]个数
Split18.txt有[250000]个数
Split19.txt有[250000]个数
Split20.txt有[250000]个数
Split21.txt有[250000]个数
Split22.txt有[250000]个数
Split23.txt有[250000]个数
Split24.txt有[250000]个数
Split25.txt有[250000]个数
Split26.txt有[250000]个数
Split27.txt有[250000]个数
Split28.txt有[250000]个数
Split29.txt有[250000]个数
Split30.txt有[250000]个数
Split31.txt有[250000]个数
Split32.txt有[250000]个数
Split33.txt有[250000]个数
Split34.txt有[250000]个数
Split35.txt有[250000]个数
Split36.txt有[250000]个数
Split37.txt有[250000]个数
Split38.txt有[250000]个数
Split39.txt有[250000]个数
DONE

real	0m54.803s
user	0m53.856s
sys	0m0.756s

```

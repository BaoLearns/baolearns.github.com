---
layout: post
title: 某公司的面试题剖析
description: 删除字符串中的'a',将一个'b'变成两个连续'b'
category: blog
---

标签: 面试题

###题目描述:

给一个字符串,删除字符串中的'a'字符,并在'b'字符后增加一个'b'.如何在$O(1)$的空间复杂度,$O(N)$的时间复杂度下实现.注意:给的串保证变换后的字符串长度小于等于原字符串.

-------
###举例

- **input:** abceaacbaca     

- **ouput:** bbcecbbc


------
###分析

删除'a'字符时,可在原字符串st中就地操作,即:

对每一个字符st[i], i表示原字符下标,n表示删除'a'之后字符串的下标:

-  if(st[i] != 'a') st[n] = st[i]
- else 这个字符是'a',不用存储在新字符串中,即什么也不做

因为n增长的速度肯定不会快于i增长的速度,所以不会有存在新旧存储区域的冲突,这样的空间复杂度为$O(1)$,时间复杂度为$O(N)$.

接下来就是复制'b'字符,如果在从首字符开始扫描,在st[i]  (0<=i < length(st))遇到'b'字符,将i以后的所有字符向右移动一位,这样做的时间复杂度是$O(N^2)$,虽然空间复杂度仍是$O(1)$.

这里可以用到逆向思维,从后往前扫描.首先可已求得最终字符串的长度 new_Length = 原字符串长度 - 'a'的个数 + 'b'的个数*2.

i$(0<=i<length(st))$是删除'a'字符后的串的下标,n是最终字符串的下标$(0 <= n < new_Length)$.

- 若st[i]是'b',则st[n] = st[i], 并且st[n - 1] = st[i] (此步是复制一个'b')
- 否则只是st[n] = st[i].

复制'b'的时间复杂度降到$O(N)$.

综上所诉,总的空间复杂度为$O(1)$,空间复杂度为$O(N)$.

C语言的memcpy()函数,如果有同一空间的复制时,也是才采用从后往前复制,从而降低时间复杂度.

------
###参考代码

```
char *deleteAcopyB(char *st){
 
	int old_Length = strlen(st);//原字符串长度
	
	int deleteA_Length = 0; //删除'a'后的长度
	
	int number = 0; //'b'的个数
	
	for(int i = 0; i < old_Length; ++i){//删除'a'字符
	
		if(st[i] != 'a') st[deleteA_Length++] = st[i];
	
		if(st[i] == 'b') number++;//统计'b'的个数
	
	}
	
	int new_Length = deleteA_Length + number;//新字符串长度
	
	st[new_Length] = '\0';
	
	for(int i = deleteA_Length - 1, j = new_Length - 1; i >= 0; --i){//复制'b'字符
	
		st[j--] = st[i];
	
		if(st[i] == 'b') st[j--] = st[i];
	
	}
	
	return st;

}
```
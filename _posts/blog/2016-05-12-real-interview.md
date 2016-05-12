---
layout: post
title: 真实的笔试题
description: 博主的一道笔试题目,当时脑热,没写出来
category: blog
---

##Description

众所周知,操作系统中有一种作业调度算法`短作业优先(SJF)`,意思是在所有准备就绪的作业中,CPU每次选择一个作业`所需要执行时间最短的执行`.

现假设操作系统中有N作业需要执行,每个作业到达时间存记录在request_time[]数组中,为方便起见:到达时间均为整除,且第一个作业到达时间总为为0,到达时间升序给出.每个作业所需执行的时间存放在need_time[]数组中,现要求出作业的平均等待时间.

平均等待时间 = N个作业等待的时间和 / N

##Simple

###Input:

> N: 4

> request_time: 0 2 4 5

> need_time: 7 3 1 4

###Output:

> 4.000000

###Explain: 

1. j0最先执行,等待时间是0
2. 选择j2执行,等待时间是7-4=3
3. 选择j1执行,等待时间是8-2=6
4. 选择j3执行,等待时间是11-5=7
5. 总等待时间是0+3+6+7=16,所以平均等待时间是16/4=4.000000

##分析与思路

很简答的问题,用一个优先队列,将当前时间所有的任务加入优先队列,每次选择need_time少作业的优先执行即可.

博主当时脑热,没有做出来,原因是好久不做C/C++代码,导致priority_queue写不来了,⊙﹏⊙b汗.

###Reference Code:

```
/*
    Language:C/C++
    Author:Royecode
    Email:Royecode@163.com
    Date:2016-05-12
    Version:1.0
    Time complexity:O(NlogN)
*/

#include <iostream>
#include <cstdio>

#include <queue>

using namespace std;

const int MAXN = 100007;

//节点
typedef struct node{
	node(int _req, int _need){
		req = _req;
		need = _need;
	}
	//任务到达的时间,需要执行的时间
	int req;
	int need;
	//重载<
	friend bool operator <(const node &a, const node &b){
		if(a.need != b.need)
			return a.need > b.need;
		return a.req >= b.req;
	}
}node;

int main(){
	//申明变量与输入
	int n;
	int request_time[MAXN];
	int need_time[MAXN];
	scanf("%d", &n);
	for(int i = 0; i < n; ++i){
		scanf("%d", &request_time[i]);
	}
	for(int i = 0; i < n; ++i){
		scanf("%d", &need_time[i]);
	}
	
	//优先队列
	priority_queue<node> que;
	//当前时间
	int curr = 0;
	//下标
	int p = 0;
	//等待的总时间
	int total = 0;
	
	for(int i = 0 ; i < n; ++i){
		//将到达时间小于当前时间的加入优先队列
		while(p < n && request_time[p] <= curr){
			que.push(node(request_time[p], need_time[p]));
			p += 1;
		}
		//取出需要执行时间最少的任务
		node q = que.top();
		que.pop();
		printf("request_time%d | need_time%d\n", q.req, q.need);
		//计算此作业等待的时间
		total += curr - q.req;
		//计算当前时间
		curr += q.need;
	}
	//answer
	printf("%lf\n", total * 1.0 / n);
	
	//DONE
	return 0;
}

```

##与我联系
有任何问题请与我联系:Royecode@163.com .


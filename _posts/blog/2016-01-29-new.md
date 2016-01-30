---
layout:     post
title:      某公司的面试题
category: blog
description: 快排的好题目.
---
题目大意：利用快速排序的划分方法，把数组分成三部分：< val, = val, > val。即数组前半部分是小于val的，中间部分是等于val的，后半部分是大于val的。

首先看看快排的思想：在某一轮排序中，将指定的值val做为码，小于val的元素放在前面，大于等于val的元素放在后面。经过足够多的轮数排序，使数组变得有序。

现在来结合题目看看，如何用到快排的思想来完成这到题。
先用一轮快排去尝试一下结果，经过一轮排序之后，前半部分是肯定小于val的，这前半部分是符合题意的，无需再改动了。而后半部分是大于等于val的，但是后半部分大于val和等于val的元素是混排的，而题目的意思是等于val在中间，大于val在后半部分，这与题目不符。所以还得想想。

其实不难想到，小于val的部分不需要动了，直接将大于等于val的部分再次来排序。设大于等于val的区间[L, R]。这次选择val + 1作为码，区间为[L, R]，那么经过这次排序，小于val + 1（即等于val）的数会被排到[L, R]的前面，大于等于val +1的元素会被排到后面。

这样经过两轮快排，就会将原有数组划分为三部分：小于val， 等于val，大于val。

```
#include <iostream>

using namespace std;

void qsort(int *arr, int L, int R, int val, int cnt)
{//将数组arr的区间[L,R]，小于val划到前面，大于等于val划到后面，cnt是还需几轮排序
    int l = L, r = R;
    while(l < r)
    {
        while(l < r && arr[r] >= val) r--;
        while(l < r && arr[l] < val) l++;
        swap(arr[l], arr[r]);
    }
    if(cnt) qsort(arr, l, R, val + 1, cnt - 1);
    //当cnt大于1时继续
}

void show(int *arr, int n)
{//打印数组
    for(int i = 0; i < n; ++i)
        cout << arr[i] << (i < n - 1? " ": "\n");
}

int main(int argc, char **argv)
{
    int n;
    cin >> n;
    int arr[n];
    for(int i = 0; i < n; ++i) cin >> arr[i];
    int val;
    cin >> val;
    qsort(arr, 0, n - 1, val, 1);
    cout << "result:" << endl;
    show(arr, n);
    return 0;
}

```
这道题目用快排的做法可能还有其他做法，若你有更好的做法，欢迎讨论。

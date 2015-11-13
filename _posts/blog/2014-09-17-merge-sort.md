---
layout: post
title: 算法基础--归并排序
description: 归并算法是一个效率高且稳定的排序算法，但是比较占用内存。
category: blog
---

## 综述 ##

归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。

## 序列合并 ##

首先考虑下如何将将二个有序数列合并。这个非常简单，只要从比较二个数列的第一个数，谁小就先取谁，取了后就在对应数列中删除这个数。然后再进行比较，如果有数列为空，那直接将另一个数列的数据依次取出即可。

    //将有序数组a[]和b[]合并到c[]中
    void MemeryArray(int a[], int n, int b[], int m, int c[])
    {
    	int i, j, k;
    
    	i = j = k = 0;
    	while (i < n && j < m)
    	{
    		if (a[i] < b[j])
    			c[k++] = a[i++];
    		else
    			c[k++] = b[j++]; 
    	}
    
    	while (i < n)
    		c[k++] = a[i++];
    
    	while (j < m)
    		c[k++] = b[j++];
    }

可以看出合并有序数列的效率是比较高的，可以达到O(n)。

## 归并排序 ##
解决了上面的合并有序数列问题，再来看归并排序，其的基本思路就是将数组分成二组A，B，如果这二组组内的数据都是有序的，那么就可以很方便的将这二组数据进行排序。如何让这二组组内数据有序了？

可以将A，B组各自再分成二组。依次类推，当分出来的小组只有一个数据时，可以认为这个小组组内已经达到了有序，然后再合并相邻的二个小组就可以了。这样通过先递归的分解数列，再合并数列就完成了归并排序。


    //将有二个有序数列a[first...mid]和a[mid...last]合并。
    void mergearray(int a[], int first, int mid, int last, int temp[])
    {
    	int i = first, j = mid + 1;
    	int m = mid,   n = last;
    	int k = 0;
    	
    	while (i <= m && j <= n)
    	{
    		if (a[i] <= a[j])
    			temp[k++] = a[i++];
    		else
    			temp[k++] = a[j++];
    	}
    	
    	while (i <= m)
    		temp[k++] = a[i++];
    	
    	while (j <= n)
    		temp[k++] = a[j++];
    	
    	for (i = 0; i < k; i++)
    		a[first + i] = temp[i];
    }

    void mergesort(int a[], int first, int last, int temp[])
    {
    	if (first < last)
    	{
    		int mid = (first + last) / 2;
    		mergesort(a, first, mid, temp);//左边有序
    		mergesort(a, mid + 1, last, temp); //右边有序
    		mergearray(a, first, mid, last, temp); //再将二个有序数列合并
    	}
    }

    bool MergeSort(int a[], int n)
    {
    	int *p = new int[n];
    	if (p == NULL)
    		return false;
    	mergesort(a, 0, n - 1, p);
    	delete[] p;
    	return true;
    }

## 总结 ##

归并排序的效率是比较高的，设数列长为N，将数列分开成小数列一共要logN步，每步都是一个合并有序数列的过程，时间复杂度可以记为O(N)，故一共为O(N*logN)。因为归并排序每次都是在相邻的数据中进行操作，所以归并排序在O(N*logN)的几种排序方法（快速排序，归并排序，希尔排序，堆排序）也是效率比较高的。

## 实测 ##

在本人电脑上对冒泡排序，直接插入排序，归并排序及直接使用系统的qsort()进行比较（均在Release版本下）

----------

- 对20000个随机数据进行测试：
![a](/images/mergesort/2w.jpg)

- 对50000个随机数据进行测试：
![b](/images/mergesort/5w.jpg)

- 再对200000个随机数据进行测试：
![c](/images/mergesort/20w.jpg)

----------

注：有的书上是在mergearray()合并有序数列时分配临时数组，但是过多的new操作会非常费时。因此作了下小小的变化。只在MergeSort()中new一个临时数组。后面的操作都共用这一个临时数组。

## 原文 ##
[MoreWindows](http://blog.csdn.net/morewindows/article/details/6678165)

[RogerAce]:    http://lirenlong.github.io  "RogerAce"

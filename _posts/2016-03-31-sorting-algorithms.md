---
layout: post
title:  "排序算法（学习笔记）"
date:   2016-03-31
---

## 选择排序[^1]
首先，找到数组中最小的那个元素，其次，将它和数组的第一个元素交换位置。再次，在剩下的元素中找到最小的元素，将它与数组的第二个元素交换位置。对于长度为N的数组，选择排序需要大约N^2/2次比较和N次交换。运行时间和输入无关，一个已经有序的数组或是主键全部相等的数组和一个元素随机排列的数组所用的排序时间一样长。数据移动是最少的。

## 插入排序
为了给要插入的元素腾出空间，需要将其余所有元素在插入之前都向右移动一位。对于随机排列的长度为N且主键不重复的数组，平均情况下插入排序需要~N^2/4次比较及交换，最坏情况下需要~N^2/2次比较和交换，最好情况下需要N-1次比较和0次交换。插入排序对于部分有序的数组很有效。

## 希尔排序
基于插入排序，希尔排序的思想是使数组中任意间隔为h的元素都是有序的(使数组部分有序后再使用插入排序，即h=1)。只需要在插入排序的代码中将移动元素的距离由1变成h即可。

## 归并排序
它能够保证将任意长度为N的数组排序所需时间和NlgN成正比；它的主要缺点则是它所需的额外空间和N成正比。

### 原地归并的抽象方法：

```java
public static void merge(Comparable[] a, int lo, int mid, int hi){
    int i = lo;
    int j = mid + 1;
    for(int k = lo; k <= hi; k++){
        aux[k] = a[k]
    }
    for(int k = lo; k <= hi; k++){
        if(i > mid)                    a[k] = aux[j++];
        else if(j > hi)                a[k] = aux[i++];
        else if(less(aux[j],aux[i])    a[k] = aux[j++];
        else                           a[k] = aux[i++];
    }
}
```

### 自顶向下的归并排序：

```java
public class Merge{
    private static Comparable[] aux;
    public static void sort(Comparable[] a){
    aux = new Comparable[a.length];
    sort(a,0,a.length-1);
    }
    
    private static void sort(Comparable[] a, int lo, int hi){
        if(hi <= lo) return;
        int mid = lo + (hi - lo)/2;
        sort(a, lo, mid);
        sort(a, mid+1, hi);
        merge(a, lo, mid, hi);
    }
}
```

### 自底向上的归并排序：

```java
public class MergeBU{
    private static Comparable[] aux;
    public static void sort(Comparable[] a){
        int N = a.length;
        aux = new Comparable[N];
        for(int sz = 1; sz < N; sz = sz + sz)
            for(int lo = 0; lo < N-sz; lo += sz + sz)
                merge(a, lo, lo+sz-1, Math.min(lo+sz+sz-1, N-1));
    }
}
```

## 快速排序
特点包括原地排序（只需要一个很小的辅助栈），且将长度为N的数组排序所需的时间和NlgN成正比。该方法的关键在于切分，这个过程使得数组满足下面三个条件：

1. 对于某个j，a[j]已经排定；
2. a[lo] 到 a[j-1] 中的所有元素都不大于a[j];
3. a[j+1] 到 a[hi] 中的所有元素都不小于a[j]。

### 快速排序的切分：

```java
private static int partition(Comparable[] a, int lo, int hi){
    int i = lo;
    int j = hi + 1;
    Comparable v = a[lo];
    while(true){
        while(less(a[++i}, v)) if(i == hi) break;
        while(less(v,a[--j]))  if(j == lo) break;
        if(i >= j) break;
        exch(a,i,j);
    }
    exch(a, lo, j);
    return j;
}
```

### 算法改进：

1. 切换到插入排序（对于小数组，快速排序比插入排序慢）
2. 三取样切分（使用子数组的一小部分元素的中位数来切分数组）
3. 熵最优的排序（三向切分，对于包含大量重复元素的数组，它将排序时间从线性对数级降低到了线性级别）

### 三向切分的快速排序：

```java
public class Quick3way{
    private static void sort(Comparable[] a, int lo, int hi){
        if(hi <= lo) return;
        int lt = lo;
        int i = lo + 1;
        int gt = hi;
        Comparable v = a[lo];
        while(i <= gt){
            int cmp = a[i].compareTo(v);
            if (cmp < 0) exch(a, lt++, i++);
            else if (cmp > 0) exch(a, i, gt--);
            else i++;
        }
        sort(a, lo, lt - 1);
        sort(a, gt + 1, hi);
    }
}
```

## 优先队列
优先队列最重要的操作就是删除最大元素和插入元素。一种实现优先队列的经典方法基于二叉堆数据结构。

> 定义1：当一棵二叉树的每个结点都大于等于它的两个子结点时，它被称为堆有序。
> 
> 定义2：二叉堆是一组能够用堆有序的完全二叉树排序的元素，并在数组中按照层级储存（不使用数组的第一个位置）。

在一个二叉堆中，位置k的结点的父结点的位置为k/2(向下取整)，而它的两个子结点的位置则分别为2k和2k+1。

### 由下至上的堆有序化（上浮）
如果堆的有序状态因为某个结点变得比它的父结点更大而被打破，那么我们就需要通过交换它和它的父结点来修复堆。

```java
private void swim(int k){
    while(k > 1 && less(k/2, k)){
        exch(k/2, k);
        k = k/2;
    }
}
```

### 由上至下的堆有序化（下沉）
如果堆的有序状态因为某个结点变得比它的两个子结点或是其中之一更小了而被打破了，那么我们可以通过将它和它的两个子结点中的较大者交换来恢复堆。

```java
private void sink(int k){
    while(2*k <= N){
        int j = 2 * k;
        if(j < N && less(j, j+1)) j++;
        if(!less(k, j)) break;
        exch(k, j);
        k = j;
    }
}
```

### 基于堆的优先队列

```java
public class MaxPQ<Key extends Comparable<Key>>{
    private Key[] pq;
    private int N = 0;
    public MaxPQ(int maxN){
        pq = (Key[]) new Comparable[maxN+1];
    }
    public boolean isEmpty(){
        return N == 0;
    }
    public int size(){
        return N;
    }
    public void insert(Key v){
        pq[++N] = v;
        swim(N);
    }
    public key delMax(){
        Key max = pq[1];
        exch(1,N--);
        pq[N+1] = null;
        sink(1);
        return max;
    }
}
```

对于一个含有N个元素的基于堆的优先队列，插入元素操作只需要不超过(lgN +1)次比较，删除最大元素的操作需要不超过2lgN次比较。

## 堆排序：

```java
public static void sort(Comparable[] a){
    int N = a.length;
    //构造堆
    for(int k = N/2; k >= 1; k--){
        sink(a, k, N);
    }
    //下沉排序
    while(N > 1){
        exch(a, 1, N--);
        sink(a, 1, N);
    }
}
```

## Reference:
[^1]: Algorithms 4th Edition, by Robert Sedgewick and Kevin Wayne.

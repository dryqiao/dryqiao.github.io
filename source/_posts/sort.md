---
title: 排序算法总结

date: 2018/05/02 16:02:03
categories: js
---

# 排序算法总结
数据结构必学的内容，现在用js重新实现一遍，加深印象
这里写了个算法demo，如果觉得本篇太长可以看这个：[算法demo](http://dryqiao.com/sort)
##### 复杂度总结
![](http://owicv5j2l.bkt.clouddn.com/sort.jpg)
# 冒泡排序

排序算法的入门，应该都很熟悉吧，冒泡排序需要两个嵌套的循环， 其中, 外层循环移动游标，内层循环遍历游标及之后(或之前)的元素, 通过两两交换的方式, 每次只确保该内循环结束位置排序正确, 然后内层循环周期结束, 交由外层循环往后(或前)移动游标, 随即开始下一轮内层循环, 以此类推, 直至循环结束。
每次移动有一个值处于正确的位置，且不改变相同元素的位置，是稳定的算法。

##### 复杂度

| 平均时间复杂度  | 最好情况    |  最坏情况  |    空间复杂度
| --------       | -----      | :----:    |     :----:    
| O(n²)      | O(n²)  |   O(n²)   |  O(n²)
##### 实现图解
    
![a](http://owicv5j2l.bkt.clouddn.com/fdce56a6-fbb6-11e6-9cfd-3f82b9667b94.png)

##### 实现代码
```
    bubbleSort(arr) {
        if (arr.length <= 1) return arr
        for (let i = 0; i < arr.length - 1; i++) {
            for (let j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    let temp = arr[j]
                    arr[j] = arr[j + 1]
                    arr[j + 1] = temp
                }
            }
        }
        return arr
    }
```

# 插入排序
插入排序的设计初衷是往有序的数组中快速插入一个新的元素。它的算法思想是: 把要排序的数组分为两个部分, 一部分是数组的全部元素(除去待插入的元素), 另一部分是待插入的元素; 先将第一部分排序完成, 然后再插入这个元素。 其中第一部分的排序也是通过再次拆分为两部分来进行的。

######### 复杂度
| 平均时间复杂度  | 最好情况    |  最坏情况  |    空间复杂度
| --------       | -----      | :----:    |     :----:    
| O(n²)      | O(n)  |   O(n²)   |  O(1)

######## 实现图解
![](http://owicv5j2l.bkt.clouddn.com/TIM%E5%9B%BE%E7%89%8720180427161231.png)

#### 实现代码
```
    insertSort(arr) {
        for (let i = 1; i < arr.length; i++) {
            let tmp = arr[i]
            let j = i - 1
            while(arr[j] > tmp && j >= 0) {
                arr[j + 1] = arr[j]
                j--
            }
            arr[j + 1] = tmp
        }
        return arr
    },
```

# 快速排序

> 快速排序是对冒泡排序的一种改进。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一不部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

##### 复杂度
| 平均时间复杂度  | 最好情况    |  最坏情况  |    空间复杂度
| --------       | -----      | :----:    |     :----:    
| O(nlog₂n)      | O(nlog₂n)  |   O(n²)   |  O(nlog₂n)

整个排序过程只需要三步：
1. 在数据集之中，选择一个元素作为"基准"（pivot）。
2. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
3. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

##### 实现图解:

![a](http://owicv5j2l.bkt.clouddn.com/quicksort.png)

##### 实现代码:
```
    quickSort(arr) {
        if(arr.length <= 1) return arr
        let right = [],left = [],base = arr.splice(Math.floor(arr.length / 2),1)[0]
        for(let i = 0 ; i < arr.length ; i++) {
            if(arr[i] < base) {
                left.push(arr[i])
            }else {
                right.push(arr[i])
            }
        }
    　　return this.quickSort(left).concat([base], this.quickSort(right))
    }
```

# 归并排序

归并排序是建立在归并操作上的一种有效的排序算法,该算法是采用分治法的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。

分的过程很像一棵二叉树，所以时间复杂度为o(log2n)。治的过程是合并两个数组，取两个数组的第一个值比较，较小的放进去，所以时间复杂度为o(n)

##### 复杂度
| 平均时间复杂度  | 最好情况    |  最坏情况  |    空间复杂度
| --------       | -----      | :----:    |     :----:    
| O(nlogn)      | O(nlogn)  |   O(nlogn)   |  O(1)

##### 实现图解

![mergesort](http://owicv5j2l.bkt.clouddn.com/mergeSort.png)


##### 实现代码
```
mergeSort(arr) {
    if (arr.length <= 1) return arr
    let m = arr.length >> 1,
        left = arr.slice(0, m)
        right = arr.slice(m)
    return this.merge(this.mergeSort(left), this.mergeSort(right))
},
merge(left, right) {
    let res = []
    while(left.length && right.length) {
        res.push(left[0] > right[0] ? right.shift() : left.shift())
    }
    return res.concat(left.length ? left : right)
},
```

# 堆排序

堆排序是利用堆这种数据结构而设计的一种排序算法，是一种选择排序，它的最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序。

堆实际上是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆；或者每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。

大顶堆：

![bigheap](http://owicv5j2l.bkt.clouddn.com/bigheap.png)

小顶堆：

![smallheap](http://owicv5j2l.bkt.clouddn.com/smallheap.png)



由上面的介绍我们可以看出堆的第一个元素最大值或最小值，我们利用堆的这个性质，在排序的时候，直接将第一个元素和最后一个元素进行交换，然后从第一个元素开始进行向下调整至第n-1个元素，就形成了一个有序数列。
堆排序的步骤分为三步: 
1. 建堆（升序建大堆，降序建小堆）
2. 交换数据
3. 向下调整

##### 复杂度
| 平均时间复杂度  | 最好情况    |  最坏情况  |    空间复杂度
| --------       | -----      | :----:    |     :----:    
| O(nlogn)      | O(nlogn)  |   O(n²)   |  O(1)

##### 实现图解
1. 初始化堆

    ![init](http://owicv5j2l.bkt.clouddn.com/initheap.png)
2. 创建大顶堆,从第一个非叶子节点开始，往上遍历，保证每个非叶子节点比它的叶子节点值大

    ![big](http://owicv5j2l.bkt.clouddn.com/initheap1.png)
    =>
    ![big1](http://owicv5j2l.bkt.clouddn.com/initheap2.png)

3. 将堆顶元素和末尾元素交换，使末尾元素最大，然后继续往上调整。这时候堆其实分成了两块，无序堆(1到n-1)和有序堆(n)

    ![](http://owicv5j2l.bkt.clouddn.com/initheap3.png)
    =>
    ![](http://owicv5j2l.bkt.clouddn.com/initheap4.png)

4. 重复步骤3,完成堆排序

    ![](http://owicv5j2l.bkt.clouddn.com/initheap5.png)

##### 实现代码

```
    heapSort(arr) {
        let len = arr.length

        if (len <= 1) return arr
        //init
        for(let i = Math.floor(len / 2 - 1); i >= 0; i--) {
            this.heapAdjust(arr, i , len)
        }
        for(let i =len - 1; i > 0; i--) {
            this.swap(0, i, arr)
            this.heapAdjust(arr, 0 , i)
        }
        return arr
    },
    heapAdjust(arr, i, len) {
        let left = 2 * i + 1,
            right = 2 * i + 2,
            largest = i
        if(left < len && arr[left] > arr[largest]) {
            largest = left
        }
        if(right < len && arr[right] > arr[largest]) {
            largest = right
        }
        if(largest != i) {
            this.swap(i, largest, arr)
            this.heapAdjust(arr, largest, len)
        }
    },
    swap(i, j, arr) {
        [arr[i], arr[j]] = [arr[j], arr[i]] 
    },
```
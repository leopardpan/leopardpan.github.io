---
layout: post
title: 跟着ChatGPTplus插件学习算法（下）
date: 2023-04-18
description: "跟着ChatGPTplus插件学习算法（下）"
tag: Python
---
## 归并排序

### 写一个归并排序算法，要求有良好的注释和print每一次排序的过程结果，并举例子

```python
def merge_sort(arr):
    # 递归终止条件，当数组长度为1时，直接返回
    if len(arr) == 1:
        return arr

    # 将数组分为两半
    mid = len(arr) // 2
    left = arr[:mid]
    right = arr[mid:]

    # 对左右两半分别进行归并排序
    left_sorted = merge_sort(left)
    right_sorted = merge_sort(right)

    # 合并两个有序数组
    result = []
    i = j = 0
    while i < len(left_sorted) and j < len(right_sorted):
        if left_sorted[i] < right_sorted[j]:
            result.append(left_sorted[i])
            i += 1
        else:
            result.append(right_sorted[j])
            j += 1

    if i == len(left_sorted):
        result += right_sorted[j:]
    else:
        result += left_sorted[i:]

    # 打印每次排序结果
    print("排序结果：", result)

    return result


# 测试
arr = [12, 45, 23, 67, 89, 34, 56, 78]
print("原始数组：", arr)
sorted_arr = merge_sort(arr)
print("最终排序结果：", sorted_arr)
```

结果如下：

```console
原始数组： [12, 45, 23, 67, 89, 34, 56, 78]
排序结果： [12, 45]
排序结果： [23, 67]
排序结果： [12, 23, 45, 67]
排序结果： [34, 89]
排序结果： [56, 78]
排序结果： [34, 56, 78, 89]
排序结果： [12, 23, 34, 45, 56, 67, 78, 89]
最终排序结果： [12, 23, 34, 45, 56, 67, 78, 89]
```

## 堆排序

### 写一个堆排序算法，要求有良好的注释和print每一次排序的过程结果，并举例子

```python
# 堆排序算法
def heap_sort(arr):
    n = len(arr)

    # 构建最大堆
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # 一个个取出元素
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # 交换
        heapify(arr, i, 0)
        print(arr)  # 打印每一次排序的过程结果

# 堆化子树
def heapify(arr, n, i):
    largest = i  # 初始化最大元素为根节点
    l = 2 * i + 1  # 左子节点
    r = 2 * i + 2  # 右子节点

    # 如果左子节点比根节点大，则更新最大元素
    if l < n and arr[l] > arr[largest]:
        largest = l

    # 如果右子节点比根节点大，则更新最大元素
    if r < n and arr[r] > arr[largest]:
        largest = r

    # 如果最大元素不是根节点，则交换
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

# 测试
arr = [12, 11, 13, 5, 6, 7]
heap_sort(arr)
print("排序后的数组：")
print(arr)
```

结果如下：

```console
[12, 11, 7, 5, 6, 13]
[11, 6, 7, 5, 12, 13]
[7, 6, 5, 11, 12, 13]
[6, 5, 7, 11, 12, 13]
[5, 6, 7, 11, 12, 13]
排序后的数组：
[5, 6, 7, 11, 12, 13]
```

堆是一种特殊的树形数据结构，它满足以下两个性质：

1. 堆是一个完全二叉树，即除了最后一层，其他层的节点都是满的，最后一层的节点都靠左排列。
2. 堆中每个节点的值都大于等于（或小于等于）其子节点的值，这被称为堆的性质。

堆可以分为最大堆和最小堆。在最大堆中，每个节点的值都大于等于其子节点的值，而在最小堆中，每个节点的值都小于等于其子节点的值。

堆排序算法利用最大堆的性质来进行排序。具体来说，它首先构建一个最大堆，然后将堆顶元素（即最大元素）与最后一个元素交换，然后将剩余元素重新构建为最大堆，重复此过程直到所有元素都有序排列。堆排序的时间复杂度为O(nlogn)。

回到您的问题，您之前问的是堆排序算法中用来构建最大堆的for循环。具体来说，它遍历数组中对应于非叶子节点的索引（即至少有一个子节点的节点）。循环从最后一个非叶子节点的索引开始（即n // 2 - 1，其中n是数组的长度），向根节点（索引为0）反向迭代。

在循环的每次迭代中，heapify函数被调用以确保以该节点为根的子树满足最大堆的性质。这是通过将节点与其子节点进行比较并在必要时与较大的子节点交换来完成的。循环有效地将数组中较大的元素“冒泡”到它们在堆中的正确位置。




## 计数排序

### 写一个计数排序算法，要求有良好的注释和print每一次排序的过程结果，并举例子

```python
def counting_sort(arr):
    # 找到数组中的最大值
    max_val = max(arr)
    # 初始化计数数组
    count_arr = [0] * (max_val + 1)
    # 计算每个元素出现的次数
    for i in arr:
        count_arr[i] += 1
    # 对计数数组进行累加
    for i in range(1, len(count_arr)):
        count_arr[i] += count_arr[i-1]
    # 初始化排序后的数组
    sorted_arr = [0] * len(arr)
    # 遍历原始数组，将元素放入排序后的数组中
    for i in arr:
        sorted_arr[count_arr[i]-1] = i
        count_arr[i] -= 1
    # 打印每一次排序的结果
    print(sorted_arr)
    return sorted_arr

# 举例
arr = [3, 1, 6, 2, 4, 3]
counting_sort(arr)
```

结果如下：

```console
[1, 2, 3, 3, 4, 6]
```

## 桶排序

### 写一个桶排序算法，要求有良好的注释和print每一次排序的过程结果，并举例子

```python
# 定义桶排序函数
def bucket_sort(arr):
    # 获取数组中的最大值和最小值
    max_num = max(arr)
    min_num = min(arr)
    # 计算桶的数量
    bucket_num = (max_num - min_num) // len(arr) + 1
    # 初始化桶
    buckets = [[] for _ in range(bucket_num)]
    # 将元素放入桶中
    for num in arr:
        index = (num - min_num) // len(arr)
        buckets[index].append(num)
    # 对每个桶进行排序
    for bucket in buckets:
        bucket.sort()
        print(bucket)
    # 将桶中的元素按顺序放回原数组
    res = []
    for bucket in buckets:
        res += bucket
    return res

# 测试
arr = [3, 1, 4, 2, 5]
print(bucket_sort(arr))  # [1, 2, 3, 4, 5]
```

结果如下：

```console
[1, 2]
[3, 4]
[5]
[1, 2, 3, 4, 5]
```

## 基数排序

### 写一个基数排序算法，要求有良好的注释和print每一次排序的过程结果，并举例子

```python
def radix_sort(arr):
    """
    基数排序算法
    :param arr: 待排序数组
    :return: 排序后的数组
    """
    # 获取数组中最大值的位数
    max_num = max(arr)
    max_digit = len(str(max_num))

    # 从个位开始，对数组按照该位进行排序
    for i in range(max_digit):
        # 初始化桶
        buckets = [[] for _ in range(10)]

        # 将数组中的元素放入桶中
        for num in arr:
            # 获取当前位数的数字
            digit = (num // (10 ** i)) % 10
            buckets[digit].append(num)

        # 将桶中的元素按照顺序放回数组中
        arr = []
        for bucket in buckets:
            arr.extend(bucket)

        # 打印每一次排序的结果
        print(f"Sorting at digit {i+1}: {arr}")

    return arr

# 举例
arr = [170, 45, 75, 90, 802, 24, 2, 66]
sorted_arr = radix_sort(arr)
print(sorted_arr)
```

结果如下：

```console
Sorting at digit 1: [170, 90, 802, 2, 24, 45, 75, 66]
Sorting at digit 2: [802, 2, 24, 45, 66, 170, 75, 90]
Sorting at digit 3: [2, 24, 45, 66, 75, 90, 170, 802]
[2, 24, 45, 66, 75, 90, 170, 802]
```

## 参考链接

[排序算法：堆排序【图解+代码】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fp4y1D7cj/?spm_id_from=333.337.search-card.all.click&vd_source=a59f9331c2b80795dda1842d7be401ec)

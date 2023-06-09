# 冒泡排序算法

<!-- toc -->

冒泡排序（Bubble Sort）是一种简单直观的排序算法，它的思想就是重复地交换相邻两个元素，直到所有的元素都排好序为止。

以下是基于C语言的冒泡排序代码示例：

```c
void bubbleSort(int arr[]) {
    if (sizeof(arr[0]) == 0) {
        return;
    }
    int stop = sizeof(arr) / sizeof(arr[0]) - 1;
    for (int i = 0; i < stop; i++) {
        // 每次循环确定一个最大值，然后数组不停缩减，大的不停往上爬o
        for (int j = 0; j < stop - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

这个算法的时间复杂度为$O(n^2)$，其中n是待排序序列的长度。需要注意的是，虽然冒泡排序的时间复杂度较高，但是它的空间复杂度只有$O(1)$，因为它是在原地排序，不需要额外的空间。

> * 时间复杂度
>
>   * 平均时间复杂度：$O(n^{2})$
>   * 最坏时间复杂度：$O(n^{2})$
>   * 最好时间复杂度：$O(n)$
>
> * 空间复杂度
>
>   $O({1})$

# 快速排序算法

快速排序（Quick Sort）是一种常用的排序算法，它的基本思想是通过划分把一个数组（或称为列表）分成两个子数组，然后递归地对子数组进行排序。

以下是基于C语言的快速排序代码示例：

```c
void swap(int *p, int *q) {
    int temp;
    temp = *p;
    *p = *q;
    *q = temp;
}

void quickSort(int arr[], int i, int j) {
    if (low < high) {
        int low = i, high = j, pivot = arr[i];
        while (low < high) {  // 该循环结束代表比较了一轮
            while (low < high && pivot <= arr[high]) {
                high -= 1;  // 本high索引的比pivot大，保留在大这，同时-1，准备下一个high
            }
            if (pivot > arr[high]) {
                swap(&a[low], &a[high]);  // 本high索引的比pivot小，换到小那边
                low += 1;    // 小的这个索引被填了，要+1准备给下一个填
            }
            while (low < high && pivot >= arr[low]) {
                low += 1;   // 本low索引的比pivot小，保留在小这，同时+1，准备下一个low
            }
            if (pivot < arr[low]) {
                swap(&a[low], &a[high]);  // 本low索引的比pivot大，换到大那边
                high -= 1;     // 大的这个索引被填了，要+1给下一个填
            }
        }
        arr[i] = pivot;
        quickSort(arr, low, i - 1);
        quickSort(arr, i + 1, high);
    }
}
```

这个算法的时间复杂度为$O(n*logn)$，其中$n$是待排序序列的长度。由于快速排序采用的是分治法的思想，其可以在适当情况下使用多线程实现多核并行，从而提高运算效率。需要注意的是，快速排序的最坏时间复杂度为$O(n^2)$，由于枢轴元素的选取方法是随意的，因此不同的选取方法可能会导致快速排序的性能表现不同。

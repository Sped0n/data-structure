# 二分查找

二分查找（也称作折半查找）是一种常用的查找算法，其时间复杂度为O(log n)。其主要思想是通过将查找区间的中间位置与目标值进行比较，从而将查找区间逐渐缩小，直到找到目标值或者确定不存在。

以下是基于C语言实现的二分查找算法过程：

```c
int binary_search(int arr[], int x) {
    if (sizeof(arr[0]) == 0) {
        return -1;
    }
    int end = sizeof(arr) / sizeof(arr[0]) - 1;
    int start = 0;
    while (start <= end) {
        int mid = (start + end) / 2;
        if (arr[mid] == x) {
            return mid;
        } else if (arr[mid] < x) {
            start = mid + 1;
        } else {
            end = mid - 1;
        }
    }
    return -1; // 如果目标值不存在，则返回-1
}
```

在这个算法中，我们使用了一个while循环来不断缩小查找区间。在每次循环时，我们通过计算中间位置来确定当前查找区间的中间值mid。接着，我们将mid与目标值进行比较，如果mid等于目标值，则说明我们已经找到了目标值，直接返回mid。否则，如果mid小于目标值，则说明目标值可能在mid的右边，因此我们将查找区间的起始位置修改为mid+1。如果mid大于目标值，则说明目标值可能在mid的左边，因此我们将查找区间的结束位置修改为mid-1。每次循环过后，我们都会根据目标值的大小，缩小查找区间，直到找到目标值或者确定不存在。

请注意：该算法要求查找的数组必须是有序的，否则可能会找不到目标值或者产生错误结果。

# 顺序查找

<!-- toc -->

基于C语言的顺序查找算法如下：

```c
int search(int arr[], int x)
{
    if (sizeof(arr[0]) == 0) {
        return -1;             // 数组为空
    }
    int n = sizeof(arr) / sizeof(arr[0]);
    for(int i = 0; i < n; i++)
    {
        if(arr[i] == x)
            return i;
    }
    return -1;
}
```

其中，`arr`是指要进行查找的数组，`n`是数组的长度，`x`是要查找的值。该算法是通过遍历整个数组来查找指定的值。如果数组中不存在该值，或者数组为空，则返回-1。

算法复杂度：

- 最好情况：要查找的值在数组的第一个位置，时间复杂度为$O(1)$。
- 最坏情况：要查找的值在数组的最后一个位置或者不存在于数组中，时间复杂度为$O(n)$。
- 平均情况：要查找的值等概率地出现在数组的任意位置，时间复杂度为$O(n/2)$。

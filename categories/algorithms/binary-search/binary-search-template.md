# 二分查找算法模板

二分查找是一种在**有序数组**中查找特定元素的高效算法，时间复杂度为 O(log n)。

## 基础二分查找模板

```java
public int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // 防止整数溢出
        
        if (nums[mid] == target) {
            return mid;  // 找到目标，返回索引
        } else if (nums[mid] < target) {
            left = mid + 1;  // 目标在右半部分
        } else {
            right = mid - 1;  // 目标在左半部分
        }
    }
    
    return -1;  // 未找到目标
}

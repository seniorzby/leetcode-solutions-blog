# 二分查找算法与经典题目

## 二分查找模板

```java
// 标准二分查找
int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}

// 查找左边界
int binarySearchLeftBound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    if (left < nums.length && nums[left] == target) {
        return left;
    }
    return -1;
}

// 查找右边界
int binarySearchRightBound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    if (right >= 0 && nums[right] == target) {
        return right;
    }
    return -1;
}
```

## LeetCode 704: 二分查找

### 题目描述
给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

### 伪代码

```
函数 二分查找(数组, 目标值):
    左指针 = 0
    右指针 = 数组长度 - 1
    
    当 左指针 <= 右指针:
        中间位置 = (左指针 + 右指针) / 2
        
        如果 数组[中间位置] == 目标值:
            返回 中间位置
        否则，如果 数组[中间位置] < 目标值:
            左指针 = 中间位置 + 1
        否则:
            右指针 = 中间位置 - 1
    
    返回 -1  // 未找到
```

### 核心思想
- 利用数组有序的特性，每次将搜索范围缩小一半
- 通过比较中间元素与目标值的大小，决定在左半部分还是右半部分继续搜索
- 时间复杂度为 O(log n)

## LeetCode 33: 搜索旋转排序数组

### 题目描述
假设按照升序排序的数组在预先未知的某个点上进行了旋转。例如，[0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2]。搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1。

### 伪代码

```
函数 搜索旋转数组(数组, 目标值):
    左指针 = 0
    右指针 = 数组长度 - 1
    
    当 左指针 <= 右指针:
        中间位置 = (左指针 + 右指针) / 2
        
        如果 数组[中间位置] == 目标值:
            返回 中间位置
        
        // 判断哪部分是有序的
        如果 数组[左指针] <= 数组[中间位置]:  // 左半部分有序
            如果 数组[左指针] <= 目标值 < 数组[中间位置]:  // 目标在左半有序部分
                右指针 = 中间位置 - 1
            否则:  // 目标在右半部分
                左指针 = 中间位置 + 1
        否则:  // 右半部分有序
            如果 数组[中间位置] < 目标值 <= 数组[右指针]:  // 目标在右半有序部分
                左指针 = 中间位置 + 1
            否则:  // 目标在左半部分
                右指针 = 中间位置 - 1
    
    返回 -1  // 未找到
```

### 核心思想
- 关键点是识别哪半部分是有序的
- 在有序部分中，可以直接判断目标值是否在范围内
- 每次仍然将搜索范围缩小一半
- 时间复杂度依然是 O(log n)

## LeetCode 34: 在排序数组中查找元素的第一个和最后一个位置

### 题目描述
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。如果数组中不存在目标值，返回 [-1, -1]。

### 伪代码

```
函数 查找元素范围(数组, 目标值):
    // 查找左边界
    左边界 = 查找左边界(数组, 目标值)
    
    如果 左边界 == -1:  // 目标不存在
        返回 [-1, -1]
    
    // 查找右边界
    右边界 = 查找右边界(数组, 目标值)
    
    返回 [左边界, 右边界]

函数 查找左边界(数组, 目标值):
    左指针 = 0
    右指针 = 数组长度 - 1
    
    当 左指针 <= 右指针:
        中间位置 = (左指针 + 右指针) / 2
        
        如果 数组[中间位置] < 目标值:
            左指针 = 中间位置 + 1
        否则:  // 数组[中间位置] >= 目标值
            右指针 = 中间位置 - 1
    
    如果 左指针 < 数组长度 且 数组[左指针] == 目标值:
        返回 左指针
    返回 -1  // 未找到

函数 查找右边界(数组, 目标值):
    左指针 = 0
    右指针 = 数组长度 - 1
    
    当 左指针 <= 右指针:
        中间位置 = (左指针 + 右指针) / 2
        
        如果 数组[中间位置] <= 目标值:
            左指针 = 中间位置 + 1
        否则:  // 数组[中间位置] > 目标值
            右指针 = 中间位置 - 1
    
    如果 右指针 >= 0 且 数组[右指针] == 目标值:
        返回 右指针
    返回 -1  // 未找到
```

### 核心思想
- 分别寻找目标值的左边界和右边界
- 左边界：第一个等于目标值的位置
- 右边界：最后一个等于目标值的位置
- 两次二分查找，时间复杂度为 O(log n)

## LeetCode 35: 搜索插入位置

### 题目描述
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

### 伪代码

```
函数 搜索插入位置(数组, 目标值):
    左指针 = 0
    右指针 = 数组长度 - 1
    
    当 左指针 <= 右指针:
        中间位置 = (左指针 + 右指针) / 2
        
        如果 数组[中间位置] == 目标值:
            返回 中间位置
        否则，如果 数组[中间位置] < 目标值:
            左指针 = 中间位置 + 1
        否则:
            右指针 = 中间位置 - 1
    
    返回 左指针  // 插入位置
```

### 核心思想
- 本质上是寻找第一个大于等于目标值的位置
- 当循环结束时，左指针指向的位置就是第一个大于等于目标值的位置
- 如果目标值大于所有元素，左指针将等于数组长度，正好是插入位置
- 时间复杂度为 O(log n)

## 二分查找的共同特点

1. **前提条件**：搜索空间有序或部分有序
2. **核心思想**：通过比较中间元素来缩小搜索范围
3. **时间复杂度**：O(log n)，这是二分查找的主要优势
4. **实现细节**：
   - 循环条件：通常为 `left <= right`
   - 中间位置计算：`mid = left + (right - left) / 2` 防止整数溢出
   - 边界更新：确保搜索范围不断缩小
   - 循环结束后的处理：根据题目要求可能需要额外检查

二分查找的关键在于理解"搜索空间"的概念，以及如何通过比较缩小这个空间。不同的二分查找变体（如寻找左边界、右边界）本质上是在维护不同的循环不变量。

# 二分查找算法变体详解：LeetCode 33、34、35、704、153题分析

二分查找是一种高效的搜索算法，时间复杂度为 O(log n)。然而，在不同类型的问题中，二分查找的具体实现细节会有所不同。本文将通过分析 LeetCode 上的几道经典二分查找题目，解释这些差异的本质原因，并提供如何根据具体问题选择正确二分查找变体的思路。

## 二分查找的核心要素

在分析不同变体前，我们需要明确二分查找的三个核心要素：

1. **循环终止条件**：`while (left <= right)` 还是 `while (left < right)`
2. **边界更新方式**：`left = mid + 1`/`right = mid - 1` 还是 `left = mid`/`right = mid`
3. **返回值处理**：循环结束时应返回什么

这些细节的选择取决于具体问题的性质。

## 经典二分查找问题分析

### LeetCode 704: 二分查找

这是最基础的二分查找问题，查找目标值在有序数组中的位置。

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

**关键特点**：
- 使用 `while (left <= right)`，因为我们需要检查 `left == right` 的情况
- 当 `nums[mid] < target` 时，可以确定中间值不是目标，使用 `left = mid + 1`
- 当 `nums[mid] > target` 时，可以确定中间值不是目标，使用 `right = mid - 1`
- 如果找不到目标值，返回 -1

### LeetCode 153: 寻找旋转排序数组中的最小值

```java
public int findMin(int[] nums) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return nums[left];
}
```

**关键特点**：
- 使用 `while (left < right)`，因为我们知道最小值一定存在
- 当 `nums[mid] > nums[right]` 时，最小值在右半部分，且不可能是 mid，使用 `left = mid + 1`
- 当 `nums[mid] <= nums[right]` 时，最小值在左半部分（可能是 mid 本身），使用 `right = mid`
- 循环结束时，`left == right`，指向最小值

### LeetCode 33: 搜索旋转排序数组

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        }
        
        if (nums[left] <= nums[mid]) {  // 左半部分有序
            if (target >= nums[left] && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {  // 右半部分有序
            if (target > nums[mid] && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

**关键特点**：
- 使用 `while (left <= right)`，因为我们要找具体值
- 首先判断 mid 在哪个有序部分，然后根据 target 是否在有序部分内决定搜索区间
- 边界更新与标准二分查找相同，因为我们可以确定 mid 不是目标（除非 `nums[mid] == target`）

### LeetCode 34: 在排序数组中查找元素的第一个和最后一个位置

这道题可以分解为两次二分查找：找第一个等于目标值的位置，和找第一个大于目标值的位置减一。

```java
public int[] searchRange(int[] nums, int target) {
    int[] result = {-1, -1};
    
    // 找第一个等于target的位置
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] >= target) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    
    // 检查是否找到了target
    if (left < nums.length && nums[left] == target) {
        result[0] = left;
        
        // 找最后一个等于target的位置
        right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        result[1] = right;
    }
    
    return result;
}
```

**关键特点**：
- 两次二分查找使用略有不同的条件
- 第一次查找中，当 `nums[mid] == target` 时，继续向左搜索，确保找到第一个位置
- 第二次查找中，当 `nums[mid] == target` 时，继续向右搜索，确保找到最后一个位置

### LeetCode 35: 搜索插入位置

```java
public int searchInsert(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return left;
}
```

**关键特点**：
- 使用 `while (left <= right)`，与标准二分查找相同
- 如果找不到目标值，返回 `left`，这正好是插入位置
- 循环结束时，`left` 是第一个大于等于目标值的位置

## 如何选择正确的二分查找变体

选择正确的二分查找变体本质上取决于三个关键问题：

### 1. 我在寻找什么？

- **具体值**（704、33题）：使用 `while (left <= right)`
- **特殊位置**（153题）：使用 `while (left < right)`
- **第一个/最后一个满足条件的位置**（34题）：通常需要特殊处理边界

### 2. 比较后，我能100%排除哪部分？

这决定了如何更新左右边界：

- 如果中间值**肯定不是答案**：使用 `left = mid + 1` 或 `right = mid - 1`
- 如果中间值**可能是答案**：使用 `left = mid` 或 `right = mid`

例如：
- 在153题中，当 `nums[mid] > nums[right]` 时，mid 肯定不是最小值，可以用 `left = mid + 1`
- 当 `nums[mid] <= nums[right]` 时，mid 可能是最小值，必须用 `right = mid`

### 3. 循环结束时，指针指向什么？

这决定了如何处理返回值：

- 在标准二分查找中，如果未找到目标，返回 `-1`
- 在153题中，循环结束时 `left == right`，指向最小值
- 在35题中，循环结束时 `left` 指向插入位置

## 常见二分查找变体模板

### 寻找具体值

```java
while (left <= right) {
    mid = left + (right - left) / 2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) left = mid + 1;
    else right = mid - 1;
}
return -1;  // 未找到
```

### 寻找第一个满足条件的位置

```java
while (left < right) {
    mid = left + (right - left) / 2;
    if (条件成立) right = mid;
    else left = mid + 1;
}
return left;  // 可能需要额外检查
```

### 寻找最后一个满足条件的位置

```java
while (left < right) {
    mid = left + (right - left + 1) / 2;  // 上取整
    if (条件成立) left = mid;
    else right = mid - 1;
}
return left;  // 可能需要额外检查
```

## 总结

二分查找虽然概念简单，但细节变化多样。要选择正确的二分查找变体，关键在于明确：

1. 你要找的具体是什么
2. 比较后，哪部分可以安全排除
3. 循环结束时，指针应该在什么位置

掌握这三点，你就能灵活应对各种二分查找问题，根据具体题目特点选择正确的算法变体。这也是为什么不同题目中二分查找的细节会有所不同的本质原因。

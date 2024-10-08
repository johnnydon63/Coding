# 数组与矩阵
<!-- GFM-TOC -->
* [Leetcode 题解 - 数组与矩阵](#leetcode-题解---数组与矩阵)
    * [1. 把数组中的 0 移到末尾](#1-把数组中的-0-移到末尾)
    * [2. 改变矩阵维度](#2-改变矩阵维度)
    * [3. 找出数组中最长的连续 1](#3-找出数组中最长的连续-1)
    * [4. 有序矩阵查找](#4-有序矩阵查找)
    * [5. 有序矩阵的 Kth Element](#5-有序矩阵的-kth-element)
    * [6. 一个数组元素在 [1, n] 之间，其中一个数被替换为另一个数，找出重复的数和丢失的数](#6-一个数组元素在-1-n-之间-其中一个数被替换为另一个数-找出重复的数和丢失的数)
    * [7. 找出数组中重复的数，数组值在 [1, n] 之间](#7-找出数组中重复的数-数组值在-1-n-之间)
    * [8. 数组相邻差值的个数](#8-数组相邻差值的个数)
    * [9. 数组的度](#9-数组的度)
    * [10. 对角元素相等的矩阵](#10-对角元素相等的矩阵)
    * [11. 嵌套数组](#11-嵌套数组)
    * [12. 分隔数组](#12-分隔数组)
    * [13. tbd](#13-tbd)
<!-- GFM-TOC -->


## 1. 把数组中的 0 移到末尾

283\. Move Zeroes (Easy)

[Leetcode](https://leetcode.com/problems/move-zeroes/description/) / [力扣](https://leetcode-cn.com/problems/move-zeroes/description/)

```html
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
```

```c
void moveZeroes(int* nums, int numsSize) { 
    int checked = 0;
    for(int i = 0; i < numsSize; i++) {
        if(nums[i] != 0) {
            nums[checked] = nums[i];
            checked++;
        }
    }
    while(checked < numsSize) {
        nums[checked++] = 0;
    }
}
```

## 2. 改变矩阵维度

566\. Reshape the Matrix (Easy)

[Leetcode](https://leetcode.com/problems/reshape-the-matrix/description/) / [力扣](https://leetcode-cn.com/problems/reshape-the-matrix/description/)

```html
Input:
nums =
[[1,2],
 [3,4]]
r = 1, c = 4

Output:
[[1,2,3,4]]

Explanation:
The row-traversing of nums is [1,2,3,4]. The new reshaped matrix is a 1 * 4 matrix, fill it row by row by using the previous list.
```

```c
/**
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *returnColumnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */
int** matrixReshape(int** mat, int matSize, int* matColSize, int r, int c, int* returnSize, int** returnColumnSizes) {
    if(matSize * matColSize[0] != r * c) {
        *returnSize = matSize;
        *returnColumnSizes = malloc(matSize * sizeof(int));
        for(int i = 0; i < matSize; i++) {
            (*returnColumnSizes)[i] = matColSize[0];
        }
        return mat;
    }
    int* arr = malloc(r * c * sizeof(int));
    for(int i = 0; i < r * c; i++) {
        arr[i] = mat[i / matColSize[0]][i % matColSize[0]];
    }
    int** out = malloc(r * sizeof(int*));
    for(int i = 0; i < r; i++) {
        out[i] = malloc(c * sizeof(int));
    }
    for(int i = 0; i < r; i++) {
        for(int j = 0; j < c; j++) {
            out[i][j] = arr[i * c + j];
        }
    }
    *returnSize = r;
    *returnColumnSizes = malloc(r * sizeof(int));
    for(int i = 0; i < r; i++) {
        (*returnColumnSizes)[i] = c;
    }
    free(arr);
    return out;
}
```

## 3. 找出数组中最长的连续 1

485\. Max Consecutive Ones (Easy)

[Leetcode](https://leetcode.com/problems/max-consecutive-ones/description/) / [力扣](https://leetcode-cn.com/problems/max-consecutive-ones/description/)

```c
int findMaxConsecutiveOnes(int* nums, int numsSize) {
    int cnt = 0, max = 0;
    for(int i = 0; i < numsSize; i++) {
        if(nums[i] == 1) {
            cnt++;
        }
        else {
            cnt = 0;
        }
        if(cnt > max) {
            max = cnt;
        }
    }
    return max;
}
```

## 4. 有序矩阵查找

240\. Search a 2D Matrix II (Medium)

[Leetcode](https://leetcode.com/problems/search-a-2d-matrix-ii/description/) / [力扣](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/description/)

```html
[
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
]
```

```c
bool searchMatrix(int** matrix, int matrixSize, int* matrixColSize, int target){
    int r = matrixSize - 1, c = 0; 
    if(matrix == NULL)
        return false;
    while(r >= 0 && c <= matrixColSize[0] - 1) {
        if(matrix[r][c] == target)
            return true;
        else if(matrix[r][c] < target)
            c++;
        else
            r--;
    }
    return false;
}
```

## 5. 有序矩阵的 Kth Element

378\. Kth Smallest Element in a Sorted Matrix ((Medium))

[Leetcode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/) / [力扣](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/description/)

```html
matrix = [
  [ 1,  5,  9],
  [10, 11, 13],
  [12, 13, 15]
],
k = 8,

return 13.
```

解题参考：[Share my thoughts and Clean Java Code](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/85173)

二分查找解法：

```c
tbd
```

堆解法：

```tbd
tbd
```

## 6. 一个数组元素在 1 n 之间 其中一个数被替换为另一个数 找出重复的数和丢失的数

645\. Set Mismatch (Easy)

[Leetcode](https://leetcode.com/problems/set-mismatch/description/) / [力扣](https://leetcode-cn.com/problems/set-mismatch/description/)

```html
Input: nums = [1,2,2,4]
Output: [2,3]
```

```html
Input: nums = [1,2,2,4]
Output: [2,3]
```

最直接的方法是先对数组进行排序，这种方法时间复杂度为 O(NlogN)。本题可以以 O(N) 的时间复杂度、O(1) 空间复杂度来求解。

主要思想是通过交换数组元素，使得数组上的元素在正确的位置上。

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findErrorNums(int* nums, int numsSize, int* returnSize) {
    int* mark = malloc(numsSize * sizeof(int));
    int* out = malloc(2 * sizeof(int));
    memset(mark, 0, numsSize * sizeof(int));
    for(int i = 0; i < numsSize; i++) {
        if(mark[nums[i] - 1] == 0) {
            mark[nums[i] - 1] = 1;
        }
        else {
            out[0] = nums[i];
        }
    }
    for(int i = 0; i < numsSize; i++) {
        if(mark[i] == 0) {
            out[1] = i + 1;
        }
    }
    *returnSize = 2;
    free(mark);
    return out;
}
```

## 7. 找出数组中重复的数 数组值在 1 n 之间

287\. Find the Duplicate Number (Medium)

[Leetcode](https://leetcode.com/problems/find-the-duplicate-number/description/) / [力扣](https://leetcode-cn.com/problems/find-the-duplicate-number/description/)

要求不能修改数组，也不能使用额外的空间。

二分查找解法：

```java
public int findDuplicate(int[] nums) {
     int l = 1, h = nums.length - 1;
     while (l <= h) {
         int mid = l + (h - l) / 2;
         int cnt = 0;
         for (int i = 0; i < nums.length; i++) {
             if (nums[i] <= mid) cnt++;
         }
         if (cnt > mid) h = mid - 1;
         else l = mid + 1;
     }
     return l;
}
```

双指针解法，类似于有环链表中找出环的入口：

```c
int findDuplicate(int* nums, int numsSize) {
    int slow = nums[0], fast = nums[nums[0]];
    while(slow != fast) {
        slow = nums[slow];
        fast = nums[nums[fast]];
    }
    fast = 0;
    while(slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

## 8. 数组相邻差值的个数

667\. Beautiful Arrangement II (Medium)

[Leetcode](https://leetcode.com/problems/beautiful-arrangement-ii/description/) / [力扣](https://leetcode-cn.com/problems/beautiful-arrangement-ii/description/)

```html
Input: n = 3, k = 2
Output: [1, 3, 2]
Explanation: The [1, 3, 2] has three different positive integers ranging from 1 to 3, and the [2, 1] has exactly 2 distinct integers: 1 and 2.
```

题目描述：数组元素为 1\~n 的整数，要求构建数组，使得相邻元素的差值不相同的个数为 k。

让前 k+1 个元素构建出 k 个不相同的差值，序列为：1 k+1 2 k 3 k-1 ... k/2 k/2+1.

```c
tbd
```

## 9. 数组的度

697\. Degree of an Array (Easy)

[Leetcode](https://leetcode.com/problems/degree-of-an-array/description/) / [力扣](https://leetcode-cn.com/problems/degree-of-an-array/description/)

```html
Input: [1,2,2,3,1,4,2]
Output: 6
```

题目描述：数组的度定义为元素出现的最高频率，例如上面的数组度为 3。要求找到一个最小的子数组，这个子数组的度和原数组一样。

```c
int findShortestSubArray(int* nums, int numsSize) {
    int freq[50000], degree[50000], start_pos[50000];
    int max_freq = 0, out = 50000;
    memset(freq, 0, sizeof(freq));
    memset(degree, 0, sizeof(degree));
    memset(start_pos, -1, sizeof(degree));
    
    for(int i = 0; i < numsSize; i++) {
        freq[nums[i]]++;
        if(start_pos[nums[i]] == -1) {
            start_pos[nums[i]] = i;
            degree[nums[i]] = 1;
        }
        else
            degree[nums[i]] = i - start_pos[nums[i]] + 1;
    }
    for(int i = 0; i < 50000; i++) {
        if(freq[i] > max_freq)
            max_freq = freq[i];
    }
    for(int i = 0; i < 50000; i++) {
        if(freq[i] == max_freq && degree[i] < out) {
            out = degree[i];
        }
    }
    return out;
}
```

## 10. 对角元素相等的矩阵

766\. Toeplitz Matrix (Easy)

[Leetcode](https://leetcode.com/problems/toeplitz-matrix/description/) / [力扣](https://leetcode-cn.com/problems/toeplitz-matrix/description/)

```html
1234
5123
9512

In the above grid, the diagonals are "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]", and in each diagonal all elements are the same, so the answer is True.
```

```c
bool isToeplitzMatrix(int** matrix, int matrixSize, int* matrixColSize) {
    //int r = 0, c = 0;
    for(int r = 0; r < matrixSize; r++) {
        for(int c = 0; c < matrixColSize[r]; c++) {
            if(r + 1 < matrixSize && c + 1 < matrixColSize[r + 1])
                if(matrix[r][c] != matrix[r + 1][c + 1])
                    return false;
        }
    }
    return true;
}
```

## 11. 嵌套数组

565\. Array Nesting (Medium)

[Leetcode](https://leetcode.com/problems/array-nesting/description/) / [力扣](https://leetcode-cn.com/problems/array-nesting/description/)

```html
Input: A = [5,4,0,3,1,6,2]
Output: 4
Explanation:
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
```

题目描述：S[i] 表示一个集合，集合的第一个元素是 A[i]，第二个元素是 A[A[i]]，如此嵌套下去。求最大的 S[i]。

```c
int arrayNesting(int* nums, int numsSize) {
    char mark[numsSize];
    int out = 0, len = 0, it = 0;
    for(int i = 0; i < numsSize; i++) {
        memset(mark, 0, sizeof(mark));
        it = i;
        while(mark[it] != 1) {
            mark[it] = 1;
            it = nums[it];
            len++;
        }
        if(len > out)
            out = len;
        len = 0;
    }
    return out;
}
```

## 12. 分隔数组

769\. Max Chunks To Make Sorted (Medium)

[Leetcode](https://leetcode.com/problems/max-chunks-to-make-sorted/description/) / [力扣](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/description/)

```html
Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.
```

题目描述：分隔数组，使得对每部分排序后数组就为有序。

```c
int maxChunksToSorted(int* arr, int arrSize) {
    if(arr == NULL)
        return 0;
    int out = 0, it = arr[0];
    for(int i = 0; i < arrSize; i++) {
        it = it > arr[i] ? it : arr[i];
        if(it == i)
            out++;
    }
    return out;
}
```

## 13. tbd

769\. Max Chunks To Make Sorted (Medium)

[Leetcode](https://leetcode.com/problems/max-chunks-to-make-sorted/description/) / [力扣](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/description/)

```html
Input: arr = [1,0,2,3,4]
Output: 4
Explanation:
We can split into two chunks, such as [1, 0], [2, 3, 4].
However, splitting into [1, 0], [2], [3], [4] is the highest number of chunks possible.
```

题目描述：分隔数组，使得对每部分排序后数组就为有序。

```c
tbd
```

# Leetcode records

### [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

M, Array, Dynamic Programming

从左上到右下，最少要多少步。动态规划，定义状态和状态转移方程，状态就是到达每一个点最少的步骤，这是一个最优子结构和具有子问题重叠的问题。无后效性。

```
s[i][j] = Min{s[i - 1][j] + s[i][j - 1]} + grid[i][j]
```

### [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

M, Array, Dynamic Programming

和63类似，只是没有了权重，每个点都是1，动态规划，核心逻辑：
```
if (obstacleGrid[i][j] == 1) {
    obstacleGrid[i][j] = 0;
} else if (i == 0 && j == 0) {
    obstacleGrid[i][j] = 1;
} else if (i == 0) {
    obstacleGrid[i][j] = obstacleGrid[i][j - 1] * 1;  // 乘法用的很巧妙！！！
    // For row 0, if there are no paths to left cell, then its 0,else 1
} else if (j == 0) {
    obstacleGrid[i][j] = obstacleGrid[i - 1][j] * 1;
    // For col 0, if there are no paths to upper cell, then its 0,else 1
} else {
    obstacleGrid[i][j] = obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1];
}
```


### [62. Unique Paths](https://leetcode.com/problems/unique-paths/)

M, Array, Dynamic Programming

m*n矩阵，top-left -> bottom-right共有多少路线.

动态规划，初始第一行、第一列为0，写出状态转移方程，计算，然后查表。O(N*M)的空间和时间复杂度。

```
[1, 1, 1, 1]
[1, 2, 3, 4]
[1, 3, 6, 10]
[1, 4, 10, 20]
```

```
for (int i = 0; i < m; i++)
    arr[i][0] = 1;
for (int j = 0; j < n; j++)
    arr[0][j] = 1;
for (int i = 1; i < m; i++)
    for (int j = 1; j < n; j++) {
        arr[i][j] = arr[i][j - 1] + arr[i - 1][j];  // 上面+左面的cell路径和
    }
```

### [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

M,  Array, Dynamic Programming, Divide and Conquer

非常经典的动态规划问题

```
/**
 * O(N^2)暴力求解
 */
public int findMaxSumOfSubArray_BruteForce(int[] array) {
    int maxSum = array[0];
    for (int i = 0; i < array.length; i++) {
        int subSum = 0;
        for (int j = i; j < array.length; j++) {
            subSum += array[j];
            if (subSum > maxSum) {
                maxSum = subSum;
            }
        }
    }
    return maxSum;
}

/**
 * 动态规划求解。
 * s[i]表示以i结尾最大的子数组和，如果s[i - 1]小于0了，则说明在i肯定是保留现在的值更大，否则就叠加前面的s[i - 1]
 * <pre>
 * s[i] = Max{s[i] (s[i - 1] < 0), s[i - 1] + v[i]}
 * </pre>
 * <p>
 * 最后遍历一下s[i]数组即可。
 */
public int findMaxSumOfSubArray_DP(int[] array) {
    int[] s = new int[array.length];
    s[0] = array[0];
    for (int i = 1; i < array.length; i++) {
        if (s[i - 1] < 0) {
            s[i] = array[i];
        } else {
            s[i] = s[i - 1] + array[i];
        }
    }

    int maxSum = s[0];
    for (int i : s) {
        if (i > maxSum) {
            maxSum = i;
        }
    }
    return maxSum;
}
```

另外还有分治法，需要进一步研究[leetcode discussion](https://discuss.leetcode.com/topic/426/how-to-solve-maximum-subarray-by-using-the-divide-and-conquer-approach/2)

### [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

E, Stack, String

```
foreach c in s
    if c is open brackets like [ { (
        push c to stack
    else
       if stack is empty
           return false
       if stack.pop is NOT pair to c
           return false
return stack is empty true or else false
```

### [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

E，linked list，two pointers

技巧是搞一个ListNode在head的前面

### [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

M, Backtracking String

回溯法 + DFS，类似于笛卡尔乘积的方式
```
find(res, combination, 0, digits);

void find(List<String> res, char[] combination, int index, String digits) {
    if (index == digits.length()) {
        res.add(new String(combination));
    } else {
        int digit = digits.charAt(index) - '0';
        for (char c : charMap[digit]) {
            combination[index] = c;
            find(res, combination, index + 1, digits);
        }
    }
}
```

### [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

M, Array, Two pointers

和15题类似，

初始值
```
int closest = nums[0] + nums[1] + nums[2];
```

中间判断条件变为：
```
if (Math.abs(target - (left + mid + right)) < Math.abs(target - closest)) {
    closest = (left + mid + right);
}
```

### [15. 3Sum](https://leetcode.com/problems/3sum/)

M, Array, Two pointers

题目要求a, b, c in S such that a + b + c = 0

* 先排序
* 从后往前定住最后一个，然后从前往后定住一个，中间的遍历找。
* 找到了，那么为了去重，要跳过从前往后重复的元素，以及从后往前重复的元素。这样就不用维护Set了。

```
right = nums.length - 1;
left = 0;
while (right > 1) {
    rightNum = nums[right];
    while (left < right - 1) {
        leftNumber = nums[left];
        for (int mid = left + 1; mid < right; mid++) {
            int midNum = nums[mid];
            if (midNum + positiveNum + negativeNum == 0) {
                result.add(leftNum, midNum, rightNum);
                break;  // need to quit in case next number is the same as mid
            }
        }
        while (left < right && nums[left] == negativeNum) {
            left++;
        }
    }
    left = 0;
    while (right > 0 && nums[right] == positiveNum) {
        right--;
    }
}
```

### [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

E, String

基本思想就是固定第一个str，对于其中的每一个char，后面的所有的str都去除第N个和其对比，直到发现不一样的为止。
```
foreach char in strs[0] {
    idx represents position from 0 to len -1
    for every str in strs
        if (strs[i].length() < idx + 1 || strs[0].charAt(idx) != strs[i].charAt(idx)) {
            return ret.toString();
        }
    ret.append(strs[0].charAt(idx));
}
```

### [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

M, Array Two Pointers
```
i = 0
j = end
while i and j not meet
    max = MAX(area(i,j) ,max)
    if height[i] < height[j]
        i = i + 1
    if height[i] > height[j]
        j = j - 1
```

### [9. Palindrome Number](https://leetcode.com/problems/palindrome-number/)

E

Problems related with numbers are frequently solved by / and %. No need of extra space is required. This problem
is similar with the Reverse Integer problem.

Note: no extra space here means do not convert the integer to string, since string will be a copy of the integerand take extra space. The space take by div, left, and right can be ignored.

计算高位和低位是不是一样的数组，用/和%运算符即可。

### [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

E, Math,String

大概简单来说，算法如下：
```
int x = ch - '0'; // char to int
num = num * 10;
num = num + x;
```
但是要处理一堆比如空格，正负数，溢出等等，总是没提交正确。faint。

### [7. Reverse Integer](https://leetcode.com/problems/reverse-integer/)

E, Math

注意溢出的问题。
```
while (x > 0) {
    // ret * 10 + (x % 10) > Integer.MAX_VALUE
    if (ret > (Integer.MAX_VALUE - x % 10) / 10) {
        return 0;
    }
    ret = ret * 10 + (x % 10);
    x /= 10;
}
```
负数也一样

### [6. ZigZag Conversion](https://leetcode.com/problems/zigzag-conversion/)

M, String

所谓的zigzag就是按照某种pattern去打印字符串。这道题解的实际就是一个找“规律”的问题。

我的解法是用笔和纸，按照numRows=2，3，4，5去尝试画出来实际的样子，然后计算

1）首先要区分单元unit，就是所谓的一个一个小格子，比如
numRows=3，那么ABCD，EFGH，IJK各自是一个单元，单元内元素的个数等于(numRows - 1)*2。

2）计算出一共有多少个单元unit。

3）一个单元一个单元的打印出来，小于numRows的很好安置位置，大于numRows的元素有一个规律，
就是他们是row--，col++的斜线往上走的，掌握的这个规律，就可以把一个char[][]的二维数组

看出我们要打印出来的样子，去确定这个row和col，遍历字符串填充进去即可。

此题比较恶心，不细说了。

### [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

M, String

例如`dfabbax`的palindromic子串是`abba`。

O(N^2)，从第一个char开始左右扩展，记录一个low和maxLen表示palindromic的字符.

```
for(every char in s) {
   j, k
   while s[k] = s[k + 1]
      k++ // skip duplicates
   if s[j] to s[k] // handle well corner condition
      j--
      k++
   low = j
   maxLen = k - j + 1
}
```

### [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

H, Binary Search, Array, Divide and Conquer

题目要求O(log(m+n))的时间复杂度，一般来说都是分治法或者二分搜索。

[refer1](http://www.07net01.com/2015/07/871155.html)
[refer2](https://leetcode.com/submissions/detail/69094393/)

另外最笨的方法就是排序下时间复杂度是O(NLogN)，不考虑。

首先定义解题的入口：

区分奇数和偶数，偶数需要计算两个值求平均，奇数直接取中间的即可。

```
if ((m + n) % 2 == 0) {
    return (findKth(nums1, nums2, 0, 0, m, n, k) + findKth(nums1, nums2, 0, 0, m, n, k + 1)) / 2;
} else {
    return findKth(nums1, nums2, 0, 0, m, n, k + 1);
}
```

把问题转换为Kth of two sorted array问题（搜索两个有序序列的第k个元素），归并计数法 Merge and Count，时间复杂度是O(K)，但是K比较大的时候，还是会退换成O(M+N)，即O(N)。在leetcode提交这个效率也比较高。这个算brute force版本，类似于two pointers的思想：

```
foreach n in array m and array n
    if isKth(n)
        found
```

优化为O(log(m+n))时间复杂度的算，采用分治法

假设序列都是从小到大排列，对于第一个序列中前p个元素和第二个序列中前q个元素，我们想要的最终结果是：p+q等于k-1,且一序列第p个元素和二序列第q个元素都小于总序列第k个元素。

因为总序列中，必然有k-1个元素小于等于第k个元素。这样第p+1个元素或者第q+1个元素就是我们要找的第k个元素。

实际上就是不断的二分，舍弃掉绝对不可能存在要找的解的区间段这种思想。

边界条件corner conditions非常多，写递归完成，退出条件要定义好。

1）保证arr1的长度较为小

2）arr1为空了，则直接在arr2里面直接索引定位

3）如果k=1，那么直接取arr1和arr2的头元素比较，小的就是要找的。

```
double findKth(int[] arr1, int[] arr2, int start1, int start2, int len1, int len2, int k) {
    if (len1 > len2) {
        return findKth(arr2, arr1, start2, start1, len2, len1, k);
    }
    if (len1 == 0) {
        return arr2[start2 + k - 1];
    }
    if (k == 1) {
        return Math.min(arr1[start1], arr2[start2]);
    }
    int p1 = Math.min(k / 2, len1);
    int p2 = k - p1;
    if (arr1[start1 + p1 - 1] < arr2[start2 + p2 - 1]) {
        return findKth(arr1, arr2, start1 + p1, start2, len1 - p1, len2, k - p1);
    } else if (arr1[start1 + p1 - 1] > arr2[start2 + p2 - 1]) {
        return findKth(arr1, arr2, start1, start2 + p2, len1, len2 - p2, k - p2);
    } else {
        return arr1[start1 + p1 - 1];
    }
}
```

### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

M,  Hash Table,Two Pointers,String

维护一个窗口，从当前char，叫做curr，往回看，如果发现相同的则，重置barrier，这个区间就是无重复的一个区间。常数时间，时间复杂度O(N)。
```
barrier = 0
foreach i in s
    while s[barrier .. i] contains charaters
        barrier = barrier + 1
    max = MAX(max, curr - barrier + 1)
```

### [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
```
M, Linked list, Math

O(N)，two pointers分别齐头并进的在两个list中从后向前，利用/计算进位，利用%就算余数，知道一个list耗尽，把剩下的list按照刚刚相同的操作继续，最后需要注意如果进位为0，那么需要把最后一个ListNode删除。

### [1. Two Sum](https://leetcode.com/problems/two-sum/)

E, Array, Hash table

用一个hashmap，key是值，value是index+1。

```
for (int i = 0; i < nums.length; i++) {
    if (map.containsKey(target - nums[i])) {
        return new int[] {map.get(target - nums[i]), i + 1};
    }
    map.put(nums[i], i + 1);
}
```
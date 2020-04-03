# Day02 - 删除排序数组的重复项

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

**示例1** 

给定数组 nums = [1,1,2],函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。你不需要考虑数组中超出新长度后面的元素。`

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int length;

        if (nums.length == 0) {
            return 0;
        }

        length = 1;

        for (int i = 0, j = 1; i < nums.length; i++) {
            while (j < nums.length && nums[j] == nums[i]) {
                j++;
            }

            if (j >= nums.length)
                break;

            if (nums[j] != nums[i]) {
                nums[i + 1] = nums[j];
                length++;
            }
        }

        return length;
    }
}

/**
执行用时 :1 ms, 在所有 Java 提交中击败了99.89%的用户
*/
```

`length` 记录新数组长度，只需要在原数组中遍历一次与第 `i` 项比较若不同则将第 `j` 项赋给第 `i+1` 项。
 

> Reference  
> https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/


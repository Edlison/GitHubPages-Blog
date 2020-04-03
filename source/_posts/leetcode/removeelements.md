# Day03 - 移除元素

给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**示例 1:** 

给定 nums = [3,2,2,3], val = 3,函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素. 

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int res = 0;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != val) {
                nums[res] = nums[i];
                res++;
            }
        }

        return res;
    }
}

/**
执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户
内存消耗 :38.1 MB, 在所有 Java 提交中击败了5.05%的用户
*/
```

`res`记录新数组长度，遍历一遍数组满足条件则在相应位置赋值。

> Reference  
> https://leetcode-cn.com/problems/remove-element


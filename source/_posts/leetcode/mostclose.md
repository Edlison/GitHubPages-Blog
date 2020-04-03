# 最接近的三数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

**例如**

给定数组 nums = [-1，2，1，-4], 和 target = 1.
与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int temp = nums[0] + nums[1] + nums[2];
        int res = temp;
        int min = Math.abs(temp - target);
        int tempMin;
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1, right = nums.length - 1;

            while (left < right) {
                temp = nums[i] + nums[left] + nums[right];
                tempMin = Math.abs(temp - target);

                if (tempMin < min) {
                    min = tempMin;
                    res = temp;
                }

                if (temp < target) {
                    left++;
                } else if (temp > target) {
                    right--;
                } else {
                    return target;
                }
            }
        }

        return res;
    }
}

/**
执行用时 :5 ms, 在所有 Java 提交中击败了95.39%的用户
*/
```

数组首先排序，循环n次，左右指针在`i ~ nums.length - 1`范围内移动，判定`left`<`right`.  
`min`用来存储最小差值，当有三数之和与`target`小于该值便将其赋值给`res`.

> Reference  
> https://leetcode-cn.com/problems/3sum-closest

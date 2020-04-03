# 三数之和(Unsolved)

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

**示例**：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：  
[
  [-1, 0, 1],
  [-1, -1, 2]
]

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        if (nums == null || nums.length <= 2) {
        return Collections.emptyList();
    }
    List<List<Integer>> result = new LinkedList<>();

    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 2; i++) {
        // 加速1：c为非负数，就不能满足a+b+c=0了
        if (nums[i] > 0) {
            return result;
        }
        // 加速2：跳过计算过的数据，同时防止结果重复
        if (i != 0 && nums[i] == nums[i-1]) {
            continue;
        }
        int head = i + 1;
        int tail = nums.length - 1;
        while (head < tail) {
            int sum = -(nums[head] + nums[tail]);
            if (sum == nums[i]) {
                result.add(Arrays.asList(nums[i], nums[head], nums[tail]));
                // 加速3：跳过计算过的数据，同时防止结果重复
                while (head < tail && nums[head] == nums[head+1]) {
                    head++;
                }
                while (head < tail && nums[tail] == nums[tail-1]) {
                    tail--;
                }
            }
            if (sum <= nums[i]) {
                tail--;
            } else {
                head++;
            }
        }
    }

    return result;
    }
}
```

> Reference  
> https://leetcode-cn.com/problems/3sum/solution/san-shu-zhi-he-javajian-ji-ti-jie-by-wang-zi-hao-z/
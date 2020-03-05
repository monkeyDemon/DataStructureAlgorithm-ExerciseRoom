# leetcode 15. 三数之和

## 题目

链接：https://leetcode-cn.com/problems/3sum

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例：

给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]


## C++代码

首先最直接的解法显然是暴力循环，将所有可能的情况都遍历一遍。

这样所需要嵌套三个for循环，因此时间复杂度O(N3)，显然无法接受。

如何优化呢？我没有思路，考虑先将问题简化。

可以对给定数组进行排序，变为有序数组，而排序的时间复杂度时O(NlogN)，在暴力法面前不值一提，因此这个思路可行。

当数组有序时，我们就有办法处理了。问题变为了：

固定三元组中的最小数，找到 b + c = -a

既然是有序数组，我们就可以用双指针法，不断缩小可取值的范围，直到左指针l<右指针r的条件不满足

另外，需要思考下如何处理满足条件的三元组不唯一的情况，以及如何去重。

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int target;
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            if ((target = nums[i]) > 0) break;
            int l = i + 1, r = nums.size() - 1;
            while (l < r) {
                if (nums[l] + nums[r] + target < 0) ++l;
                else if (nums[l] + nums[r] + target > 0) --r;
                else {
                    ans.push_back({target, nums[l], nums[r]});
                    ++l, --r;
                    while (l < r && nums[l] == nums[l - 1]) ++l;
                    while (l < r && nums[r] == nums[r + 1]) --r;
                }
            }
        }
        return ans; 
    }
};
```

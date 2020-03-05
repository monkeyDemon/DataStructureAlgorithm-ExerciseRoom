# leetcode 16. 最接近的三数之和

## 题目

链接：https://leetcode-cn.com/problems/3sum-closest/

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).

## C++代码

依旧延续`leetcode NO.15 三数之和`的方案，没有思路，先将问题简化，对数组nums进行排序，转为有序数组。

此时，我们依然可以使用双指针法来求解本问题。

注意思考：如何证明双指针法在逐渐收缩的过程中不会错过最优解？

```c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int total_best_sum = -1;
        int total_min_dict = INT_MAX;
        if(nums.size() < 3)
            return total_best_sum;
        // 首先对数组进行排序
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size() - 2; i++){
            int a = nums[i];
            int residual = target - a;
            int l = i + 1;
            int r = nums.size() - 1;
            int min_dict = INT_MAX;
            int best_sum = -1;
            while(l < r){
                int cur_threeSum = a + nums[l] + nums[r];
                int cur_dict = abs(target - cur_threeSum);
                if(cur_dict == 0)
                    return target;
                if(cur_dict < min_dict){
                    min_dict = cur_dict;
                    best_sum = cur_threeSum;
                }
                if(target - cur_threeSum > 0)
                    l++;
                else
                    r--;
            }
            // 结束了a = nums[i]时最优三元组的寻找，判断是否为总体最优
            if(min_dict < total_min_dict){
                total_min_dict = min_dict;
                total_best_sum = best_sum;
            }
        }
        return total_best_sum;
    }
};
```

执行用时: 4 ms, 在所有 C++ 提交中击败了 99.46% 的用户
内存消耗: 12.2 MB, 在所有 C++ 提交中击败了 5.14% 的用户

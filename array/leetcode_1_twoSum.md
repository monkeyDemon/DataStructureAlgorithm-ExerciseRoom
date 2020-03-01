# leetcode 1.两数之和

## 题目

链接：https://leetcode-cn.com/problems/two-sum

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9

所以返回 [0, 1]


## C++

### 暴力解法1

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> r;
        for(int i = 0; i < nums.size() - 1; ++i){
            for(int j = i+1; j < nums.size(); ++j){
                if(nums[i] + nums[j] == target){
                    r.push_back(i);
                    r.push_back(j);
                    return r;
                }
            }
        }
        return {};
    }
};
```

执行用时: 180 ms, 在所有 C++ 提交中击败了 42.70% 的用户
内存消耗: 11.5 MB, 在所有 C++ 提交中击败了 5.04% 的用户

### 暴力解法2

即使是暴力解法，上面的实现中也存在一个显著的优化点

当nums[i]确定时，我们可以提前把差值算好，而不是每次都去计算 nums[i] + nums[j] == target

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> r;
        for(int i = 0; i < nums.size() - 1; ++i){
            int res = target - nums[i];
            for(int j = i+1; j < nums.size(); ++j){
                if(nums[j] == res){
                    r.push_back(i);
                    r.push_back(j);
                    return r;
                }
            }
        }
        return r;
    }
};
```

执行用时: 164 ms, 在所有 C++ 提交中击败了 45.01% 的用户
内存消耗: 11.3 MB, 在所有 C++ 提交中击败了 5.04% 的用户

### 哈希表解法

这道题最优的解法应该是哈希表

时间复杂度：O(n), 空间复杂度：O(n)

设置一个 map 容器 record 用来记录元素的值与索引，然后遍历数组 nums。

每次遍历时使用临时变量 res 用来保存目标值与当前值的差值

在此次遍历中查找 record ，查看是否有与 res 一致的值，如果查找成功则返回查找值的索引值与当前变量的值 i

如果未找到，则在 record 保存该元素与索引值 i

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> record;
        for(int i = 0; i < nums.size(); ++i){
            int res = target - nums[i];
            if(record.find(res) != record.end()){
                int r[] = {i, record[res]};
                return vector<int>(r, r+2);
            }
            record[nums[i]] = i;
        }
        return {};
    }
};
```

执行用时: 4 ms, 在所有 C++ 提交中击败了 99.75% 的用户
内存消耗: 12.1 MB, 在所有 C++ 提交中击败了 5.04% 的用户

## Python3

python 哈希表解法

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap={}
        for i,num in enumerate(nums):
            if hashmap.get(target - num) is not None:
                return [i,hashmap.get(target - num)]
            hashmap[num] = i #这句不能放在if语句之前，解决list中有重复值或target-num=num的情况
```

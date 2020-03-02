# leetcode 26. 删除排序数组中的重复项

## 题目

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:

给定数组 nums = [1,1,2], 

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 

你不需要考虑数组中超出新长度后面的元素。

示例 2:

给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。


## C++代码

### 双指针法1

这道题很简单，双指针法。

用一个变量double_num记录当前是什么值（用于判断是否重复）

还有一个变量cover_idx记录不重复的情况下应该覆盖的索引位置即可

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() <= 1)
            return nums.size();
        int cover_idx = 1;
        int cur_idx = 1;
        int double_num = nums[0];
        for(int i = 1; i < nums.size(); ++i){
            int cur_num = nums[i];
            if(cur_num != double_num){
                nums[cover_idx++] = cur_num;
                double_num = cur_num;
            }
        }
        return cover_idx;
    }
};
```

执行用时: 20 ms, 在所有 C++ 提交中击败了 64.56% 的用户

内存消耗: 15.3 MB, 在所有 C++ 提交中击败了 5.21% 的用户

### 双指针法2

上面的代码不够简洁和优雅，还可以继续优化:

实际上，我们只需一个变量i来记录上一个不重复的有效值的索引，然后用变量j从1开始遍历数组即可

因为nums[i]即是上一个不重复值，用于和nums[j]进行对比

而i+1即为下一个新的不重复值的存放位置

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        int i = 0;
        for(int j = 1; j < nums.size(); ++j){
            if(nums[j] != nums[i]){
                nums[++i] = nums[j];
            }
        }
        return i+1;
    }
};
```

执行用时: 16 ms, 在所有 C++ 提交中击败了 83.05% 的用户

内存消耗: 15.3 MB, 在所有 C++ 提交中击败了 5.21% 的用户

## Python3代码

### 双指针法

同样的逻辑，我们可以写出python的双指针法代码如下

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        i = 0
        for j in range(1, len(nums)):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]
        return i+1
```


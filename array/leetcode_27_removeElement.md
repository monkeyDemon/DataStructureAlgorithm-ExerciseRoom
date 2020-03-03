# leetcode 27. 移除元素

## 题目

链接：https://leetcode-cn.com/problems/remove-element

给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例 1:

给定 nums = [3,2,2,3], val = 3,

函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。

你不需要考虑数组中超出新长度后面的元素。
示例 2:

给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。


## C++代码

### 双指针法

这道题和 leetcode 26. 删除排序数组中的重复项 非常类似。

依旧使用双指针法，使用变量j来遍历数据，使用变量i来记录下一个满足要求的元素存放的位置即可

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i = 0;
        for(int j = 0; j < nums.size(); ++j){
            if(nums[j] != val)
                nums[i++] = nums[j];
        }
        return i;
    }
};
```

执行用时: 4 ms, 在所有 C++ 提交中击败了 80.43% 的用户

内存消耗: 11.1 MB, 在所有 C++ 提交中击败了 5.84% 的用户


## Python3代码

### 双指针法

同样的逻辑，我们可以写出python的双指针法代码如下

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i = 0
        for j in range(len(nums)):
            if nums[j] != val:
                nums[i] = nums[j]
                i += 1
        return i
```


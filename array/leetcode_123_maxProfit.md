# leetcode 123. 买卖股票的最佳时机 III

## 题目

链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

输入: [3,3,5,0,0,3,1,4]

输出: 6

解释: 在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3。随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。

示例 2:

输入: [1,2,3,4,5]

输出: 4

解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4。注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。

示例 3:

输入: [7,6,4,3,1] 

输出: 0 

解释: 在这个情况下, 没有交易完成, 所以最大利润为 0。

## C++代码

思路提示：

这道题难度蛮大，如何进行两笔交易完成最大收益很难想明白，那么先将问题拆解。

如何在代表对应股票价格的数组中进行一笔交易完成收益最大？

这个问题想明白，这个问题就解了。

这个问题可以用到动态规划的思想：前N天的最大收益可以借助前N-1天的最大收益来得到。

如果我们在第N天没有进行卖出，那么前N天的最大收益=前N-1天的最大收益

如果我们在第N天卖出，那么前N天最大收益=max(第N天卖出的最大收益, 高于前N-1天的最大收益)

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int day_num = prices.size();
        if(day_num < 2)
            return 0;

        // 动态规划从前往后求解在前i天完成交易的最大收益
        vector<int> min_prices_list(day_num);  // 元素i记录截止到第i天的最低股票价格
        vector<int> max_profit_list(day_num);  // 元素i记录截止到第i天所能获得的最大收益
        min_prices_list[0] = prices[0];
        for(int i = 1; i < day_num; ++i){
            int cur_day_profit = prices[i] - min_prices_list[i-1]; // 如果在第i天卖出所能获得的最大收益
            max_profit_list[i] = (cur_day_profit > max_profit_list[i-1]) ? cur_day_profit : max_profit_list[i-1];  // 截止到第i天所能获得的最大收益
            min_prices_list[i] = (prices[i] < min_prices_list[i-1]) ? prices[i] : min_prices_list[i-1]; // 截止到第i天的最低股票价格
        }

        // 动态规划从后往前求解在前i天完成交易的最大收益
        vector<int> max_prices_list(day_num);  // 元素i记录第i天到最后一天的最高股票价格
        vector<int> second_max_profit_list(day_num);  // 元素i记录第i天到最后一天所能获得的最大收益
        max_prices_list[day_num - 1] = prices[day_num - 1];
        for(int i = day_num - 2; i >= 0; --i){
            int cur_day_profit = max_prices_list[i+1] - prices[i]; // 如果在第i天买入所能获得的最大收益
            second_max_profit_list[i] = (cur_day_profit > second_max_profit_list[i+1]) ? cur_day_profit : second_max_profit_list[i+1];  // 第i天到最后一天所能获得的最大收益
            max_prices_list[i] = (prices[i] > max_prices_list[i+1]) ? prices[i] : max_prices_list[i+1]; // 第i天到最后一天的最高股票价格
        }
        
        //遍历求出最终两笔交易的最大收益
        int total_max_profit = INT_MIN;
        for(int i = 1; i < day_num; ++i){
            // 计算第一笔交易发生在[0,i],第二笔交易发生在[i+1,day_num-1]的情况下的最大收益
            int profit1 = max_profit_list[i];
            int profit2 = (i == day_num-1) ? 0 : second_max_profit_list[i+1];
            int total_profit = profit1 + profit2;
            if(total_profit > total_max_profit)
                total_max_profit = total_profit;
        }
        return total_max_profit;
    }
};
```

执行用时: 8 ms, 在所有 C++ 提交中击败了 87.44% 的用户

内存消耗: 15.8 MB, 在所有 C++ 提交中击败了 12.94% 的用户

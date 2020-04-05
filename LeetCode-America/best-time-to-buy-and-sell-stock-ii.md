## LeetCode : Best Time to Buy and Sell Stock Ⅱ

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

**Example 2:**

```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```

**Example 3:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```


## Solve

使用两个变量： `buyprice` `sellprice` 分别代表买入价格和卖出价格。每当卖出价格大于买入价格时，相减计算所得利润。

有人会问：如果是 `[7, 1, 5, 3, 6, 8]` 中的 6 既是卖出价格又是买入价格，这不合题意啊。

我只能说：你只看到了第一层，而我已经在第五层了。

```
This is what you think :       5 - 1 = 4, 6 - 3 = 3, 7 - 6 = 1;   4 + 3 + 1 = 8
But the answer actually is :   5 - 1 = 4, 7 - 3 = 4;             4 + 4 = 8
```

我就是这么回答老外的。嘻嘻。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int i = 0;
        int size = prices.size();
        if(size == 1 || size == 0) return 0;
        int buyprice = prices[0];
        int sellprice = prices[0];
        int profit = 0;
        while(i < size - 1) {
            while(i < size - 1 && prices[i] >= prices[i+1]) i++;
            buyprice = prices[i];
            while(i < size - 1 && prices[i] <= prices[i+1]) i++;
            sellprice = prices[i];
            profit += (sellprice - buyprice);
        }
        return profit;
    }
};
```

* Time complexity : **O(n)**
* Space complexity : **O(1)**

**Optimizing:**

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if(size == 1 || size == 0) return 0;
        int maxprofit = 0;
        for(int i = 0; i < size - 1; i++) {
            if(prices[i+1] > prices[i]) {
                maxprofit += (prices[i+1] - prices[i]);
            }
        }
        return maxprofit;
    }
};
```

* Time complexity : **O(n)**
* Space complexity : **O(1)**



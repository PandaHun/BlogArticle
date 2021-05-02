## Problem

Say you have an array `prices` for which the _i_th element is the price of a given stock on day _i_.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

**Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

## Example

```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```

## Code

```
import java.util.HashSet;

class Solution {
        public int maxProfit(int[] prices) {
        int price = prices[0];
        int profit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (price < prices[i]) {
                profit += prices[i] - price;
            }
            price = prices[i];
        }
        return profit;
    }
}
```

전날 가격 차이만큼 벌어드린 총 합을 구하면 되는 간단한 문제였습니다.

---

## Link

[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
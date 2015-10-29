# Most Stock Profit

出处

Given an integer array, each element is a stock price on day i (i is the index). Design an algorithm to find the maximum profit. You may complete at most two transactions(one buy, one sell).

## Solution

如果当前节点的解，既依赖于前驱问题的解，又依赖于后驱问题的解，但这两部分又互相独立，则可以分别自左开始DP，计算从最左节点到当前节点的结果；自右开始DP，计算从最右节点到当前节点的结果；再用同一个DP Table来合并解。

假设在i位置买入，j位置卖出，那么对于i，j之间的某个节点，如何计算其利润？可以分成两部分计算：从i到当前节点的利润，这部分只和前驱问题有关；从当前节点到j的利润，这部分只依赖于后驱问题。并且这两部分相互独立，可以把结果叠加在DP Table上。直观上说，相当于在当天卖出又立刻买进，相当于增加了两次虚拟操作。因此，可以以当前节点为分界线，第一次DP自左向右，只计算到当前为止可获得的最大收益(即从之前某天买入，当前卖出的最大收益)；第二次DP自右向左，计算从当前开始可获得的最大收益(即从当前买入，之后某天卖出的最大收益)。两部分收益之和即为总收益。

## Complexity

两轮遍历，时间复杂度 O(n)，空间复杂度 O(n)

## Code

```java
int maxProfit(int[] prices){
	int len = prices.length;
	if (len == 0) return 0;
	int[] dp = new int[len];
	// left dp
	int minPrice = prices[0];
	for (int i = 0; i < len; i++){
		dp[i] = prices[i] - minPrice;
		if (minPrice > prices[i]){
			minPrice - prices[i];
		}
	}
	
	int global = 0;
	
	// right dp
	int maxPrice = prices[len-1];
	for (int i = len-1; i >= 0; i++){
		if (maxPrice < prices[i]){
			maxPrice = prices[i];
		}
		dp[i] += maxPrice - prices[i];
		global = Math.max(global, dp[i]);
	}
	return global;
}
```

```C++
#include <iostream>
#include <queue>
#include <vector>
#include <algorithm>

using namespace std;

struct Node {
	int level;  // 当前节点所处的层级（对应物品的索引）
	int profit;  // 到达当前节点时已经获得的总价值
	int weight;  // 到达当前节点时已经使用的总重量
	float bound;  // 当前节点的上界值
	
	Node(int l, int p, int w) : level(l), profit(p), weight(w) {
		bound = 0.0;
	}
};

struct CompareNode {
	bool operator()(const Node& n1, const Node& n2) {
		return n1.bound < n2.bound;
	}
};
// 计算节点的上界值
float getBound(Node u, int n, int W, vector<int>& wt, vector<int>& val) {
	if (u.weight >= W) {
		return 0.0;  // 如果当前节点的重量已经超过背包容量，则上界值为0
	}
	
	float bound = u.profit;  // 初始化上界值为当前节点的价值
	int j = u.level;
	int totWeight = u.weight;
	
	// 尽可能装入后续物品，直到超过背包容量为止
	while (j < n && totWeight + wt[j] <= W) {
		totWeight += wt[j];
		bound += val[j];
		j++;
	}
	
	// 如果还有剩余物品未装入，则按照单位重量价值装入部分物品
	if (j < n) {
		bound += (W - totWeight) * ((double)val[j] / wt[j]);
	}
	
	return bound;  // 返回计算得到的上界值
}


// 计算0-1背包问题的最大价值
int knapsack(int W, vector<int>& wt, vector<int>& val) {
	int n = wt.size();  // 物品的数量
	priority_queue<Node, vector<Node>, CompareNode> pq;  // 优先队列，提供初始元素的容器是vector。在队列中用函数对象对 vector 元素的副本排序。
	

	
	int maxProfit = 0;  // 最大价值
	Node u(0, 0, 0);  // 初始化根节点
	Node v(0, 0, 0);
	
	u.bound = 0.0;  // 计算根节点的上界值
	pq.push(u);  // 将根节点加入优先队列
	
	while (!pq.empty()) {
		u = pq.top();  // 获取优先队列中的节点
		pq.pop();  // 弹出队首节点
		
		if (u.level == n) {
			continue;  // 如果已经遍历完所有物品，则跳过
		}
		
		v.level = u.level + 1;  // 扩展出左子节点
		v.weight = u.weight + wt[v.level - 1];
		v.profit = u.profit + val[v.level - 1];
		
		// 如果当前节点的价值大于已知的最大价值，则更新最大价值
		if (v.weight <= W && v.profit > maxProfit) {
			maxProfit = v.profit;
		}
		
		v.bound = getBound(v, n, W, wt, val);  // 计算左子节点的上界值
		
		if (v.bound > maxProfit) {
			pq.push(v);  // 如果左子节点的上界值大于当前最大价值，则加入优先队列
		}
		
		v.weight = u.weight;  // 扩展出右子节点
		v.profit = u.profit;
		v.bound = getBound(v, n, W, wt, val);  // 计算右子节点的上界值
		
		if (v.bound > maxProfit) {
			pq.push(v);  // 如果右子节点的上界值大于当前最大价值，则加入优先队列
		}
	}
	
	return maxProfit;  // 返回最大价值
}


int main() {
	vector<int> weights = {3, 5, 7, 8, 20};
	vector<int> values = {4, 6, 7, 9, 10};
	int W = 22;  // 背包的容量
	
	int maxProfit = knapsack(W, weights, values);  // 调用分枝—限界算法求解最大价值
	cout << "Maximum profit: " << maxProfit << endl;  // 输出最大价值
	
	return 0;
}

```


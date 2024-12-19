# dfs-and-bfs
The difference between dfs and bfs.
![圖表](https://github.com/ryan0225/dfs-and-bfs/blob/main/different.png)

以下是我實做出來的dfs跟bfs程式碼，主要作用是遍歷一個無權無向圖!
dfs
```cpp
#include <iostream>
#include <vector>
using namespace std;

// 必須加&(傳址參數)，不然不同遞迴層的資料會不公通
void dfs(int note, vector<vector<int>>& graph, vector<bool>& eee) {
	eee[note] = true; // 代表我來過
	cout << note << " "; // 輸出節點
	for (int nbr : graph[note]) { // 查看此節點所連通的全部節點
		if (!eee[nbr]) { // 如果此節點沒有拜訪過，就以這節點往下繼續看
			dfs(nbr,graph,eee);
		}
	}
}

int main () {
	vector<vector<int>> graph(5); //5個節點
	vector<bool> eee(5,false); //一開始皆沒到訪
	graph[0] = {1,2,4}; //這是無向圖
	graph[1] = {0,3};
	graph[2] = {0,3,4};
	graph[3] = {1,2};
	graph[4] = {0,2};
	dfs(0,graph,eee);
}
```

bfs
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

// 必須加&(傳址參數)，不然不同遞迴層的資料會不公通
void bfs(int go, vector<vector<int>>& graph, vector<bool>& eee) {
	queue<int> q; // 建立一個佇立(bfs基本都是由佇立去製作)
	q.push(go); // 把當前節點放進佇立中
	eee[go] = true; // 我來過
	while(!q.empty()) { // 若是佇立空了 就離開循環
		int note = q.front(); // 把佇立中第一個資料存進note
		q.pop(); // 把佇立中第一個資料刪掉
		cout << note << " "; // 輸出節點
		for (int nbr : graph[note]) { // 查看此節點所連通的全部節點
			if (!eee[nbr]) { // 如果此節點沒有拜訪過，就以這節點往下繼續看
				q.push(nbr); // 把它放進佇立中
				eee[nbr] = true; // 我來過
			}
		}
	}
}

int main () {
	vector<vector<int>> graph(5); //5個節點
	vector<bool> eee(5,false); //一開始皆沒到訪
	graph[0] = {1,2,4}; //這是無向圖
	graph[1] = {0,3};
	graph[2] = {0,3,4};
	graph[3] = {1,2};
	graph[4] = {0,2};
	bfs(0,graph,eee);
}
```

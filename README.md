# dfs-and-bfs 還有 dp
它們的區別~
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
# 在void dfs那行，為何要在二維向量那邊加入&呢?
先來講講有沒有加入的區別好了(某種程度上來說，我覺得這跟區域變數還有全域變數的概念挺像的)
沒有加&我們稱其為<string>Call by Value</string>
會建立一個複製品，在裡面做修改並不會影響到其他地方，他只在他那層那個區塊才會修改。我們一開始在學自製函式時基本上都是用此方式去製作。
有加&我們稱為<string>Call by Reference</string>
他則是引用原本的變數，你在自製函式中修改他，亦會連帶著修改到原本變數的數值。
# 在這裡我們為何要用Call by Reference呢?
因為這是一個遞迴的結構，我們需要讓裡面的每一層得到的消息皆相同，不然當一條路下到底部開始慢慢回朔往上時，便有可能再走一次已經去過的節點。
就是說每個層級戶不知道其他層級去了哪些層級，這樣明顯是不行的。
而使用call by reference就沒這問題了，因為eee用的都是最一開始的，有改變時每個層級都會收到訊息

# 程式碼下方的圖長這樣
![未命名繪圖 (2) (1)](https://github.com/user-attachments/assets/6e8c59a8-2d7e-4de6-bd99-430dd908509c)



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

dp
用來解AIME 2 problem 8
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
using namespace std;

int main () {
	int n = 1000;  //1~n的錢
	vector<int> d(1,0);  //d用來存greedy的結果
	vector<int> coins = {1,10,25};  //幣值
	vector<int> rev_coins(coins.size());  //用來算greedy
	vector<int> dp(n+1,INT_MAX);  //DP算結果(全局最佳解)
	dp[0] = 0;
	//DP
	for (int i=1;i<=n;i++) {
		for (auto& j : coins) {
			if (i >= j) {
				dp[i] = min(dp[i],dp[i-j]+1);
			}
		}
	}
	
	//greedy
	reverse_copy(coins.begin(),coins.end(),rev_coins.begin());
	for (int i=1;i<=n;i++) {
		int sum = 0;
		int coin;
		int flag = i;
		for (auto& j : rev_coins) {
			coin = flag / j;
  			sum += coin;
  			flag = flag % j;
  	    }
  	d.push_back(sum);
	}
	
	int wrong = 0;
	for (int i=1;i<=n;i++) if (d[i] != dp[i]) wrong++;
	cout << "wrong " << wrong;
}
```

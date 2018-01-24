##LCA

least common ancestor

트리의 공통 조상을 찾는 알고리즘이다.

트리의 공통 조상을 찾는 단순한 사실만 놓고 본다면 이 알고리즘을 굳이 왜 알아야 하나 싶다.

하지만 가중치가 있는 트리에서 임의의 두 노드 사이의 가중치의 합을 구하고자 하면 일단 두 노드의 공통 부모를 찾고, 그것을 기반으로 결과를 계산해야 한다.

	#include <iostream>
	#include <vector>
	#include <queue>
	using namespace std;
	
	vector<int> tree[50001];
	vector<int> depth(50001 , 0);
	vector<int> parent(50001 , 0);
	vector<bool> isChecked(50001, false);
	
	int main()
	{
	    int nodeNum, LCAProblemNum;
	    cin >> nodeNum;
	
	    for(int i = 0; i < nodeNum - 1; i++){
	        int start, end;
	        cin >> start >> end;
	
	        tree[start].push_back(end);
	        tree[end].push_back(start);
	    }
	
	    queue<int> q;
	    q.push(1);
	
	    while(!q.empty()) {
	        int current = q.front();
	        q.pop();
	
	        isChecked[current] = true;
	
	        for(auto ele : tree[current]){
	            if(!isChecked[ele]) {
	                depth[ele] = depth[current] + 1;
	                parent[ele] = current;
	                q.push(ele);
	            }
	        }
	    }
	
	    cin >> LCAProblemNum;
	
	    for(int i = 0; i < LCAProblemNum; i++) {
	        int node1, node2;
	        cin >> node1 >> node2;
	
	        int node1Depth = depth[node1];
	        int node2Depth = depth[node2];
	
	        while(node1Depth != node2Depth) {
	            if(node1Depth > node2Depth) {
	                node1 = parent[node1];
	                node1Depth = depth[node1];
	            }else if(node1Depth < node2Depth){
	                node2 = parent[node2];
	                node2Depth = depth[node2];
	            }
	        }
	
	        if(node1 == node2) {
	            cout << node1 << '\n';
	        }else {
	            while(parent[node1] != parent[node2]) {
	                node1 = parent[node1];
	                node2 = parent[node2];
	            }
	            cout << parent[node1] << '\n';
	        }
	    }
	
	    return 0;
	}

이런식으로 공통 부모를 구할 수 있다.

일단 각각의 depth를 구하고, depth가 다르다면 depth가 깊은 쪽을 낮은쪽으로 depth를 같도록 맞춰준다.

그리고 나서 한칸씩 depth를 올리면서 공통된 부모를 만날 때 가지 계산한다.

이것을 기반으로 임의의 두 노드 사이의 거리를 구할 수 있다.

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>    // std::swap
using namespace std;
const int MAX = 40001;

struct Node {
	int to, cost;
    Node(){
	}
	Node(int to, int cost) : to(to), cost(cost) {
	}
};

vector<Node> tree[MAX];
vector<int> depth(MAX , 0);
vector<Node> parent(MAX);
vector<bool> isChecked(MAX, false);

int main()
{
    int nodeNum, LCAProblemNum;
    cin >> nodeNum;

    for(int i = 0; i < nodeNum - 1; i++) {
        int start, end, cost;
        cin >> start >> end >> cost;

        tree[start].push_back(Node(end, cost));
        tree[end].push_back(Node(start, cost));
    }

    queue<int> q;
    q.push(1);

    while(!q.empty()) {
        int current = q.front();
        q.pop();

        isChecked[current] = true;

        for(Node ele : tree[current]){
            if(!isChecked[ele.to]) {
                depth[ele.to] = depth[current] + 1;
                parent[ele.to] = Node(current, ele.cost);
                q.push(ele.to);
            }
        }
    }

    cin >> LCAProblemNum;

    for(int i = 0; i < LCAProblemNum; i++) {
        int node1, node2;
        cin >> node1 >> node2;

        int node1Depth = depth[node1];
        int node2Depth = depth[node2];

        int node1Costsum = 0;
        int node2Costsum = 0;

        if( node2Depth < node1Depth ) {
            swap(node2, node1);
            swap(node2Depth, node1Depth);
        }

        while(node1Depth != node2Depth) {
            node2Costsum += parent[node2].cost;
            node2 = parent[node2].to;
            node2Depth = depth[node2];
        }
        
        if(node1 == node2) {
            cout << node2Costsum << '\n';
        }else {
            while(node1 != node2) {
                node1Costsum += parent[node1].cost;
                node1 = parent[node1].to;
                node2Costsum += parent[node2].cost;
                node2 = parent[node2].to;
            }
            cout << node1Costsum + node2Costsum<< '\n';
        }
    }

    return 0;
}
```

이렇게 부모배열에 자신의 부모를 저장 해 두고, 부모 배열의 원소로 그 부모까지 가는 가중치와 부모의 num을 저장 해 두면 공통 조상을 구하면서 두 점 사이의 가중치를 구해서 결과값을 낼 수 있다.
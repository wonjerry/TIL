## Tree

### 정의
사이클이 없는 연결 '그래프'이다. 그래프 내부에 트리가 속한다.

### 루트 있는 트리
상위에 있는것이 루트가 아니다. 부모가 없는 노드가 루트노드이다.

### Depth
루트로부터의 거리를 뜻한다. 루트의 깊이를 0으로 하는 경우가 있고, 1로 하는 경우가 있다.
![Alt text](./treeDepth.png)

### Height
 깊이중 가장 큰 값을 뜻한다. 위의 사진에서 2 또는 3이 이에 속한다.

### 조상, 자손
p -> q가 가능할 때, p가 루트에 더 가깝다면 p는 q의 조상, q는 p의 자손이다.

### 이진트리
자식을 최대 2개만 가지고 있는 트리

### 트리의 표현 방법
- 트리는 그래프에 속하기 때문에 그래프 표현과 같은 방식으로 저장할 수 있다.
- 트리의 모든 노드는 부모를 하나 또는 0개만 가지기 때문에 부모만 저장하는 방식으로 저장할 수 있다.
- 부모가 0개인 경우는 트리의 루트인데 이 경우 부모를 -1 또는 0으로 처리하는 방식을 사용한다. 또는 null 로 표현할 수도 있겠다.
- 이진 트리의 경우에는 배열로 표현 가능하다.
	- 부모노드가 x인 경우, 자식 노드는 2*x, 2*x+1로 나타내면 된다.

### 트리의 순회
- 트리의 모든 노드를 방문하는 순서이다.
- 그래프의 경우에는 DFS, BFS가 있다.
- 트리에서도 위 두 방법을 사용할 수 있지만 트리에서만 사용할 수 있는 세가지 방법이 있다.
	- pre-order
		- 노드 방문
		- 왼쪽 자식을 루트로 하는 서브 트리 프리오더
		- 오른쪽 자식을 루트로 하는 서브 트리 프리오더
		- 처음 방문한 노드를 출력하고, 서브 트리들의 루트로 가서 또 처음 방문하는 루트를 출력하는 방식이다.
		- 그냥 나 왼 오 순서로 방문하면서 방문 하는대로 출력한다.
		- 부모부터 출력하고 그다음에 자식 출력한다. 그래서 pre.
		- 그래프에서 DFS와 순서가 같다.
	- in-order
		- 왼쪽 자식 노드를 루트로 하는 서브 트리 인오더
		- 노드방문
		- 오른쪽 자식 노드를 루트로 하는 서브 트리 인오더
		- 쭉 내려가서 왼쪽 나 오른쪽 출력하고 올라와서 나 출력하고 오른쪽으로 가서 왼쪽 나 오른쪽 출력한다.
		- 쭉 내려가서 왼쪽부터 출력, 그다음 나 그다음 오른쪽 출력 즉 맨 하위의 서브트리부터 출력하면서 올라온다.
	- post-order
		- 왼쪽 자식 노드를 루트로 하는 서브 트리 포스트오더
		- 오른쪽 자식 노드를 루트로 하는 서브 트리 포스트오더
		- 노드방문
		- 맨 아래로 내려가서 자식부터 출력하고 그들의 부모 출력하고 올라온다. 그래서 post
- 세 방법의 차이는 노드 방문을 언제하느냐의 차이다.

```cpp

	#include <iostream>
	
	using namespace std;
	
	int tree[50][2];
	
	
	void preorder(int index)
	{
	    if(index == -1) return;
	
	    cout << (char) (index + 'A');
	    preorder(tree[index][0]);
	    preorder(tree[index][1]);
	}
	
	void inorder(int index)
	{
	    if(index == -1) return;
	
	    inorder(tree[index][0]);
	    cout << (char) (index + 'A');
	    inorder(tree[index][1]);
	}
	
	void postorder(int index)
	{
	    if(index == -1) return;
	
	    postorder(tree[index][0]);
	    postorder(tree[index][1]);
	    cout << (char) (index + 'A');
	}
	
	int main()
	{
	    int n;
	    cin >> n;
	
	    for (int i = 0; i < n; i++)
	    {
	        char me, left, right;
	        cin >> me >> left >> right;
	        me = me - 'A';
	        if (left == '.')
	        {
	            tree[me][0] = -1;
	        }else{
	            tree[me][0] = left - 'A';
	        }
	
	        if (right == '.')
	        {
	            tree[me][1] = -1;
	        }else{
	            tree[me][1] = right - 'A';
	        }
	    }
	    //pre order
	    preorder(0);
	    cout << '\n';
	    //in order
	    inorder(0);
	    cout << '\n';
	    // post order
	    postorder(0);
	    return 0;
	}
```

### 트리의 탐색
- 트리의 탐색은 DFS/BFS 알고리즘을 이용해서 할 수 있다.
- 트리는 사이클이 없는 그래프이기 때문에 임의의 두 정점 사이의 경로는 1개이다.
- 따라서, BFS  알고리즘을 사용하면 각 층? 에 있는 노드들을 먼저 탐색 해 보면서 최단 거리를 구할 수 있다.
- 이유 : 각 노드간 연결된 경로가 1개라 찾은 그 경로가 최단 경로이기 때문이다.

#### 트리의 부모 찾기 문제
그래프로 트리를 입력받고, 루트를 1이라고 정했을 때 각 노드의 부모를 찾는 문제

```cpp
	#include <algorithm>
	#include <iostream>
	#include <vector>
	#include <queue>
	
	using namespace std;
	
	vector<int> a[100001];
	int parent[100001];
	bool check[100001];
	
	void bfs(int start) {
		queue<int> q;
		q.push(start);
		check[start] = true;
		while (!q.empty()) {
			int node = q.front();
			q.pop();
	
			for (int y : a[node]) {
				if (check[y] == false) {
					parent[y] = node;
					check[y] = true;
					q.push(y);
				}
			}
		}
	}
	
	int main() {
		int n;
		cin >> n;
	
		for (int i = 1; i < n; i++) {
			int s, e;
			cin >> s >> e;
			a[s].push_back(e);
			a[e].push_back(s);
		}
	
		bfs(1);
	
		for (int i = 2; i <= n; i++) {
			printf("%d\n",parent[i]);
		}
	    
	    return 0;
	}
```

- 트리 구조를 생각 해 봤을 때 bfs 탐색을 하면 각 층의 상위 노드들은 고정 되어있으므로 차례차례 내려가면서 자신의 부모를 체크할 수 있다.

#### 트리의 지름
- 트리에 존재하는 모든 경로 중에서 가장 긴 것의 길이를 트리의 지름이라고 한다.
- 트리의 지름은 탐색 2번으로 알 수 있다.
- 루트에서 모든 정점까지의 길이를 구하고, 그중 길이가 최대인 정점을 저장 해 둔다.
- 최대 길이의 정점에서 다시 그곳을 기점으로 가장 긴 길이를 구한다.
- 이렇게 하면 트리의 지름을 알 수 있다.

// 트리 지름 코드 최적화 후 진행하기
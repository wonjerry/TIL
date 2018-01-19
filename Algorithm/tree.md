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




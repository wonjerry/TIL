## LCA2
### LCA는 시간 복잡도가 꽤나 크다.
- 일단 bfs 도는 시간은 각 노드를 방문하면서 연결되어있는 모든 노드를 일단 순회하기 때문에 최악의 경우 O(N) 만큼의 시간 복잡도가 나온다.
- 그리고 depth를 맞춰주기 위해서 O(1)의 시간이 걸린다.
- 그리고 한칸씩 올라가면서 맞추기 위해서 또 약 O(1)의 시간이 걸린다.
- 위의 두 시간은 그냥 더해지기 때문에 O(N + 1 + 1) = O(N) 만큼 걸린다.
- 이 방식은 depth를 맞추기 위해 한칸씩 올려보고, 또 공통 부모를 찾기위해서 한칸씩 올려 나가는데 꽤나 시간이 걸리고, 하나하나 탐색하기 때문에 N이 커지면 점점 커진자.

 ### LCA를 보완하기 위해 나온 방법
 - 기존의 LCA는 한칸한칸 올라가면서 부모가 같은지를 탐색했다.
 - 이제는 2^K 만큼 훌쩍 훌쩍 뛰어 넘어 검색한다.
 - 이런 방법을 사용하면 시간복잡도가 O(logN)까지 줄어든다.
 
 ### 구체적인 방법
 1. 일단 문제에서 각 노드끼리 연결된 경로는 단 하나라고 한다면 tree이다.
 2. tree 구조를 위해서 각 노드를 bfs로 돌면서 부모 노드를 저장 해 둔다.
 3. p[i][j]라는 배열을 만드는데, 이 배열은 각 원소가 노드i의 2^j 번째 부모를 저장 해 둔다.
 4. 두개의 노드를 입력 받았다고 가정하자
 5. 두 노드중 depth가 깊은 노드를 기준으로 한다.
 6. depth가 깊은 노드에 대해서 그것보다 작거나 같은 2의k승을 구한다.
 7. k승을 구했으면 그것에서 1을 뺀다.
  - 왜냐하면 해당 노드에서 K 만큼 depth를 타고 올라가면 root노드를 넘기 때문이다.
 8. 루프를 돌며 k에 대해서 비트연산을 통해서 u의 depth에서 2의k승을 뺐을 때 v의 depth를 넘지 않는다면 u = p[u][i] 로 u를 업데이트 시킨다.
 >
 ```cpp
 for (int i=log; i>=0; i--) {
        if (depth[u] - (1<<i) >= depth[v]) {
            u = memoiztion[u][i];
        }
    }
 ```
	- v의  depth와 같은 것이 아니라, 크거나 같을때가 조건인 이유는 2의 i승으로 depth를 이동시키다 보면 v의 depth보다 더 작아지는 경우가 있고, 그런 경우는 뛰어 넘고, 다시 업데이트된 u에 대해서 v와 같아질 때 까지 aim을 맞추기 위해서이다.


 9. 만약  u와 v가 같아졌으면 u를 return 한다.
 10. 아니라면 2의 k승으로 계속 u와 v가 같아질 때 까지 depth를 조정하면서 공통 부모의 바로 아래 depth까지 이동시킨다.
 11. u의 parent를 return 하여 공통 조상을 리턴한다.

``` cpp
if (u == v) {
        return u;
} else {
    for (int i=log; i>=0; i--) {
        if (memoiztion[u][i] != 0 && memoiztion[u][i] != memoiztion[v][i]) {
            u = memoiztion[u][i];
            v = memoiztion[v][i];
        }
    }
    return parent[u];
}
```

### 깨달은 점
맨 처음에는 그냥 부모의 부모의 부모를 다 돌면서 저장하는 것으로 생각해서 많이 틀렸다.
그러나 2의k승 번째 조상을 저장하고 탐색한다는 것을 깨닫고는 제대로 곰곰히 생각 해서 해결했다.

이 알고리즘에서 중요한 점은 
- tree를 위해서는 bfs 연산을 해야된다.
- 한 칸씩 비교 하던것을 2의 k승만큼 뛰어 넘으면서 검색하는 것
- 만약 뛰어 넘었을 때 target depth보다 크다면 log를 1씩 줄여가면서 aim을 맞추는 것
- log는 depth를 2의 k승씩 올라가면서 검색하기 위해 저장하는 것이다.

이렇게 두 가지이다.

어떤 관점으로 보자면 이렇게 탐색하는 것은 binary search를 하는 것과 비슷하다고 볼 수 있다.

그리고 어떤 tartget depth에 대해서 2의 배수만큼 올라가다가 1개 차이나면 2의 0승때까지 다른 것들은 무시하다가 적용한다는 점에서 뭔가 새로운 것을 배웠다.

"단순히 부모를 저장하는 것이 아니고 2의 k승 번째 조상을 저장 함으로써 나중에 탐색할 때 2의 k승만큼 뛰어다니면서 검색할 수 있도록 한 것이다."

### 전체 코드

```cpp
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
const int MAX = 100001;
vector<int> a[MAX];
int parent[MAX];
bool check[MAX];
int depth[MAX];
int p[MAX][18];
int lca(int u, int v) {
    if (depth[u] < depth[v]) {
        swap(u,v);
    }
    int log = 1;
    for (log=1; (1<<log) <= depth[u]; log++);
    log-=1;
    for (int i=log; i>=0; i--) {
        if (depth[u] - (1<<i) >= depth[v]) {
            u = p[u][i];
        }
    }
    if (u == v) {
        return u;
    } else {
        for (int i=log; i>=0; i--) {
            if (p[u][i] != 0 && p[u][i] != p[v][i]) {
                u = p[u][i];
                v = p[v][i];
            }
        }
        return parent[u];
    }
}
int main() {
    int n;
    scanf("%d",&n);
    for (int i=0; i<n-1; i++) {
        int u,v;
        scanf("%d %d",&u,&v);
        a[u].push_back(v);
        a[v].push_back(u);
    }
    depth[1] = 0;
    check[1] = true;
    queue<int> q;
    q.push(1);
    parent[1] = 0;
    while (!q.empty()) {
        int x = q.front();
        q.pop();
        for (int y : a[x]) {
            if (!check[y]) {
                depth[y] = depth[x] + 1;
                check[y] = true;
                parent[y] = x;
                q.push(y);
            }
        }
    }
    for (int i=1; i<=n; i++) {
        p[i][0] = parent[i];
    }
    for (int j=1; (1<<j) < n; j++) {
        for (int i=1; i<=n; i++) {
            if (p[i][j-1] != 0) {
                p[i][j] = p[p[i][j-1]][j-1];
            }
        }
    }
    int m;
    scanf("%d",&m);
    while (m--) {
        int u, v;
        scanf("%d %d",&u,&v);
        printf("%d\n",lca(u, v));
    }
    return 0;
}
```
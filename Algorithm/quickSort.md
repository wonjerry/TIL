## Quick Sort

현업에서 가장 많이 사용하는 sort 함수 중 하나라고 한다.

c++은 stl sort 함수가 잘 되어있어서 이걸 쓰는게 좋지만, 공부하는 입장해서 조사하고 구현 해 본 것이다.

pivot을 계산하는 것에 따라 더 빨라질 수도 느려질 수도 있다. 지금은 그냥 중간 index 값을 계산하여 구했다.

### 알고리즘
- 퀵정렬은 분할 정복을 통해 데이터를 정렬한다.
- 리스트 가운데서 하나의 원소를 고른다. 이렇게 고른 원소를 pivot이라고 한다.
- 피벗을 기준으로 피벗 원소보다 큰 수는 뒤로, 작은수는 앞으로 가도록 리스트를 둘로 나눈다.
- 분할 된 두 작은 리스트에 대해 재귀적으로 이것을 수행한다.
- 퀵소트의 피벗은 최대한 데이터 중 중간값을 고르는 것이 최적화에 좋다. 그래야 피벗을 두고 
- 평균적인 경우
	- 시간복잡도 : Θ(NlogN)
	-      6
	-  2		2
- 최악의 경우 : O(n^2)
	- 최악의 경우는 언제일까?
	- 바로 피봇을 최소 또는 최댓값으로 선택한 경우이다.
		- 8 7 6 5 4 3 2 1 을 원소로 가지는 배열이 존재한다고 해보자.
		- 1을 피봇으로 뒀다고 가정하자.
		- stored index 즉 피봇보다 작은 수를 저장하고 있는 인덱스를 체크하면서 배열을 돈다.
		- 일단 8은 1보다 크므로 stored index를 키우지 않는다.
		- 그 이후에 숫자들도 마찬가지 이다.
		- 그렇다면 결국 2까지 다 돌았을 때 이동하는 원소는 없다.
		- stored index에 있는 8과 1을 교체한다.
		- storedIndex를 피봇으로, quick sort를 재귀적으로 다시 돈다.
		- 이때 stored index는 0이므로 low~pivot까지는 원소가 없으므로 왼쪽 퀵소트는 돌지 않고 오른쪽만 돌게 된다.
		- 이렇게 되면 n개의 원소에 대한 탐색을 n번 하므로 O(N^2)의 시간복잡도가 나오게 된다.
		- 6
			- 5
				- 4
					- 3
						- 2
							- 1


### 코드 진행 흐름
- 알고리즘을 코드로 구현 해 보자.

```cpp
#include <iostream>
#include <algorithm>
#include <cstdio>

using namespace std;

int arr[1000000];

int calPivot(int start, int end) {
    return (start + end) / 2;
}

int partition(int start, int end) {
    int pivot = calPivot(start, end);
    int pivotValue = arr[pivot];
    swap(arr[pivot], arr[end]);
    
    int storedIndex = start;

    for(int i = start; i < end; i++) {
        if(arr[i] < pivotValue) {
            swap(arr[i], arr[storedIndex]);
            storedIndex++;
        }
    }

    swap(arr[storedIndex], arr[end]);

    return storedIndex;
}

void quickSort(int start, int end) {
    if(start < end) {
        int pivot = partition(start, end);
        quickSort(start, pivot - 1);
        quickSort(pivot + 1, end);
    }
}

int main() {
    int num;
    scanf("%d", &num);

    for(int i = 0; i < num; i++) {
        scanf("%d", &arr[i]);
    }

    quickSort(0, num-1);

    for(int i = 0; i < num; i++) {
        printf("%d\n", arr[i]);
    }
    return 0;
}
```


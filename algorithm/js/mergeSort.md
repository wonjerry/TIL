## mergeSort
1. 한 배열을 중심을 기준으로 왼쪽 오른쪽 분리한다.( 왼쪽과 오른쪽 배열의 원소가 2개 남을 때 까지 분리한다.)
2. 왼쪽 오른쪽 배열의 원소들을 하나씩 비교하며 작은 것을 result 배열에 넣는다.
3. 정렬이 끝났을 때 왼쪽 또는 오른쪽 배열에 원소가 남아있다면 모두 result 배열에 넣어주어야 한다. 이때 js에서는 concat을 사용하면 수월하다.
4. arr를 리턴하면서 재귀적으로 돌면 정렬된 결과물이 나온다.

	function mergeSort(arr,valueParsingCallback) {
	  const len = arr.length;
	  if (len < 2) return arr;
	  if(!valueParsingCallback) valueParsingCallback = (ele)=>{return ele;};
	  const mid = Math.floor(len / 2);
	  let left = arr.slice(0, mid);
	  let right = arr.slice(mid, len);

	  return merge(mergeSort(left,valueParsingCallback), mergeSort(right,valueParsingCallback),valueParsingCallback);
	}

	function merge(left, right, valueParsingCallback) {
	  let result = [];
	  let l = 0;
	  let r = 0;

	  while (l < left.length && r < right.length) {
	    if (valueParsingCallback(left[l]) < valueParsingCallback(right[r])) {
	      result.push(left[l++]);
	    } else {
	      result.push(right[r++]);
	    }
	  }
	  return result.concat(left.slice(l,left.length)).concat(right.slice(r,right.length));
	}

callback은 혹시 obj array를 해당 obj의 property에 대해 sorting 하고 싶을 때 callback을 넣으면 가능하게 하였다.

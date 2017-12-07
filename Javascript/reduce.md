## reduce

underscore에서 제공하는 기능이다.
- 매개변수로 collection, iterator, accumulator를 받는다.
- collection은 object 또는 array이다.
- accumulator가 undefined이면 collection의 첫번째 값으로 지정한다.
- collection의 모든 원소를 돌며 각 원소에 대해 iterator를 수행하고 그 결과값을 total에서 받았다가 return 한다.
- 모든 원소를 더할때에도 사용되거나 모든 원소의 평균을 구한다거나 하는 등 배열이나 object의 값들을 한꺼번에 묶는다? 라는 개념으로 이해하면 활용도가 높아질듯 하다.

    _.reduce = function (collection, iterator, accumulator) {
	    var total = accumulator
	    var array = collection
	    var i = 0

	    if (!Array.isArray(array)) {
	      array = []
	      for (var key in collection) {
	        if (collection.hasOwnProperty(key)) {
	          array.push(collection[key])
	        }
	      }
	    }

	    if (accumulator === undefined) {
	      total = array[0]
	      i = 1
	    }

	    for (var len = array.length; i < len; i++) {
	      total = iterator(total, array[i])
	    }
	    return total
	  }

자 이렇게 내부를 살펴보면 total 값은 accumulator가 지정한 값 또는 array의 첫번째 원소를 가지고, iterator을 돌면서 total값이 업데이트 된다.

iterator의 내용은 대부분 모든 원소에 대한 것이 많다. reduce의 활용을 살펴보면 이해가 더 빠르다.

#### reduce의 활용 - contains

이 함수는 collection이 어떤 target 값을 가지는지 아닌지를 파악해서 true 또는 false 값을 리턴하는 함수이다.

    _.contains = function (collection, target) {
	    return _.reduce(collection, function (wasFound, item) {
	      if (wasFound) {
	        return true
	      }
	      return item === target
	    }, false)
	  }

이렇게 보면 accumulator에 false 값을 주고, iterator로 해당 item이 있는지 없는지를 체크하는 로직을 넣어줌으로써 contain이라는 기능을 reduce로 손쉽게 구현이 가능하였다.

이 밖에도 많은 함수가 reduce를 활용한 것이 많고 앞으로 내가 어떤 기능을 구현할 때도 reduce를 사용해서 구현할 부분이 있을 것 같다.
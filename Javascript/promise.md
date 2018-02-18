## Promise

### Why we use promise??
1. callback hell을 벗어나기 위해
	- 우리는 비동기로직을 처리하기위해 이전에는 callback을 사용했다.
	- 비동기 테스크가 끝나면 그 후에 필요한 일들을 callback을 통해 처리하도록 하기위해서다.
	- 그런데 만약 비동기 테스크가 n개 있고, 그 n개가 각각의 실행 이후에 다음것이 실행되어야 한다면??
	- 우리는 depth가 n인 callback hell을 맞이하게 될 것이다.
	- 이것을 효과적으로 처리하기 위한 것이 프로미스이다.
	- 어떤 비동기 테스크를 처리한 후에, return을 통해 그 결과값 또는 promise를 리턴 해 주고, promise chaining을 통해 계속해서 처리 해 준다면 우리는 항상 depth가 1인 안정적인 구조의 비동기 처리를 할 수 있을것이다.
```javascript
request("https://www.wonjerry.com/user")
.then((user) => {
	return request(`https://www.wonjerry.com/user/${user.id}`)
}).then((userId) => {
	console.log(userId);
});
```

이런식으로 말이다. 

2. 예외 처리를 효과적으로 하기 위해
	- 우리가 만약 callback hell로 어떤 처리를 해버렸다고 가정하자.
	- 각각의 비동기처리에서 에러가 발생한다면, 우리는 각 error를 catch하기 위해서 callback으로 넘겨주는 함수 내부에 일일히 error를 받아야 할 것이다.
	- 그러나 promise chain 말미에 .catch로 error를 받고 처리할 수 있다면 훨씬 가독성 좋고 안정적인 비동기 처리가 가능할 것이다.

### Rookie Mistakes
나같은 주니어 개발자들은 promise에 대한 이해가 부족하여 여러가지 실수를 하게 된다. 어떤 실수를 하게 되는지 살펴보자.

1. How do I use forEach with promises?
	- 일반적으로 반복을 다루는 일을 할 때 forEach, for, while등 우리가 기존에 알고 있는 문법을 사용한다.
	- 그리고 이런 것들을 promise와 사용할 때 어떻게 해야할지 모른다.

```javascript
// I want to remove() all docs
db.allDocs({include_docs: true}).then(function (result) {
  result.rows.forEach(function (row) {
    db.remove(row.doc);  
  });
}).then(function () {
  // I naively believe all docs have been removed() now!
});
```

이 코드를 살펴보자. allDocs에서 result를 받아서 remove를 시키고 있다. 그러나 첫번쨰 then이 undefined를 리턴하기 때문에 두번째 then은 같은 result 값을 받을 것이다. 그리고 forEach가 실행되자마자 몇개의 doc만 삭제되고 나서 바로 두번째 then이 실행될 것이다.

return이 없어서 해당 동작에 대한 확신을 못하게 되는 것이다.

```javascript
db.allDocs({include_docs: true}).then(function (result) {
  return Promise.all(result.rows.map(function (row) {
    return db.remove(row.doc);
  }));
}).then(function (arrayOfResults) {
  // All docs have really been removed() now!
});
```

이렇게 promise.all을 사용하게 되면, 어떤 동작에 대한 결과 promise들의 array를 다음 then으로 return 하게 되고, 두번째 then에서는 그것들을 체크할 수 있다. 그리고 원소들중 하나라도 reject된다면 바로 catch문으로 넘어가거나 함으로 다음 then이 실행되지 않는다.

2. Forgetting to add .catch
catch를 사용하지 않는다면 만약 에러가 발생했을 때 어떠한 피드백도 받지 못할 것이다. 따라서 적어도 promise chain 마지막에 .catch를 붙여서 어떤 error가 발생하는지 출력이라도 하는 코드를 작성하자.

3. Using side effects instead of returning
```javascript
somePromise().then(function () {
  someOtherPromise();
}).then(function () {
  // Gee, I hope someOtherPromise() has resolved!
  // Spoiler alert: it hasn't.
});
```

이코드에는 문제가 있다. 첫번째 then이 실행되고나서 그 안의 someOtherPromise가 실행됨과 동시 또는 더 빠르게 또는 바로 직후에 다음 then이 실행되게 된다.

이렇게 값을 리턴하지 않는 then에 대해서, 그 로직의 확실성을 보장할 수 없고, 다음번 then은 인자값으로 undefined를 받게 되므로 안정적이지 못하다.

promise를 리턴하거나, 어떤 결과값을 리턴함으로써 동작을 보장하는 습관을 가지자.
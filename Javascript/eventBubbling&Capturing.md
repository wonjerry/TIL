## 이벤트 버블링 캡쳐링

어떤 상황을 가정해 보자. div가 3개가 겹쳐져 있고, 제일 바깥쪽이 depth1(최상위 부모), 그다음이 depth2(중간 부모), 제일 내부가 depth3(막내)이다.

그리고 각각의 div에 click 이벤트를 걸고 one, two, three를 출력하게 하고 depth3를 클릭했다면 어떤일이 벌어질까??

정답은 three, two, one이다. 왜 그런줄 알겠는가??

HTML 이벤트 모델이 실행되는 두 단계가 있다.

1. 이벤트 캡쳐링
- 이벤트 캡쳐링은 만약 최 하위 element를 클릭했다면 그 element의 최상단 element부터 타고 내려가면서 캡쳐링 이벤트가 걸려있는지를 확인하고 실행시키면서 내려간다.

2. 이벤트 버블링
- 위 상황과 마찬가지로 최하위 element를 클릭했다고 가정해보자.
- 우리가 일반적으로 addEventListener 하는 것은 대부분 버블링 이벤트이다.
- 최 하위 element가 클릭되면 그 element의 이벤트가 먼저 실행되고 그 element의 부모 element를 탐색하면서 등록된 이벤트가 존재하는지 확인하고 모두 실행시킨다.
- 이 과정이 마치 탄산 음료의 거품처럼 올라온다고 하여 이벤트 버블링이라고 부른다.

```javascript
	var firstElement = document.getElementById('first');
	var secondElement = document.getElementById('second');
	var thirdElement = document.getElementById('third');

	firstElement.addEventListener('click', function () {
	  console.log('Capturing: ' + firstElement.id);
	}, true);

	secondElement.addEventListener('click', function () {
	  console.log('Capturing: ' + secondElement.id);
	}, true);

	thirdElement.addEventListener('click', function () {
	  console.log('Capturing: ' + thirdElement.id);
	}, true);

	firstElement.addEventListener('click', function () {
	  console.log('Bubbling: ' + firstElement.id);
	});

	secondElement.addEventListener('click', function () {
	  console.log('Bubbling: ' + secondElement.id);
	});

	thirdElement.addEventListener('click', function () {
	  console.log('Bubbling: ' + thirdElement.id);
	});
```

html에 div를 3개 중첩하고 가장 바깥쪽 div 부터 id를 first, second, third라고 명명하자.

second를 클릭하면
- first의 캡쳐링 이벤트 발생
- second의 캡쳐링 이벤트 발생
- second의 버블링 이벤트 발생
- first의 버블링 이벤트 발생

이런 단계로 이벤트가 발생한다. 앞에서 설명한 부분을 이해했다면 금방 추척할 수 있을 것이다.

그런데 만약 depth가 (그럴일은 없겠지만) 100인 element에 이벤트를 걸어 놓았다고 생각해보자. 만약 버블링이 발생하면 element의 이벤트가 발생된 이후에 쓸데없는 100개의 이벤트 탐색이 이루어진다.

이런 쓸데없은 추적을 막기 위해서 stopPropagation()이라는 메소드가 event 객체에 존재한다.

이 메소드를 실행시키면 더 이상 캡쳐링 또는 버블링이 발생하지 않고 거기서 이벤트가 멈춘다.


---
[출처](http://blog.javarouka.me/2011/12/html-event-bubbling.html)
[예제 출처](www.vanillacoding.co)


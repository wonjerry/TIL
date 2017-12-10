## 자바스크립트 디자인 패턴 - mixin

자바스크립트의 객체 상속 관련 패턴 중 클래스 상속과 프로토타입 상속 이외에 한 객체의 프로퍼티를 다른 객체로 '복사'하여 사용하는 방식이다.

간단하게 생각하면 어떤 객체를 상속 한다고 가정해보자

우리는 extends라는 확장 함수를 만들고 source, destination을 매개 인자로 받는다. 그리고 source에 있는 property 들에 대해서 순회하면서 그대로 destination에 추가하는 것이다. 소스코드 예제를 보자.

    function extend(destination, source) {
	    for (var k in source) {
		    if (source.hasOwnProperty(k)) {
			    destination[k] = source[k];
			}
		}
		return destination;
	}
그냥 우리가 동적으로 객체에 프로퍼티 값을 추가하듯, 다른 객체의 key와 value를 가져와서 고대로 추가하는 것이다.

나는 뭔가 이방식이 더 간편하고 좋다. prototype 상속은 너무 복잡한 감이 있고, 클래스 상속은 ES6에서만 지원한다.

ES6 문법이 모든 브라우저에서 표준이 되는 날이 온다면 이런 것 굳이 안쓰고 바로 class 상속으로 넘어가면 좋겠다.

그런데 왜 어떤 사람들은 ES6의 클래스 문법을 싫어하는지 사용하지 않는지는 궁금하다 한번 물어보고 여기에 내용을 추가 해 봐야 겠다.


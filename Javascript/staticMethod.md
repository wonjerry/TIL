## Static method

나는 그동안 어떤 프로젝트를 진행 할 때, 공통적으로 많이 사용하지만 어느 클래스에 속해있긴 애매한 것들을 util 모듈로 빼서 그것을 호출해서 사용했었다.

이 방법외에 다른 방법이 있다.

만약 우리가 dom을 조작하기 위해 View 라는 객체를 만들고 전반적인 dom에대한 컨트롤을 한다고 하자. 우리는 일단 View의 프로토타입에 메소드들을 추가해서 사용하고, View를 상속받아서 다른 것들을 사용할 것 이다.

그러나 view의 성격을 가지지만 다른곳에서 util 처럼 많이 사용되는 함수는 어떻게 사용할 것 인가??
-> View라는 함수 자체에 프로퍼티를 추가해서 함수를 저장해두고 다른 곳에서는 객체 생성을 안하고도 View의 성격을 가진 유틸 함수를 사용할 수 있다.

예를들어 Array를 보자. Array라는 기본 객체는 우리가 쓰는 형태와는 조금 다르지만 객체 생성을 안하고 forEach를 사용할 수 있다.

    Array.prototype.forEach.call(array, cb);

즉 이와 비슷하게 View 성격을 가진 유틸 함수를 만들고, 추가해 두면 언제 어디서나 객체 생성을 안하고 편하게 사용 가능하다.

    View.forEach = function(elements, cb) {
	    Array.prototype.forEach.call(elements, cb);
	};
이런식으로 말이다.
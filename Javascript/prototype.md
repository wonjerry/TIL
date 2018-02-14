###그러면 이 프로토타입 체인을 어떻게 만들고 관리할까??
일단 자바스크립트의 프로토타입이라는 개념을 조금 더 파고들어보자.

프로토타입은 앞서 얘기한것에서 추론해보면 기존의 상속 기능과 비슷하게,  공통적인 메소드나 변수를 "재사용"할 수 있도록 도와주는 것이다.

#### Prototype Object
모든 Object의 조상은 함수이다. 

	function Person() {}
	vat personObj = new Person(); // => 함수로 객체 생성

personObj는 person이라는 함수로부터 생성된 객체이다. new로 생성한 personObj를 console.log을 통해 찍어보면 function이라는 키워드와 같이 나올 것이다.

function이 정의될 때는 2가지 일이 동시에 이루어진다.
1. 해당 function에 생성자 자격 부여
	- 만약 함수 리터럴 방식으로 생성한 객체에 대해 new 키워드로 다른 인스턴스를 생성하려고 한다면 오류가 발생한다. constructor가 없기 때문이다!
2. 해당 function의 prototype object 생성 및 연결
	- 함수를 생성하면 함수만 생성되는 것이 아니라, prototype object도 생성된다.
	- function의 prototype이라는 속성은 person prototype object를 가리키고, person prototype object의 constructor는 person이라는 function을 가리키게 된다.
	- 그리고 생성된 함수는 prototype이라는 속성을 통해 prototype object에 접근이 가능하며 코드의 재사용이 가능해진다.

	function Person() {}
	Person.prototype.eyes = 2;
	Person.prototype.nose = 1;
	var kim  = new Person();
	var park = new Person():
	console.log(kim.eyes); // => 2

이런식의 코드를 작성하면 Person이라는 function 내부에는 아무런 값도 없지만, Person.prototype이라는 속성을 들여다 보면, eyes, nose라는 속성이 Person prototype object에 들어가있는 것을 볼 수 있다.

### Prototype Link
함수를 console로 찍어보면 __proto__라는 속성이 있다. 이것을 prototype link라고 불린다.

위의 코드를 보면 kim에는 eyes라는 속성을 추가한 적이 없는데 2라는 값이 찍히게 된다. 이게 어떻게 가능할까??? -> Person의 __proto__라는 속성이 이것을 가능하게 해준다.

prototype 속성은 function만 가지고있는 것과 달리,  __proto__ 라는 속성은 모든 object가 다 가지고 있다.

__proto__는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다. kim 객체는 Person 함수로부터 생성되었으니 Person 함수의 Prototype Object를 가리킨다.

	var kim = new Person(){};

이러한 코드가 존재한다고 해보자. new 키워드를 통해 Person 인스턴스가 생기는데 이때 Person의 prototype 속성이 가리키는 person prototype object에 대한 참조를 __proto__ 속성이 가져가게 되는 것이다.

따라서 kim.eyes 라고 하면, 
1. 맨 처음 kim에 그런 속성이 있는지 체크하고, 
2. 없다면 prototype chain 개념으로 __proto__가 리키는 prototype object로 거슬러 올라가고, 거기에 그런 속성이 있는지 체크하게 되는 것이다.
3. 만약 또 없으면 또 prototype object의 __proto__ 속성을 통해 타고 올라가게 되는 것이다.
4. 만약 있으면 그것을 출력하게 되고, Object의 prototype Object까지 도달했는데 없다면 undefined를 리턴하게 된다.

이런 프로토타입 체인의 구조 때문에 모든 객체는 Object의 자식이라 불리는 것이다.

[출처](http://insanehong.kr/post/javascript-prototype/)
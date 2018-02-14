## By reference and By value
1. primitive value
	- object가 아닌 타입들 (number)을 primitive value라고 한다.
	- 만약 a라는 value가 있고, 이 value를 어떤 함수의 매개인자 b로 넣었다고 하자
	- a라는 변수는 사실 메모리상의 어떤 주소값에 존재하는 값이고, a는 그 주소값을 참조하고 있다.
	- 만약 b라는 매개인자로 받는다면, b는 메모리상의 또 다른 메모리 주소값을 가지고, 그 주소값에 a의 실제 값이 copy되는 것이다.
	- 이것은 pass by value라고 한다.
2. object value
	- 만약 어떤 객체의 주소값이 a라는 변수에 저장되어있다고 하자.
	- 만약 어떤 함수의 매개 인자로 b라는 값이 있다고 할 때 b는 객체의 주소값을 넘겨받게 된다.
	- 따라서 a가 가지고 있는 객체와 b가 가지고 있는 객체가 같은 객체가 되는 것이다.
	- 이것을 pass by reference 라고 한다.
	- 한쪽을 바꾸면 다른쪽도 바뀐다.
## react 함수 .bind(this)의 차이
1. constructor 에서 this.dummyFunction = this.dummyfunction.bind(this); 하면 그 객체에 붙는거라서 인스턴스를 생성할때마다 해당 함수가 생성된다.
2. 그냥 함수 react 객체 내부에 생성하면 프로토타입 객체에 붙기때문에 계속 재사용된다.

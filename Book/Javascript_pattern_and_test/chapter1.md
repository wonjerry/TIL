## 자바스크립트 패턴과 테스팅
### 1장 좋은 소프트웨어 만들기

### 자바스크립트의 특성을 완벽히 섭렵하라

- 자바스크립트의 객체 지향적 함수 오버로딩은 arguments의 상태에 따라서 다른 함수를 내어주는 것이다.

D3.js에서 SVG 라인을 그리는 함수를 살펴보자.

    // 다른 전역 변수와 충돌을 피하기 위해 명칭공간을 생성한다.
	var rj3 = {};

	// svg라는 하위 명칭공간을 만든다.
	rj3.svg = {};

	// rj3.svg 명칭공간에 line 함수를 넣는다.
	rj3.svg.line = function() {
	  var getX = function(point) {
	    return point[0];
	  },
	  getY = function(point) {
	    return point[1];
	  },
	  interpolate = function(points) {
	    return points.join("L");
	  };

	  function line(data) {
	    var segments = [],
	      points = [],
	      i = -1,
	      n = data.length,
	      d;

	    function segment() {
	      segments.push("M",interpolate(points));
	    }

	    while (++i < n) {
	      d = data[i];
	      points.push([+getX.call(this,d,i), +getY.call(this,d,i)]);
	    }

	    if (points.length) {
	      segment();
	    }

	    return segments.length ? segments.join("") : null;
	  }

	  line.x = function(funcToGetX) {
	    if (!arguments.length) return getX;
	    getX = funcToGetX;
	    return line;
	  };

	  line.y = function(funcToGetY) {
	    if (!arguments.length) return getY;
	    getY = funcToGetY;
	    return line;
	  };

	  return line;
	};

자 이 코드에서 우리는 SVG 라인을 그리는 함수를 만들었고, 라인을 그리는 여러가지 경우를 보자.
1. array를 받아서 라인을 그리는 경우

	    (function() {
		  var arrayData = [
		        [10,130],
		        [100,60],
		        [190,160],
		        [280,10]
		      ],
		      lineGenerator = rj3.svg.line(),
		      path = lineGenerator(arrayData);

		  document.getElementById('pathFromArrays').setAttribute('d',path);
	}());

코드를 살펴보면 array로 좌표를 받고, lineGenterator함수를 받아서 arrayData를 넣어서 path를 생성한 후 라인을 그려주게 된다.

lineGenerator 함수에 인자값을 넣어서 함수를 실행시켜 주면, D3.js 코드에서 data가 arrayData가 되고, while문을 돌면서 x,y 좌표들을 배열로 묶어서 points 배열에 넣고, 그것들을 어떤 규칙을 가진 문자열로 만들어서 리턴한다. 그리고 그 문자열이 path가 된다.

그런데 만약에 입력값이 배열이 아니라 객체라면 어떻게 할 것 인가??

2. Object를 받아서 라인을 그리는 경우

객체가 들어온다면 그 객체를 돌면서 객체의 프로퍼티를 하나하나 빼면서 그것을 배열로 만들고 그것을 넣어줄 것인가??? 별로 유연하지 않은 것 같다. object array에 따라 유연하게 동작하는 함수를 만들고 싶다. 다음을 보자.

			(function() {
			  var objectData = [
			        { x: 10, y: 130 },
			        { x: 100, y: 60 },
			        { x: 190, y: 160 },
			        { x: 280, y: 10 }
			      ],
			      lineGenerator = rj3.svg.line()
			        .x(function(d) { return d.x; })
			        .y(function(d) { return d.y; }),
			      path = lineGenerator(objectData);

			  document.getElementById('pathFromObjects').setAttribute('d',path);
		}());

line 함수의 x, y 에 대해서 다른 함수를 지정 해 주었다. 왜 이렇게 했을 까?

D3.js를 살펴보면 x,y는 line 함수 자체를 리턴한다 디자인 패턴 중 chaining을 구현 한 것 같다. 그리고 arguments에 따라서 argument가 없다면 getX 함수를 리턴하고 있다면 새로 들어온 X좌표를 계산하는 함수를 getX에 배정한다.

이렇게 함으로써 기본 데이터가 array 데이터 라도 사용자는 객체 데이터에 대해 x와 y좌표를 넣어줄 수 있고 기존과 같은 기능을 하도록 할 수 있는 것이다.

자 그렇다면 점 데이터에 z좌표를 추가하려면 어떻게 해야할까?

	var objData = {
		{x : 10, y : 130, z : 99}
		{x : 10, y : 130, z : 99}
	}

기존에 자바나 c++같은 언어에서는 z를 또 선언 해주고 그에 맞는 처리를 해주는 일을 해야겠지만 자바스크립트에서는 덕 타이핑이라는 개념을 사용하면 다른 입력값이지만 같은 일을 한다.

#### 덕타이핑?
> 객체의 변수 및 메소드의 집합이 객체의 타입을 결정하는 것을 말한다. 클래스 상속이나 인터페이스 구현으로 타입을 구분하는 대신, 덕 타이핑은 객체가 어떤 타입에 걸맞은 변수와 메소드를 지니면 객체를 해당 타입에 속하는 것으로 간주한다.
> '오리처럼 생계서 오리처럼 걷고 오리처럼 꽥꽥 소리를 낸다면 그건 오리다.'

즉 쉽게 말하면 자바스크립트처럼 매개인자에 형이 없는 경우, 우리는 매개인자의 함수가 실행되어 런타임 에러가 나기 전까지 이 함수가 진짜 있는지 없는지 확인할 수 없다. 따라서 어떤 프로퍼티 또는 메소드가 있다면 그것이 같은 일을 하는 객체라고 간주하고 동작하게 하는 것 이다.

예를 들면

	var objData = {
		new XYPair(10, 130),
		new XYPair(100, 60),
		new XYPair(190, 160)
	}

이런 코드가 존재한다고 하자 그래도 D3.js가 똑같이 동작 하도록 할 수 있다. 매개인자.hasOwnProperty('x') 라는 조건을 만족하면 그에 맞는 동작을 하게하면 되는것이다.

### 대규모 시스템에서 자바스크립트의 함정을 주의하라
- 전역 네임스페이스를 더럽히지 말아라
- 자바스크립트는 덕 타이핑 덕분에 적은 코드로 많은 일을 할 수 있지만, 내가 짠 프로그램에 누가 뭘 넣을지 모른다. 따라서 함수 인자에 특정한 조건이 있다면 그 값을 꼭 검증해야 한다.

### 소프트웨어 공학 원칙을 적용하라
>실수 없이 완벽하게 악기를 연주하려면 느리게 연주하는 것이 빠른 길이며 그것이 숙련되면 자연히 빨라진다.

1. SOLID 원칙
- Single Responsibility Principle( 단일 책임 원칙 )
		- 모든 클래스는 반드시 한가지 일을 한다.
		- D3.js의 line 함수를 보자. 이 함수가 일을 하는 방식은 데이터를 받아서 라인을 그리는 것 뿐이며 데이터가 들어와서 그 데이터를 추출하는 방법 까지도 외부에서 전달받는다. 단지 line 함수는 멍청하게 라인을 그리는 일만 할 뿐이다.
- Open/Closed Principle ( 개방 폐쇄 원칙 )
		- 모든 소프트웨어는 확장 가능성은 열어두되 수정 가능성은 닫아야 한다.
		- D3.js에서 보면 좌표 데이터와 좌표를 연결하는 방식에는 변경이 있을 것이라고 보고 그것들을 추상화하여 다른 함수로 추출하였다. 이렇게 하면 좌표를 그리는 행위는 변하지 않고 데이터를 변환하는 방식, 데이터를 연결하는 방식만 변화가 있다.
		- 그런데 나는 정말 필요하다면 무한 확장을 하는 것이 아닌 수정하는 편도 더 낫다고 본다.
- Liskov Substitutuion Principle (리스코프 치환 원칙 )
		- 어떤 타입에서 파생된 타입의 객체가 있다면 이 타입을 사용하는 코드는 변경하지 말아야 한다.
		- 즉 기반 클래스와 서브 클래스가 하는 일이 다르다면 이 원칙을 어긴 셈이다.
- Interface Segregation Principle ( 인터페이스 분리 원칙 )
		- 인터페이스란 어떤 기능을 구현하지 않고 서술만 한 것 이다.
		- 기능이 많은 인터페이스는 더 작게 응축시킨 조각으로 나누어야 한다.
		- 자바스크립트에서 이 원칙을 사용하자면, 어떤 함수에 필요한 매개인자가 무엇인지 명확히 하고 최대한 줄이는 것이다.
		- 특정 타입의 인자를 바라기 보다는 어떤 인자가 이 타입에 실제로 필요한 프로퍼티가 있을 것임을 기대하는 것이다.
- Dependency Inversion Principle ( 의존성 역전 원칙 )
		- 상위 수준 모듈을 하위 수준 모듈에 의존해서는 안돼며 이 둘은 추상화에 의존해야 한다.
		- 인터페이스 기반 언어에서는 **의존성 주입** 이라는 개념으로 표현한다.
		- A 클래스가 B 클래스의 서비스가 필요하다면 A는 B를 생성하는 것이 아니라 A의 생성자에 들어온 파라미터가 B를 서술하는 인터페이스 역할을 한다.
		- 약간 위임과도 비슷한 느낌인 것 같다.

	rj3.svg.line = function() {
		return d3_svg_line(d3_identity)
	}

	function d3_svg_line(projection){
		// 선언문 생략
		function line(data){
				// 선언문 생략
				function segment() {
			      segments.push("M",interpolate(projection(points), tension);
			    }
		}
	}

이렇게 line을 그리는 기본 함수는 두고, point를 어떻게 처리할 것인지에 대한 함수를 projection 함으로써 A(d3_svg_line)는 B(projection) 을 따로 생성하지 않고 단지 그것을 사용할 뿐이다.

만약 선이 아닌 원을 그린다고 해 보자.

	rj3.svg.line.radial = function() {
		return d3_svg_line(d3_svg_lineRadial)
	}

	function d3_svg_line(projection){
		// 선언문 생략
		function line(data){
				// 선언문 생략
				function segment() {
			      segments.push("M",interpolate(projection(points), tension);
			    }
		}
	}

	function d3_svg_lineRadial(points){
		//점 데이터를 극 좌표계로 변환
		return points
	}

이렇게 치환 함수를 넣어줌으로써 상위 함수는 하위 함수의 메소드를 단지 projection받아서 사용할 뿐이다.


### DRY 원칙
- Don't repeat Yourself 즉 모든 지식 조각은 한번만 나와야 한다.

### 단위 테스트는 미래에 대한 투자다.
- 대부분 준비(arrange) 실행(act) 단언(assert)의 패턴을 따른다.
- 테스트 주도 개발 (TDD)를 실천하라
	- 테스트를 충족할 코드를 만들기 전에 테스트를 먼저 작성한다.
	- 코드를 만들고 테스트를 만들면 코드가 어떻게 작동해야 한다는 것이 아니라 코드를 만든대로 작동할 것이라는 이상한 사실을 확인할 뿐이다.
- 테스트하기 쉬운 코드로 다듬어라
	- 어떤 함수가 있는데 3가지 일을 한다고 가정해 보자.
	- 이 함수를 테스팅 하기 위해서는 한가지의 일을 하게 하고 그것에 대한 테스팅만 하는 편이 쉽다.
	- 따라서 그 3가지일을 나눠서 함수로 만들고 조합하는 편이 더 좋다.

### TDD의 장점
- 장기적인 신뢰가 가는 테스트 꾸러미가 나온다.
- 어플리케이션 객체에 대한 정확한 인터페이스를 설계할 때 도움이 된다.
- 단위테스트를 통해 코드를 더 빨리 개발할 수 있다.
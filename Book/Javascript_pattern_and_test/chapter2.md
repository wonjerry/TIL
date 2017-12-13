## 2장 도구다루기

### 테스팅 프레임워크
- 이 책에서 소개하는 프레임워크는 jasmine 이다.
- 만약 우리가 어떤 코드를 만들고 나서 테스트를 작성했다고 하자. 그리고 내가 테스트 할 때는 문제가 없어서 업로드를 했다고 가정 해 보자
- 그런데 다른 팀원이 테스트 했더니 테스트가 전부 오류이다???
- 명세서와 내가 만든 코드의 매개인자명이 달라서 테스트를 통과하지 못했다.
- 만약 명세를 먼저 보고 테스트를 작성하고 최소한의 코드를 먼저 작성했다고 한다면 이런 일을 없었을 것이다.
- 이것이 TDD의 장점 중 하나이다. 로직을 돌리면서 오류가 발생하면 고치는 것이 아닌 미리 확인하고 고칠 수 있다.

### 잘못된 코드 발견하기
- TDD는 코드 결함을 최대한 빨리, 코드 생성 직후에 감지하며 작은 기능 하나라도 테스트를 먼저 작성한 뒤 최소한의 코드만으로 기능을 구현한다.
- 내가 생각한 중요한 점은 코드를 꼼꼼히 잘 만들고 테스트를 만들어서 돌려보는 것이 아닌 명세에 따라서 테스트 만들고 **최소한의 코드만으로 기능을 구현 한 다음 구현을 리펙토링을 통해 최적화 한다.**

### 테스트성을 감안하여 설계하기
- 테스트를 차후에 생각하는 것이 아닌 우선적인 주요 관심사로 생각한다.
- 나의 경험에 비춰서 생각 해 보면 한 메소드가 여러일을 하면 테스팅 하기도 어렵고 일단 SOLID 원칙에도 맞지 않는다. 테스트하기 좋게 짠다고 생각하다보면 자연스레 SOLID 원칙과 맞물려서 유지보수와 확장성이 좋은 구조가 나올 확률이 높다.
- 만약 어떤 메소드가 예약기능을 하는데 그 내부에서 ajax 통신까지 해야되는 상황이 발생했다고 하자. 그러면 일단 테스트 하기는 힘들고 그냥 ajax 끼워넣어서 돌아가게 하면 좋다고 생각하니까 그냥 넘어가려고 한다. 그런데 이러면 안된다.
	- 일단 ajax에 대한 테스트를 먼저 하고
	- 예약기능을 하는 메소드에 넣기 전에 이 메소드가 이런일까지 해야하나? 라는 생각을 먼저 해 본다.
	- ajax 관련 객체를 만들어서 처리하게 하고 각각의 기능에 충실하게 한다. -> S 원칙에 맞음

### 꼭 필요한 코드만 작성하기
- 우리는 테스팅을 작은 기능 단위별로 다 작성 할 것이다.
- 이때 우리는 테스팅을 위한 최소한의 로직만을 작성하고 테스트 통과 후 리펙토링을 한다.
- 리펙토링을 통해 중복 코드를 들어낸다.
- **이 과정을 거치며 진짜 필요한 최소한의 코드만 작성하게 된다.**

### 안전한 유지보수와 리팩토링
- TDD를 실천하면 프로젝트 제품 코드를 대상으로 확실한 단위 테스트 꾸러미를 구축할 수 있다.

### 실행 가능한 명세
- 탄탄하게 구축된 단위 테스트 꾸러미는 테스트 대상 코드의 실행 가능한 명세 역할도 한다.
- 재스민은 행위 기반으로 테스트를 구성하며 재스민에서 스팩이라고 부르는 개별 테스트는 검증할 작동 로직을 일상 문장으로 작성하며 시작한다.
- 그리고 각각이 명세가 된다.

### 재스민 들어가기
- 재스민은 행위 주도 개발 방식으로 단위 테스트를 작성하기 위한 라이브러리이다.
-  BDD : 단위 테스트로 확인할 기능을 일상의 언어로 표현하는데 이로 인해 개발자는 자신의 로직이 어떻게가 아니라 무엇을 해야하는지 표현할 수 있다. 그리고 일상의 언어로 작성되기 때문에 기능 명세서로 삼을 만하다.

#### 테스트 꾸러미와 스펙
- 테스트 꾸러미는 describe로 정의되며 무엇을 해야하는지 서술 되고, 작은 테스트 들이 들어있다.
- 각 작은 테스트는 it으로 정의되며 이것도 무엇을 해야하는지 서술되고, 하나의 기대식을 가진 함수로 구성된다.

	describe('createReservation(passenger, flight)', function() {
	  var testPassenger = null,
	    testFlight = null,
	    testReservation = null;

	  beforeEach(function() {
	    testPassenger = {
	      firstName: '윤지',
	      lastName: '김'
	    };

	    testFlight = {
	      number: '3443',
	      carrier: '대한항공',
	      destination: '울산'
	    };

	    testReservation = createReservation(testPassenger, testFlight);
	  });

	  it('passenger를 passengerInformation 프로퍼티에 할당한다', function() {
	    expect(testReservation.passengerInformation).toBe(testPassenger);
	  });

	  it('flight를 flightInformation 프로퍼티에 할당한다', function() {
	    expect(testReservation.flightInformation).toBe(testFlight);
	  });
	});

- beforeEach 함수를 사용하면 각 테스트꾸러미가 시작되기 전에 실행되며 테스트 꾸러미가 완료되면 afterEach가 실행된다.

#### 기대식과 매처
	expect(testReservation.passengerInformation).toBe(testPassenger);
- toBe 에 들어가는 부분이 내가 기대하는 값이고 expect에 들어가는 부분이 내가 테스트해볼 값이다.
- 영어처럼 해석하면 이해하고 기억하기 더 쉽다.

#### Spy
- 어떤 함수/객체의 원래 구현부를 테스트 도중 다른 코드로 대체한 것을 말한다.
- 웹서비스와 같은 외부 자원과의 의존 관계를 없애고 단위 테스트의 복잡도를 낮출 목적으로 사용된다.
- 만약 예약 메소드가 실행된 후에 데이터를 서버로 보내는 등의 통신을 해야한다고 생각 해 보자. 우리는 예약 메소드만 테스트 해 보고 싶은데 통신 부분까지 다 구현해서 처리해야 할까? 아니다. 그 부분은 spy를 통해 간단한 코드로 대체하고 우리가 원하는 코드만 테스팅 해 볼 수 있다.

	function createReservation(passenger, flight, saver) {
	   var reservation = {
	    passengerInformation: passenger,
	    flightInformation: flight
	  };

	  saver.saveReservation(reservation);
	  return reservation;
	}

	// 샬롯이 작성한 ReservationSaver
	function ReservationSaver() {
	  this.saveReservation = function(reservation) {
	    // 예약 정보를 저장하는 웹서비스와 연동하는 복잡한 코드가 있을 것이다.
	  };
	}

이런 코드가 존재한다고 생각 해보자. 우리는 ReservationSaver에 대한 기능은 테스트하고 싶지도 않고 테스트를 다 하려면 테스트 복잡도가 너무 커진다.

	describe('createReservation(passenger, flight, saver)', function() {
	  var testPassenger = null,
	    testFlight = null,
	    testReservation = null,
	    testSaver = null;

	  beforeEach(function() {
	    testPassenger = {
	      firstName: '윤지',
	      lastName: '김'
	    };

	    testFlight = {
	      number: '3443',
	      carrier: '대한항공',
	      destination: '울산'
	    };

	    testSaver = new ReservationSaver();
	    spyOn(testSaver, 'saveReservation');

	    testReservation = createReservation(testPassenger, testFlight, testSaver);
	  });

	  it('passenger를 passengerInformantion 프로퍼티에 할당한다', function() {
	    expect(testReservation.passengerInformation).toBe(testPassenger);
	  });

	  it('주어진 flight를 flightInformation 프로퍼티에 할당한다', function() {
	    expect(testReservation.flightInformation).toBe(testFlight);
	  });

	  it('예약 정보를 저장한다', function() {
	    expect(testSaver.saveReservation).toHaveBeenCalled();
	  });
	});

- 위와같이 spyOn(인스턴스, 감시할 함수) 이렇게 해 주면 그 함수를 실행하지 않고 간단하게 넘긴다.
- 밑에서는 expect(testSaver.saveReservation).toHaveBeenCalled(); 이 코드를 통해서 진짜 call 되었는지 체크한다.

### 의존성 주입 프레임워크
- 이전에 우리는 의존성 역전이 SOLID 개발 요소 중 하나이고 의존성 주입은 이를 실현하기 위한 메커니즘이라고 말했었다.

#### 의존성 주입이란??
- 만약 우리가 어떤 컨퍼런스를 위한 웹 사이트를 개발해야 하고, 많은 기능들 중 참가자가 세션을 등록하면 성공 여부를 알려주는 기능을 만들어야 한다.

	Attendee = function(attendeeId) {

	  // 'new'로 생성하도록 강제한다.
	  if (!(this instanceof Attendee)) {
	    return new Attendee(attendeeId);
	  }

	  this.attendeeId = attendeeId;

	  this.service = new ConferenceWebSvc();
	  this.messenger = new Messenger();
	};

	// 주어진 세션에 좌석을 예약 시도한다.
	// 성공/실패 여부를 메시지로 알려준다.
	Attendee.prototype.reserve = function(sessionId) {
	  if (this.service.reserve(this.attendeeId, sessionId)) {
	    this.messenger.success('좌석 예약이 완료되었습니다!' +
	      ' 고객님은 ' + this.service.getRemainingReservations() +
	      ' 좌석을 추가 예약하실 수 있습니다.');
	  }  else {
	    this.messenger.failure('죄송합니다, 해당 좌석은 예약하실 수 없습니다.');
	  }
	};

- 위 코드를 보면 꽤나 잘 되어있는 것 같다. service는 Http 통신을 하고 있고 messenger는 성공 여부를 띄우는 역할을 한다. 그리고 우리는 Attendee를 테스트할 때 이것을 까지 테스트할 필요는 없다.
- 이 코드는 service, messanger에 각 기능들을 의존하고 있다고 생각 할 수 있다. 그리고 이 코드들을 미쳐 준비하기도 전에 Attendee.reserve 함수를 테스팅 할 수도 없다.
- 이 코드 내부에서 new 를 통해 의존성을 만들지 말고 의존성 주입을 통해서 해결해보자.
- 실제 환경에서는 진짜로 의존성을 주입하겠지만 ( new를 하지 않고 각 인스턴스를 매개인자로 받겠지만 ) 재스민 에서는 스파이같은 대체제를 주입해 주면 된다.

	// 실제 환경
	var attendee = new Attendee(new ConferenceWebSvc(), new Messenger() , id)
	// 테스트 환경
	var attendee = new Attendee(fakeService, fakeMessenger, id)

### 의존성을 주입하여 믿음직한 코드 만들기
#### 의존성 주입의 모든것
> 어떤 객체를 코딩하든 어떤 객체를 생성하든지 스스로 다음 질문들을 해 보자. 하나라도 '예' 라면 직접 인스턴스화 하지 말고 주입하는 방향으로 생각 해 보자.

- 객체 또는 의존성중 하나라도 DB, 설정 파일, HTTP 등등 외부 자원에 의존하는가?
- 객체 내부에서 발생할지 모를 에러를 테스트에서 고려해야 하는가?
- 특정한 방향으로 객체를 작동시켜야 할 테스트가 있는가?
- 서드파티 제공 객체가 아니라 온전히 내가 소유한 객체인가?

이 이후에 다른 방식의 테스팅 라이브러리가 있지만 일단 재스민으로 계속 사용해볼 예정이다.

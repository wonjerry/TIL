## Strategy Pattern

### 정의
자바스크립트가 아닌 모든 언어에 대해 정의하자면 프로그래밍에서, the strategy pattern 이란 런타임시에 어떤 동작을 하는 알고리즘을 선택할 수 있는 것이다.

### 어떤 문제를 Strategy Pattern으로 해결할 수 있을까?
- 어떤 알고리즘이 런타임시에 바뀌어야 할 때 사용되는 듯 하다.
- 여기서 알고리즘이란 어떤 유한한 값이 들어오고, 그것을 처리하고, 처리 결과값을 아웃풋으로 내는 것을 말한다.
- 어떤 알고리즘을 미리 클래스 내에 선언 해 두고 그것을 사용하는 것은 어떤 관점에서는 유연하지 않다.
- 자바스크립트에서는 런타임시 어떤 함수가 필요하다면 이미 정의해둔 함수를 갈아 끼움으로써 이 패턴을 구현할 수 있을 것 같다.

### 구현 예제
- Java

```java
	import java.util.ArrayList;
	import java.util.List;

	public class StrategyPatternWiki {

	    public static void main(final String[] arguments) {
	        Customer firstCustomer = new Customer(new NormalStrategy());

	        // Normal billing
	        firstCustomer.add(1.0, 1);

	        // Start Happy Hour
	        firstCustomer.setStrategy(new HappyHourStrategy());
	        firstCustomer.add(1.0, 2);

	        // New Customer
	        Customer secondCustomer = new Customer(new HappyHourStrategy());
	        secondCustomer.add(0.8, 1);
	        // The Customer pays
	        firstCustomer.printBill();

	        // End Happy Hour
	        secondCustomer.setStrategy(new NormalStrategy());
	        secondCustomer.add(1.3, 2);
	        secondCustomer.add(2.5, 1);
	        secondCustomer.printBill();
	    }
	}


	class Customer {

	    private List<Double> drinks;
	    private BillingStrategy strategy;

	    public Customer(final BillingStrategy strategy) {
	        this.drinks = new ArrayList<Double>();
	        this.strategy = strategy;
	    }

	    public void add(final double price, final int quantity) {
	        drinks.add(strategy.getActPrice(price*quantity));
	    }

	    // Payment of bill
	    public void printBill() {
	        double sum = 0;
	        for (Double i : drinks) {
	            sum += i;
	        }
	        System.out.println("Total due: " + sum);
	        drinks.clear();
	    }

	    // Set Strategy
	    public void setStrategy(final BillingStrategy strategy) {
	        this.strategy = strategy;
	    }

	}

	interface BillingStrategy {
	    double getActPrice(final double rawPrice);
	}

	// Normal billing strategy (unchanged price)
	class NormalStrategy implements BillingStrategy {

	    @Override
	    public double getActPrice(final double rawPrice) {
	        return rawPrice;
	    }

	}

	// Strategy for Happy hour (50% discount)
	class HappyHourStrategy implements BillingStrategy {

	    @Override
	    public double getActPrice(final double rawPrice) {
	        return rawPrice*0.5;
	    }

	}
```

- 메인 코드를 먼저 살펴보자.
	- Customer를 선언 한 후 계속해서 add 메소드를 호출한다.
	- 중간 중간 NormalStrategy , HappyHourStrategy 객체를 생성해서 strategy에 넣어주고 있다.
	- 그로인해 add의 계산법이 달라진다.
- BillingStrategy 인터페이스를 살펴보자
	- getActPrice 메소드를 정의하고 있다. 이것을 구현하는 클래스는 모두 이 메소드를 구현해야 하며, interface 형으로 어떤 객체를 받는다면 인터페이스에 속한 메소드만 사용 가능하다.
- NormalStrategy, HappyHourStrategy
	- normal은 그냥 값을 리턴하고 happyHour은 50% 싼 가격을 리턴한다.
	- 만약 HappyHour 시간이 되면 런타임시 유동적으로 계산법을 조정하는 것이다.
	- if 문으로 어떤 시간이 되면 true 값을 넘기거나 어떤 type을 넘길수도 있겠지만 이렇게 함으로써 어떤 함수의 내부는 변하지 않고 interface 만 새로 구현해서 넣어줌으로써 유연한 프로그래밍이 가능하다.

### 이것을 자바스크립트로 구현한다고 생각하면 어떨까?

자바스크립트는 원래 언어자체가 런타임시에 함수를 추가할 수 있다.

만약에 인터페이스 개념으로 어떤 객체에 여러 대체 함수를 만들어 두고, 그것이 필요할 때 type에 따라서 그것을 가져와서 내가 사용하고자 하는 함수 이름을 key로 value를 찾아서 교체해 주면 되겠다.

	function Customer(strategy) {
		drinks : [];
		strategy : strategy;
		add : function(price, quantity) {
		    drinks.push(strategy(price*quantity));
		}
		printBill: function() {
		    var sum = 0;
		    for (var i : drinks) {
		        sum += i;
		    }
		    console.log("Total due: " + sum);
		        drinks = [];
		}
		setStrategy: function(strategy) {
		    this.strategy = strategy;
		}
	}
	function BillingStrategy(){
		NormalStrategy: function(price){
			return price;
		},
		HappyHourStrategy: function(price){
			return price*0.5;
		}
	}

	function main(){
		var customer = new Customer(BillingStrategy.NormalStrategy);
		customer.add(100,2);
		customer.setStrategy(BillingStrategy.HappyHourStrategy);
		customer.add(200,2);
	}

이런식으로 프로그래밍 한다면 java 버전과 비슷한 느낌으로 돌아갈 것이다.

사실 지금의 예제는 너무 간다하기도 하고 단지 0.5곱하는 것만 바뀌는 것에 대해서 이렇게 디자인 패턴까지 사용하는 것이 비효율적으로 보이기까지 하다.
그리고 아직까지 어떻게 이것을 실제 프로젝트에서 적용할지, 어떤 복잡한 상황을 유연하고 편리하게 만들어 줄지는 모르겠지만 자바스크립트처럼 함수를 넘기거나 추가하는 것이 자유롭지 않은 언어에서는 유용하게 쓰일 것도 같다.
실제 프로젝트에서 사용해보고 느끼게 된다면 이 문서에 추가할 것이다

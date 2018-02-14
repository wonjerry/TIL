## Java 접근 지정자

접근 지정자가 있는 언어를 오랫동안 안쓰다보니 기본적인 내용인데도 혼란이 와서 정리해보려고 한다.

접근 지정자에는 크게 4가지가 있다.
- public
- private
- protected
- default

### Public
단어 그대로 이 키워드가 붙은 클래스, 메소드, 프로퍼티는 어디서나 접근이 가능하다.

### Private
이것도 단어 그대로 이 키워드가 붙은 메소드 프로퍼티는 외부에서 접근이 불가능하다. 다만 같은 클래스 멤버라면 접근이 가능하다.

클래스 앞에는 Private가 붙을 수 없다 왜냐하면 클래스를 통해서 인스턴스를 선언해야 하는데, 만약 private가 붙어버리면 다른곳에서 호출할 수 없기 때문이다.

### Protected
단어의 의미를 생각하면 보호된 정도로 해석할 수 있겠다. 같은 패키지, 또는 클래스 멤버 또는 상속 관계일 때만 외부에서 접근이 가능하고, 그 이외에는 접근이 불가능하다.

예를들어

```java
package package1;
import package2.test3;

public class test1 extends test3 {
	public test1(String a) {
		this.a = a;
	}
	test1 a = new Test1("wonjae");
}
---
package package2;
public class test3 {
	protected String a;
}

```

이러한 관계에서, default라면 원래 test1 이라는 class에서는 a에 접근할 수 없으나, protect 이고, 상속관계가 있기 때문에 지금 접근이 가능한 것이다.

만약 다른 package가 아닌 같은 package라면, 그 패키지 내부에서는 public 과 같은 역할을 한다고 볼 수 있다.

### default
이건 거의 protect에서 상속만 뺐다고 생각하면 된다.

같은 패키지 내부에서는 다 접근이 가능하지만, 다른 패키지에서는 접근 할 수 없도록 한다.
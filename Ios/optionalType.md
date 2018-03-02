### Optional type
swift는 var, let 이외에 optional이라는 하나의 type을 가지고 있다.

이 type은 쉽게말하자면 "음 이럴수도 있고 아닐수도있어"이다.

우리가 화면에 두개의 버튼을 배치해 두었다고 하고, 한개의 버튼에는 "1"을, 다른 버튼에는 아무것도 써두지 않았다고 가정하자.

만약 sender를 받아서 currentTitle을 받을 때 우리는 항상 그 값이 string임을 보장할 수 있을까?? 아니다. 아무것도 할당되어있지 않을 경우에는 String이 아닌 그냥 nil인 상태이다.

int도 마찬가지이다. int형 변수에 항상 숫자가 들어오면 좋겠지만 아닌 상황도 올 수 있다. 이렇게 불확실한 경우 optional Int로 선언 해둔다.

만약 자신의 로직에서 string 또는 int로 들어오는 것이 확실하다면 optional  변수 뒤에 느낌표를 붙이면 확실히 String이라고 얘기하는것과 같은 것이다.

```swift
let digit = sender.currentTitle!
// 이렇게 하면 digit은 optional String이 아닌 확실한 String 형으로 swift가 판단한다.
```

그런데 이것은 가끔 앱이 crash되는 원인이 될 수 있어 안정성을 좋아하는 사람들은 느낌표를 절대 쓰지 않겠다고 할 수도 있지만, 느낌표를 통해 crash를 발생 시킴으로써 꼭 set 되어야할 변수를 체크하고 디버깅 할 수 있게 된다.
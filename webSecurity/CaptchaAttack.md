### CAPTCHA 공격
이미지에 적힌 글씨를 쓰도록 해서 사람인 것을 확인하는 방법이 captcha이다.

#### 과정
1. CAPTCHA 확인
2. 확인 완료, 비밀번호 변경
3. 확인 절차를 제대로 해 두지 않았다면 CAPTCHA를 확인한 것 처럼 꾸미고 2로 진행 할 수도 있음

#### 대응 방법
1. 요청을 한단계로 처리한 것은 좋지만, 요청 내부의 특정한 값을 체크하도록 하는것은 해커가 얼마든지 그 값을 바꿔서 우회할 수 있기 때문에 좋은 방법은 아니다.
2. 현재 비밀번호를 한번 더 입력하도록 하고, captcha를 두 단계가 아닌 한단계로 처리하도록 한다.

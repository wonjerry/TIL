### Scalability
EC2 인스턴스를 이용해서 여러개의 머신을 만들고, 그것을 병렬로 연결 할 것이다. 그것을 위해서 ELB라는 것을 배울 것이다.

#### 가상화 및 종량제
- 가상머신이란??
	- 물리적인 컴퓨터가 아닌 소프트웨어가 만든, 컴퓨터랑 똑같이 돌아가는 상상의 컴퓨터이다.
- 우리는 가상머신을 통해 하나의 컴퓨터에서 여러개의 운영체제를 구동시킬 수 있다.
- aws와 같은 클라우드 컴퓨팅 서비스를 제공하는 회사는 개인 보다는 기업에 초점을 맞추고 있다.
- aws는 어마어마한 물리적인 컴퓨터를 장만 해둔다. 그리고 전세계의 사용자들에게(기업 또는 개인) 서비스를 제공한다.
- 클라우드 컴퓨팅을 사용하는 것, 그 반대의 경우의 차이점은??
	- 만약 물리적인 서버를 사용한다면, 사용량이 적을 때는 컴퓨터 성능이 남아돌고, 사용량이 많을 때는 컴퓨터 성능이 쫓아가지 못하고있기 때문에 사용자가 안좋은 서비스를 제공받게 된다.
	- 하지만 클라우드 컴퓨팅 기술을 사용한다면 내가 필요하지 않을 때는 서버의 규모를 줄이고, 필요할 때는 규모를 자유롭게 늘릴 수 있으므로 효율적인 처리가 가능해진다.

즉 scalability는 변화하는 수요에 따라 얼마나 탄력적으로 공급을 조정할 수 있는가에 대한 애기이다.

#### Scale UP
컴퓨터 수요가 늘어나면 더 좋은 컴퓨터로 업그레이드해서 대체하는 것이다.

이것을 실습하기 위해서 일단 두개의 인스턴스로 한쪽은 접속 요청을 많이하고, 다른 한쪽은 수비를 하는 그런 것을 해볼 것이다.

한쪽은 wordpress 이미지로 인스턴스를 생성하고 한쪽은 ubuntu로 인스턴스를 생성한다.

wordpress에서는 top을 통해서 cpu 점유율을 관찰한다.
ubuntu에서는 apache2-utils 을 설치해서 ab라는 프로그램을 통해 wordpress에 부하를 준다.

ab에서 -n은 몇명이 접속 할 것인가, -c는 동시접속을 몇명이나 할 것인가에 대한 옵션이다.
Ex) ab -n 400 -c 1 http://127.0.0.1/ 이런식으로 하면 해당 주소에 대해 400명이 순차적으로 한명씩 접속한다는 의미이다.

이런 프로그램을 통해 동접자 100 200 300에 대한 성능을 비교하면 현재 내가 만든 서비스가 얼만큼의 스트레스를 버틸 수 있는지 알 수 있겠다.
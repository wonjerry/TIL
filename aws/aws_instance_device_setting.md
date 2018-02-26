### 인스턴스 장치 설정
1. network
	- 자신이 network에 정통하다면 더 보안적으로 뛰어나고 성능이 좋은 설정을 걸면된다.
2. shutdown dehavior
	- stop을 하면 인스턴스는 사라지지 않고 서버의 동작만 멈춘다.
	- terminate하면 인스턴스 자체가 삭제되며, 데이터도 다 날아가 버릴 수 있다.
3. Termination protection
	- 실수로 인스턴스를 삭제할 경우 그런것을 방지하는 기능이다.
4. Monitoring
	- 원래 내가 돌리고 있는 인스턴스의 cpu 상태 램 상태 등등을 기본적으로 볼 수있다.
	- 이 기능은 좀 더 디테일한 상태를 볼 수 있을때 사용한다.
5. add Storage
	- Volume Type은 ssd 또는 hdd를 정할 수 있고 sdd를 골랐을 때 general purpose는 보통의 속도,  Provisioned IOPS를 선택하면 IOPS 수치를 조정할 수 있는데 이건 SSD의 속도이고, 이 수치를 높이면 더 많은 요금이 과금된다.
	- Delete on Termination은 만약 내가 인스턴스를 삭제했을 때 이게 체크가 되어있으면 저장장치도 다 폐기 되는 것이고, 체크가 안되어있으면 인스턴스만 삭제되고 이건 그대로 있게 된다.
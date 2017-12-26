## css position

- static
	- postion을 정해주지 않으면 기본적으로 태그가 가지는 속성이다.
- relative
	- top left right left 속성을 통해 위치를 지정 해 줄 수 있다.
- absolute
	- static 속성을 가지지 않는 태그를 기준으로 이동한다.
	- 부모에 static 속성을 가지지 않는 태그가 없다면 body가 기준이 된다.
- fixed
	- 스크롤을 내려도 항상 그자리에 있도록 하는 것이다.
	- 만약 top, left를 0으로 맞춰두고 스크롤을 내렸다면 fixed된 element가 계속 그자리를 지키고 있다.

scroll이 되었을 때 모달을 그 자리에 띄우고 싶다면 fixed로 해결하면 별 난리 안피워도 편안히 구현된다.

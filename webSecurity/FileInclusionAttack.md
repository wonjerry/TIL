### File Inclusion Attack
지정한 파일을 PHP include()로 소스코드에 삽입한다.

- LFI(로컬 파일 인클루젼)
- RFI(리모트 파일 인클루젼)

php에서 주로 이루어지는 공격으로, 만약 다른 page를 접근 할 때 해당 file 이 있는지 체크하는 로직으로 대응할 수 있다.

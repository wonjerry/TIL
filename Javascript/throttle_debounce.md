## Lodash throttle, debounce

### throttle

```javascript
_.throttle(fn,limit);
return wrapped function
```

throttle은 인자값으로 들어온 함수를 한번 실행시킨 후 limit 된 시간이 지나기 전까지 아무리 fn을 실행시켜도 limit시간이 지나지 않으면 실행되지 않도록 하는 함수를 fn에 wrapping 해서 리턴한다.

limit가 지나면 다시 한번 실행 가능하며, limit가 지나기 전까지 fn이 실행되지 않는다.
### debounce

```javascript
_.debounce(fn,limit);
return wrapped function
```

debounce 제한시간이 지나기 전까지 fn을 아무리 실행시켜도 실행되지 않는다.

제한 시간이 지나야만 fn이 실행된다. 즉 실행시 fn을 실행하도록 예약 걸어놓았다가 제한 시간이 지나면 예약 걸어놓은 fn이 실행된다.

만약 제한시간이 지났어도 예약 걸어놓은 fn이 실행되지 않았다면 아무리 실행해도 실행되지 않는다.


이 두 기능은 보편적으로 사용자가 어떤 버튼을 아무리 빠르게 많이 눌러도 limit 시간에 대해서 한번만 실행하도록 보장하기 위해서 사용한다.

예를들어 대표적으로 네트워크 요청을 하는 버튼의 경우 버튼을 누를때마다 네트워크 요청을 다 해버리면 트래픽이 너무 커져버리기 때문에 이런 제한을 둔다.

## react에서 window 또는 document 컨트롤하기

nss-client 과제를 할 때 modal 창을 만들어야 했다.

modal창이 필요한 부분은

- modal이 뜨면 스크롤 막기
- scroll 하고 modal를 불렀을 때 그 위치에서 modal이 뜨기

그런데 이것들을 하려면 window 객체와 document가 필요했다. 그리고 보통 react내부에서는 이것들을 사용하는것은 권장되지 않고 최대한 피한다

이것들에 접근하려면 리액트 컴포넌트의 생명주기에서 사용해주면 별 문제가 없는 듯 하다.

	componentDidMount() {
	    this.setState({
	      modalOffsetY: window.pageYOffset + window.innerHeight/2,
	      overlayOffsetY: window.pageYOffset
	    });
	    document.body.classList.add('lock_scroll');
	  }

	  componentWillUnmount() {
	    document.body.classList.remove('lock_scroll');
	  }

이런식으로 컴포넌트가 mount 되었을때나 unmount 되었을 때 document나 window를 건드려서 가져올 수 있다.

사실 생명주기 없어도 그냥 window에 접근 가능하다 근데 최대한 사용 안하는 쪽이 훨씬 좋다

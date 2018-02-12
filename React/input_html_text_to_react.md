## React html text rendering

[출처](https://reactjs.org/docs/dom-elements.html)

react는 악의적인 코드를 막기 위해, html 텍스트가 들어오면 그것을 랜더링 하지 않고 그냥 text형태로 출력한다.

그러나 개발자 입장에서 html text를 html 로써 랜더링 해야하는데 그렇지 못하면 html code가 그대로 드러나서 취약점이 발견되고, 보안에 구멍이 생길 수 있다.

따라서 react는 이러한 기능을 지원하기 위해서 “Dangerously Set innerHTML”이라는 기능을 제공한다.

	render() {
	  let codes = "<b>Will This Work?</b>";
	    return (
	      <div dangerouslySetInnerHTML={ {__html: codes} }>
	      </div>
	    );
	  }

이렇게 html text를 div의 dangerouslySetInnerHTML프로퍼티에 대해 넣어주면 react가 그것을 인식하고 랜더링 해 주게 된다.

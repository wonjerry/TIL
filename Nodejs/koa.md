### Koa란?
koa.js는 작고 유연한 node.js 웹 어플리케이션 프레임워크이다. express 개발자가 개발하고 유지보수하는 오픈소스 프레임워크이다.

### 왜 koa를 사용할까?
- koa는 작고 추상화가 아주 얇게 되어있다.
- koa는 커다란 커뮤니티를 형성하고 있어서 플러그인도 많고, 검색하기도 용이하다.
- koa는 express에서 쓰던 플러그인들을 가져다가 쓸 수 있다.

### Hello world!
- npm i koa --save
- create app.js
- copy code below

	var koa = require('koa');
	var app = koa();
	
	app.use(function* (){
	   this.body = 'Hello world!';
	});
	
	app.listen(3000, function(){
	   console.log('Server running on https://localhost:3000')
	});

### Why koa use generator function?
koa는 generator function middleware들의 배열로 구성 되어 있으며, 각 request에 대해 stack과 같이 실행되도록 내부가 구현되어 있다.

예시를 한번 살펴보자.

```javascript
	var koa = require('koa');
	var app = koa();
	 
	app.use(function* (next) {
	   //do something before yielding to next generator function 
	   
	   //in line which will be 1st event in downstream
	   console.log("1");
	   yield next;
	 
	   //do something when the execution returns upstream, 
	   //this will be last event in upstream
	   console.log("2");
	});
	 
	app.use(function* (next) {
	   // This shall be 2nd event downstream
	   console.log("3");
	   yield next;
	 
	   // This would be 2nd event upstream
	   console.log("4");
	});
	 
	app.use(function* () { 
	   // Here it would be last function downstream
	   console.log("5");
	   
	   // Set response body
	   this.body = "Hello Generators";
	
	   // First event of upstream (from the last to first)
	   console.log("6"); 
	 
	});
	
	app.listen(3000);
```

generator 함수 내부에 yield next라는 것이 있는 것은 console.log('1');을 실행하고 나서 다음번 요청 처리 함수를 처리하라는 것이다.

그러면 다음번 함수의 console.log('3') 이 출력되며 또 다음번 함수로 넘어가게 된다.

마지막 함수에서는 yield가 없다. 그래서 5를 출력하고,  body에 string을 할당하고, 6을 출력한다. 즉 모든 일을 다 마치고 다시 처리 안된 것을 이 처리된다.

마지막으로 두번째 함수가 아까 처리되다가 말아서 그쪽으로 다시 돌아가서 4를 출력하고, 첫번째 함수로 넘아가서 2를 출력하고 모든 일이 끝난다.

### Routing
koa는 routing 모듈을 제공하지 않는다. 따라서 npm에서 koa-router를 다운받아서 사용해야 한다.

```javascript
	const Koa = require('koa');
	const Router = require('koa-router');
	 
	const app = new Koa();
	const router = new Router();
	 
	router.get('/hello',  (ctx, next) => {
	  ctx.body = 'Helloworld';
	});
	 
	app
	  .use(router.routes())
	  .use(router.allowedMethods());
	app.listen(3000);
```

router 객체를 생성하고 router에서 제공하는 메소드를 이용해서 curd 처리를 할 수 있다.


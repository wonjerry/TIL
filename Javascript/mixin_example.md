### mixin 예제

    function mixEvents(obj) {
	  /*밖으로 빼줄 이유 없이 이 스코프에 선언해 준다.*/
	  var events = {};

	  obj.trigger = function() {
	    if (events.hasOwnProperty(arguments[0])) {
	      var self = this;

	      var eventName = arguments[0];
	      //var arg = Array.from(arguments);
	      //arg.splice(0,1);
	      var arg = Array.prototype.slice(arguments, 1);

	      events[eventName].forEach(function(cb) {
	        cb.apply(self, arg);
	      });
	    }
	  };

	  obj.on = function(eventName, cb) {
	    if (!events.hasOwnProperty(eventName)) {
	      /* 외부 객체를 오염시키는 방법*/
	      //obj[eventName] = [];
	      /*권장되는 방법*/
	      events[eventName] = [];
	    }

	    events[eventName].push(cb);
	  };

	  return obj;
	}

	var obj = mixEvents({namne: 'wan', age: 30});

	obj.on('ageChange',
	    function ageChange() {
	      console.log('나이변함 : ' + this.age);
	      console.log(arguments);
	    });

	obj.on('ageChange',
	    function ageChange() {
	      console.log('나이변함2 : ' + this.age);
	      console.log(arguments);
	    });

	obj.age++;

	obj.trigger('ageChange', 'wow1', 'wow2', 'wow3');


- mixin의 기본 마인드는 외부에서 들어온 객체는 더럽히지 않는다 이다.
- 그래서 외부에서 들어온 객체에 어떤 key, value를 기존에 있던 데이터가 덮어씌여질 수 있다.
- 따라서 clouser를 사용해서 함수 내부에 event를 따로 저장해 두는 객체를 만들어 두고 그것을 사용한다.
- Array 의 protype을 사용해서 코드를 줄일 수 있겠다.
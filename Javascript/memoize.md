## memoize

underscore에서 제공하는 기능으로, 어떤 함수가 어떤 매개변수(arguments)로 어떤 결과값을 도출해냈다면, 다음번에 같은 매개변수로 연산을 시도했을때 또 함수를 실행시키는 것이 아닌 저장된 결과값을 리턴한다.

    _.memoize = function (func) {
    var result = {}
    return function () {
      var key = JSON.stringify(arguments)
      if (!result[key]) {
        result[key] = func.apply(this, arguments)
      }
      return result[key]
    }

  이렇게 내가 만약 어떤 함수에 대해 memoize 기능을 씌우고 싶다면 memoize의 인자값으로 함수를 넘긴다.

그렇게 하면 memoize 함수 내부에서는 매개변수로 key값을 만들어서 key에 해당하는 결과값이 없다면 함수를 실행시켜 결과값을 받고, 아니면 저장된 결과값을 리턴하는 함수로 원래 함수를 감싸서 '함수를' 리턴하게 된다.
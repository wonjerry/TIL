## Dataset

Dom 객체의 element에는 dataset이 존재한다. 일단 기본적으로 dataset은 사용자가 자신만의 dom element property를 만들 수 있게 해주는 것이다.

만약 우리가 dom element중 하나에 wonjae라는 프로퍼티를 주고 싶다고 해 보자.

    <div class='test' wonjae='great'></div>

이런식으로 적용하고 싶지만 일단 저렇게 막무가내로 넣어서는 안된다. 그래서 사용하는 것이 dataset이다.

    element.dataset.wonjae = 'greate'

이렇게 자바스크립트를 통해 추가해 주면  dom element에는

    <div class='test' data-wonjae='great'></div>

이렇게 'data-'라는 헤더가 붙는다.

그런데 만약에 우리가 model에 있던 data에 id 값이 존재한다고 해 보자. 그렇다면 기존의 나는 당연히 dom element의 id 값에 그 id를 넣어줬을 것이다.

하지만 dom element의 id 값은 대부분 css 관련해서 조작 할 때만 사용되고 이런 데이터 관련 id는 dom의 id 프로퍼티를 사용하는 것이 아닌 dataset을 이용하여 data-id를 만들어서 사용하는 것이 더 좋다고 한다.

이것이 dataset을 사용하는 이유이다. dom 적인 요소와 data 적인 요소의 분리.

#### dataset으로 만든 커스텀 프로퍼티 찾기
1. Element를 바로 찾아서 element.dataset.커스텀프로퍼티 = 수정할 값 이렇게 해 준다.
2. querySelector로 찾는다면 [data-커스텀프토퍼티 = 수정할 값] 이렇게 해 준다.## Dataset

                                                       Dom 객체의 element에는 dataset이 존재한다. 일단 기본적으로 dataset은 사용자가 자신만의 dom element property를 만들 수 있게 해주는 것이다.

                                                       만약 우리가 dom element중 하나에 wonjae라는 프로퍼티를 주고 싶다고 해 보자.

                                                           <div class='test' wonjae='great'></div>

                                                       이런식으로 적용하고 싶지만 일단 저렇게 막무가내로 넣어서는 안된다. 그래서 사용하는 것이 dataset이다.

                                                           element.dataset.wonjae = 'greate'

                                                       이렇게 자바스크립트를 통해 추가해 주면  dom element에는

                                                           <div class='test' data-wonjae='great'></div>

                                                       이렇게 'data-'라는 헤더가 붙는다.

                                                       그런데 만약에 우리가 model에 있던 data에 id 값이 존재한다고 해 보자. 그렇다면 기존의 나는 당연히 dom element의 id 값에 그 id를 넣어줬을 것이다.

                                                       하지만 dom element의 id 값은 대부분 css 관련해서 조작 할 때만 사용되고 이런 데이터 관련 id는 dom의 id 프로퍼티를 사용하는 것이 아닌 dataset을 이용하여 data-id를 만들어서 사용하는 것이 더 좋다고 한다.

                                                       이것이 dataset을 사용하는 이유이다. dom 적인 요소와 data 적인 요소의 분리.

                                                       #### dataset으로 만든 커스텀 프로퍼티 찾기
                                                       1. Element를 바로 찾아서 element.dataset.커스텀프로퍼티 = 수정할 값 이렇게 해 준다.
                                                       2. querySelector로 찾는다면 [data-커스텀프토퍼티 = 수정할 값] 이렇게 해 준다.
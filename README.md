# Vue.js 공부하기!
Vue.js 공식문서   
: __https://kr.vuejs.org/v2/guide/__


### #01 Vue 인스턴스 생성하기

```
<div id="app">
  {{ name }}
</div>

<script>
  new Vue({
    el: '#app',
    data: {
     name: 'kenta'
    }
  }
</script>
```
Vue.js에서 데이터를 쓸 때에는 중괄호 2개 안에 쓴다.   

---

### #02 메소드(Methods) 사용하기
```
new Vue({
  el: '#app',
  data: {
    person: {
      name: 'kenta',
      age: 19
    }
  },
  methods: {
    nextYear(greeting) {
      return `${greeting}! ${thperson.name}는(은) ${(thperson.age + 1)}살 입니다.`;
    },
    otherMethod: function() {
      this.nextYear();
    }
  }
});
```
함수를 사용할 때에는 인스턴스에 methods에 함수를 추가하여 사용하고, 함수 안에 함수를 넣는 것도 가능하다.   
그리고 함수는 "함수이름: fucntion() { }" 이런식으로 작성해도 되지만, function을 생략하고 작성해도 무관하다.   
__Ex.__
```
nextYear: function(greeting) { }

nextYear(greeting) { }
// 둘이 동일하다.
```

매개변수를 사용할 때에는 {{ nextYear(매개변수) }} 이렇게 넣어주면 된다.   
__Ex.__
```
{{nextYear('안녕하세요')}}
```

---

### #03 데이터 바인딩 (Data Binding)
__v-bind:__   
HTML 태그 속성에도 데이터 값을 넣어줄 수 있다.   
또한, 함수를 넣는 것도 가능하다.   

__Ex.__
```
<div id="app">
  <input v-bind:type="type">
  <input :type="type">

  <!-- v-bind:를 생략하고 : 만 작성해도 상관없다. -->

  <a :href="getLink('kenta5293')"> 이동하기 </a>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    type: 'text',
    link: 'https://github.com/'
  },
  methods: {
    getLink(nickname) {
      return this.link + nickname
    }
  }
})
</script>
```

---

### #04 이벤트 (Events)
__v-on:__   
이벤트 앞에 v-on을 작성하는 것으로 이벤트를 생성할 수 있다.   
또, 데이터 바인딩과 같이 v-on을 @ 으로 대체할 수 있다.    

__Ex.__
```
<div id="app>
  <form v-on:submit.prevent="submit">
    <input type="text">
    <button>Submit</button>
  </form>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
    },
    methods: {
      submit() {
        alert('Hello')
        console.log('Hello')
      }
    }
  });
</script>
```

Submit 버튼을 누르면 methods에 작성된 submit() 함수가 실행되며,   
.prevent는 수식어로, Vue.js 에는 수식어가 존재한다.   
.prevent는 자바스크립트 기본 문법의 event.preventDefault()를 호출하는 것이다.   
외에도 다양한 수식어가 있으니 Vue.js 공식 사이트를 참고하면 좋을 듯 하다.   
__https://kr.vuejs.org/v2/api/index.html#v-on__

---

### #05 데이터 양방향 바인딩 (Data Two Way Binding)
__v-model__   
v-model="데이터"를 작성하는 것으로 데이터 양방향 바인딩이 가능하다.   
__데이터 → 뷰__ 의 형태로 데이터의 값을 뷰로 업데이트하여 보여주는 것을 __단방향 바인딩__ 이라고 한다.   
한 방향으로 흐르는 것과 다르게 __데이터 ⇄ 뷰__ 형태로 바인딩하여 양 방향으로 흐르게 해주는 것을 __양방향 바인딩__ 이라고 한다.   
즉, 데이터에 있는 값이 뷰에 나타나고, 이 뷰의 값이 바뀌면 데이터의 값도 바뀌는 것이다.

__Ex.__
```
<div id="app">
  <form v-on:submit.prevent="submit">
    <input type="text" v-model="text" ><br>
    {{ text }}<br>
  </form>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      text: 'text'
    },
  });
</script>
```

---

### #06 Computed 속성
__Computed__   
간단한 연산의 경우 {{ }} 안에 넣어도 괜찮으나, 많은 연산이 들어갈 경우 코드가 비대해지고, 유지보수가 어렵다고 한다.   
이를 해결하기 위해 사용해야하는 것이 바로 __computed 속성__ 이다.   

__Ex.__
```
<div id="app">
  {{ reversedMessage }}
  <!-- '안녕하세요'의 역순인 '요세하년안'이 출력된다. -->
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: '안녕하세요'
    },
    methods: {

    },
    computed: {
      reversedMessage() {
        return this.message.split('').reverse().join('');
      }
    }
  });
</script>
```

Computed 속성은 computed 라는 오브젝트 안에 작성하는 것으로, 템플릿 안에는 {{ reversedMessage }} 와 같이 작성한다.   
메소드와의 차이점으로는 메소드는 템플릿에 () 소괄호를 꼭 작성해야하지만 Computed에 정의된 경우에는 소괄호를 작성하지 않는다!   

다른 차이점으로 Computed는 캐싱을 하고, 메소드는 캐싱을 하지 않는다는 점이다!   
Computed는 처음 한번만 연산하여 __저장(캐싱)__ 을 하여 값을 가지고 있고, 종속 대상이 바뀔 때에만 함수를 다시 실행한다.   
즉, 몇 번을 요청해도 처음 계산했던 결과를 즉시 반환해주는 것이다.   
메소드의 경우에는 요청될 때마다 data에서 가져와 여러번 연산을 하고 return을 해주는 것이다.   

---

### #07 Watch 속성
__Watch__   
Watch 속성은 가끔 Computed 속성을 쓸 수 없는 상황일 때 사용하면 된다. Computed 속성으로 구현 가능한 것이면 Computed 속성을 사용하면 된다.   
Watch 속성은 감시하고 있는 데이터가 바뀌면 작동을 한다.

__Ex.__
```
<div id="app">
  {{ message }}<br>
  <button @click="changeMessage">Click</button><br>
  {{ updated }}
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: '안녕하세요',
      updated: '안녕못해요'
    },
    methods: {
      changeMessage() {
        this.message = "Hello"
      }
    },
    watch: {
      message(newVal, oldVal) {
        console.log(newVal, oldVal);
        this.updated = "NO Hello!";
      }
    }
  });
</script>
```

Watch속성을 사용할 때에는 감시하고 싶은 데이터를 함수 형식으로 작성하면 된다. 위의 예제에선 "message"를 감시하고 있으므로 watch 안에 "message"라는 함수를 만든 것이다.   
또 매개변수로 __newValue__ 와 __oldValue__ 를 받는데, 업데이트 된 값이 첫번째(newValue)에 들어오고, 기존의 값이 두번째(oldValue)에 들어오게 된다.

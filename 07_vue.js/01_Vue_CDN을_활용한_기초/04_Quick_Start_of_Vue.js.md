## Quick Start of Vue.js

* Django & Vue.js 코드 작성 순서
  
  * Django
    
    * "데이터의 흐름"
    
    * url -> views -> template
  
  * Vue.js
    
    * "Data가 변화하면 DOM이 변경"
    1. Data 로직 작성
    
    2. DOM 작성

* 공식문서 "시작하기" 따라하기
  
  * 선언적 렌더링
    
    ```html
    <h2> 선언적 렌더링</h2>
    <div id="app">
        {{message}}
    </div>
    ```
    
    ```javascript
    var app = new Vue({
        el:'#app',
        data: {
            message:' hi~'
        }
    })
    ```
  
  * Element 속성 바인딩
    
    ```html
    <h2>Element 속성 바인딩 </h2>
    <div id="app-2">
        <span v-bind:title="message">
            마우스를 오버하면 title을 볼 수 있음
        </span>
    </div>
    ```
    
    ```javascript
    var app2 = new Vue({
        el: '#app-2',
        data: {
            message:'이 페이지는' + new Date() + '에 로드 외었습니    '
        }
    })
    ```
  
  * 조건문
    
    ```html
    <h2>조건</h2>
    <div id="app-3">
        <p v-if="seen">ok~~!</p>
    </div>
    ```
    
    ```javascript
    var app3 = new Vue({
        el:'#app-3',
        data: {
            seen:true
        }
    })
    ```
  
  * 반복문
    
    ```html
    <h2>반복</h2>
    <div id="app-4">
        <ol>
            <li  v-for="foro in todos">
                {{todo.text}}
            </li>
        </ol>
    </div>
    ```
    
    ```javascript
    var app4 = new Vue({
        el:'#app-4',
        data: {
            todos: [
                {text:'study javascript'},
                {text:'study Vue'},
                {test:'good'}
            ]
        }
    })
    ```
  
  * 사용자 입력 핸들링
    
    ```html
    <h2>사용자 입력 핸들링</h2>
    <div id="app-5">
        <p>{{message}}</p>
        <button v-on:click="reverseMessage">reverse~</button>
    </div>
    ```
    
    ```javascript
    var app5 = new Vue({
        el:'#app-5',
        data: {
            message: 'hi~'
        },
        methods: {
            reverseMessage:function() {
                this.message = this.message.split('').reverse().join('')
            }
        }
    })
    ```
  
  * 사용자 입력 핸들링
    
    ```html
    <div id ="app-6">
        <p>{{message}}</p>
        <input v-model="message">
    </div>
    ```
    
    ```javadoclike
    var app6 = new Vue({
        else:'#app-6',
        data: {
            message:'hi!'
        }   
    })
    ```
    
    

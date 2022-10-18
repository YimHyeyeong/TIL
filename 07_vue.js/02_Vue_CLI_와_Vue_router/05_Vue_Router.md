## Vue Router

* Vue Router
  
  * "Vue.js 공식 라우터"
  
  * 라우트(route)에 컴포넌트를 매핑한 후, 어떤 주소에서 렌더링할 지 알려줌
  
  * SPA 상에서 라우팅을 쉽게 개발할 수 있는 기능을 제공

* Vue Router 시작하기
  
  * Vue Router plugin 설치 (Vue CLI 환경)
    
    ```
    $ vue add router
    ```
    
    * [주의] 기존 프로젝트를 진행하고 있던 도중에 추가하게 되면 App.vue를 덮어쓰므로 프로젝트 내에서 다음 명령을 실행하기 전에 필요한 경우 파일을 백업(커밋)해야 함
  
  * commit 여부 -> Yes / History mode 사용 여부 -> Yes

* Vue Router로 인한 변화
  
  1. App.vue 코드
  
  2. router/index.js 생성
  
  3. views 디렉토리 생성
  
  ```html
  // App.vue
  <template>
      <div id="app">
          <div id="nav">
              <router-link to="/">Home</router-link>
          </div>
      <div>
      <router-view/>
      </div>
  </template>
  ```

* Vue Router - "index.js"
  
  * 라우트에 관련된 정보 및 설정이 작성 되는 곳
  
  ```javascript
  // router/index.js
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  import Home from '../views/Home.vue'
  
  Vue.use(VueRouter)
  const routes = [
      {
      path:'/',
      name:'Home',
      component: Home
      },
      {
      path:'/about',
      name: 'About',
      componet: () => import('../views/About.vue')
      }
  ]
  const router = new VueRouter({
      mode:'history',
      base:process.env.BASE_URL,
      routes
  })
  export default router
  ```

* Vue Router - "router-link"
  
  * `<router-link>`
    
    * 사용자 네이게이션을 가능하게 하는 컴포넌트
    
    * 목표 경로는 'to' prop으로 지정됨
    
    * HTML5 히스토리 모드에서 router-link는 클릭 이벤트를 차단하여 브라우저가 페이지를 다시 로드 하지 않도록 함
    
    * a 태그지만 우리가 알고있는 GET 요청을 보내는 a태그와 조금 다르게 기본 GET 요청을 보내는 이벤트를 제거한 형태로 구성됨
    
    ```
    <router-link to='/'>Home</router-link>
    <router-link to='/about'>About</router-link>
    ```

* Vue Router - "router-view"
  
  * `<router-view>`
    
    * 주어진 라우트에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트
    
    * 실제 component가 DOM에 부착되어 보이는 자리를 의미
    
    * router-link를 클릭하면 해당 경로와 연결되어 있는 index.js에 정의한 컴포넌트가 위치
    
    ```
    <router-view/>
    ```

* History mode
  
  * HTML History API를 사용해서 router를 구현한 것
  
  * 브라우의 히스토리는 남기지만 실제 페이지는 이동하지 않는 기능을 지원
  
  * 즉 페이지를 다시 로드하지 않고 URL을 탐색할 수 있음
    
    * SPA의 단점 중 하나인 "URL이 변경되지 않는다"를 해결

* History API
  
  * DOM의 Window 객체는 history 객체를 통해 브라우저의 세션 기록에 접근할 수 있는 방법을 제공
  
  * history 객체는 사용자를 자신의 방문 기록 앞과 뒤로 보내거나 기록의 특정 지점으로 이동하는 등 유용한 메서드와 속성을 가짐
1. Named Routes
   
   * 이름을 가진 라우트
   
   * 명명된 경로로 이동하려면 객체를 `vue-router`컴포넌트 요소의 prop에 전달
   
   ```javascript
   // router/index.js
   import Vue from 'vue'
   import VueRouter from 'vue-router'
   import Home from '../views/Home.vue'
   import About from '../views/About.vue'
   
   Vue.use(VueRouter)
   const routes = [
       {
       path:'/',
       name:'Home',
       component: Home
       },
       {
       path:'/about',
       name: 'About',
       componet: About
       }
   ]
   const router = new VueRouter({
       mode:'history',
       base:process.env.BASE_URL,
       routes
   })
   export default router
   ```
   
   ```html
   // App.vue
   <template>
       <div id="app">
           <div id="nav">
           <router-link :to="{name:'Home'}">Home</router-link>
           <router-link :to="{name:'About'}">About</router-link>
           </div>
       </div>
   </template>
   ```

2. 프로그래밍 방식 네비게이션
   
   * `<router-link>`를 사용하여 선언적 탐색을 위한 a 태그를 만드는 것 외에도 router의 인스턴스 메서드를 사용하여 프로그래밍 방식으로 같은 작업을 수행할 수 있음
   
   | 선언적 방식                   | 프로그래밍 방식            |
   | ------------------------ | ------------------- |
   | `<router-link to="...">` | `$router.push(...)` |
   
   * Vue 인스턴스 내부에서 라우터 인스턴스에 `$router`로 접근할 수 있음
   
   * 따라서 다른 URL로 이동하려면 `this.$router.push`를 호출할 수 있음
     
     * 이 메서드는 새로운 항목을 히스토리 스택에 넣기 때문에 사용자가 브라우저의 뒤로 가기 버튼을 클릭하면 이전 URL로 이동하게 됨
   
   * `<router-link>`를 클릭할 때 내부적으로 호출되는 메서드이므로 `<router-link :to="...">`를 클릭하면 **router.push(...)** 를 호출하는 것과 같음
   
   * 작성할 수 있는 인자 예시
     
     ```
     // literal string path
     router.push('home')
     
     //object
     router.push({path:'home'})
     
     //named route
     router.push({name:'user,params:{userId:'123'}})
     
     // with query, resultring in /register?plan=private
     router.push({path:'register', query:{plan:'private'}})
     ```
   
   * About에서 Home으로 이동하는 로직 작성
     
     ```html
     // About.vue
     ...
     <script>
     export default {
         name:'About',
         methods: {
             moveToHome:function () {
                 this.$router.push({name:'Home'})
             }
         }
     }
     </script>
     ```

3. Dynamic Route Matching
   
   * 동적 인자 전달
   
   * 주어진 패턴을 가진 라우트를 동일한 컴포넌트에 매핑해야 하는 경우
   
   * 예를 들어 모든 User에 대해 동일한 레이아웃을 가지지만 다른 User ID로 렌더링 되어야하는 User 컴포넌트 예시
     
     ```javascript
     const routes = [
         {
         path:'/user/:userId',
         name:'user',
         component: User
         },
     ]
     ```
   
   * 동적 인자는 `:`(콜론)으로 시작
   
   * 컴포넌트에서 `this.$route.params`로 사용가능
   
   | pattern                            | matched path          | $route.params                  |
   | ---------------------------------- | --------------------- | ------------------------------ |
   | /user/:userName                    | /user/john            | {username:'john'}              |
   | /user/:userName/article/:articleId | /user/john/article/12 | {username:'john',articleId:12} |
   
   * TheLotto.vue 예시
     
     ```javascript
     // index.js
     const routes = [
         ...
         {
         path:'/lotto/:lottoNum',
         name:'TheLotto',
         component: TheLotto
         },
     ]
     ```
     
     ```html
     // TheLotto.vue
     <template>
         <div>
         ...
         <h2>{{$route.params.lottoNum}}</h2>
         </div>
     </template>
     <script>
     import _ from 'lodash'
     
     export default {
         name:'TheLotto',
         data: function () {
             return {
             selectedLuckyNums: []
             }
         },
         methods:{
             getLuckyNums: function () {
                 const numbers = .range(1,46)
                 this.selectedLuckyNums = _.samplSize(numbers,this.$route.params.lottoNum)
             }
         }
     }
     </script>
     ```
     
     ```html
     // App.vue
     <template>
         <div id="app">
             <div id="nav">
                 ...
             <router-link :to="{name:'TheLotto', params:{lottoNm:6}}">TheLotto</touter-link>
             </div>
         <router-view/>    
         </div>
     </template>  
     ```
* components와 views
  
  * 기본적으로 작성된 구조에서 components 폴더와 views 폴더 내부에 각기 다른 컴포넌트가 존재하게됨
  
  * 컴포넌트를 작성해 갈 때 정해진 구조가 있는 것은 아니며 주로 아래와 같이 구조화하여 활용함
  
  * **App.vue**
    
    * 최상위 컴포넌트
  
  * **views/**
    
    * router(index.js)에 매핑되는 컴포넌트를 모아두는 폴더
    
    * ex) App 컴포넌트 내부에 About & Home 컴포넌트 등록
  
  * **components/**
    
    * router에 매핑된 컴포넌트 내부에 작성하는 컴포넌트를 모아두는 폴더
    
    * ex) Home 컴포넌트 내부에 HelloWorld 컴포넌트 등록

* Vue Router가 필요한 이유
  
  1. SPA 등장 이전
     
     - 서버가 모든 라우팅을 통제
     
     - 요청 경로에 맞는 HTML를 제공
  
  2. SPA 등장 이후
     
     - 서버는 index.html 하나만 제공
     
     - 이후 모든 처리는 HTML 위에서 JS 코드를 활용해 진행
     
     - 즉, 요청에 대한 처리를 더 이상 서버가 하지 않음(할 필요가 없어짐)
  
  3. 라우팅 처리 차이
     
     - `SSR`
       
       - 라우팅에 대한 결정권을 서버가 가짐
     
     - `CSR`
       
       - 클라이언트는 더 이상 서버로 요청을 보내지 않고 응답받은 HTML 문서안에서 주소가 변경되면 특정 주소에 맞는 컴포넌트를 렌더링
       
       - 라우팅에 대한 결정권을 클라이언트가 가짐
     
     - 결국 Vue Router는 라우팅의 결정권을 가진 Vue.js에서 라우팅을 편리하게 할 수 있는 Tool을 제공해주는 라이브러리

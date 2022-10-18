## LocalStorage

* vuex-persistedstate
  
  * Vuex state를 자동으로 브라우저의 LocalStorage에 저장해주는 라이브러리 중 하나
  
  * 페이지가 새로고침 되어도 Vuex state를 유지시킴
  
  * 설치
    
    ```
    $ npm i vuex-persistedstate
    ```
  
  * 라이브러리 사용
    
    ```javascript
    // index.js
    import createPersistedState from 'vuex-persistedstate'
    export default new Vuex.Store({
        plugins: [
            createPersistedState(),
        ],
        ...
    })
    ```

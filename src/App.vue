<template>
  <div id="app">
    {{msg}}
    <Home v-if="!isLogin"></Home>
    <Editor v-if="isLogin" :user="userData"></Editor>
  </div>
</template>

<script>
import Home from './components/Home.vue';
import Editor from './components/Editor.vue';

export default {
  name: 'app',
  data () {
    return {
      isLogin:false,
      msg: 'Welcome to Vue.js App'//変数は引き継がれない
    };
  },
  create: function() {
    // Vie.jsがこのコンポーネントを作成したタイミングで実行
    firebase.auth().onAuthStateChanged( user => {
      console.log(user);
      if (user) {//user情報が格納されたらisLoginのtrue/falseを付与
        this.isLogin = true;
      }else{
        this.isLogin = false;
      };
    });
  },
  components: {
    Home:Home,
    Editor:Editor
  }
};
</script>
<template>
    <div id="home">
        <h1>{{ msg }}</h1>
        <!-- @clickはHTMLのonClickのようなもの　Vue.jsでは呼び出す関数を記載する -->
        <button @click="googleLogin">Googleアカウントでログインするよ</button>
    </div>
</template>

<script>
export default {
    name: "home",
    data() {
        return {
            msg: "Hello"
        };
    },
    //buttonから呼び出した関数。コンポーネント内で呼び出す関数はmothodsないに記載して利用する
    methods: {
    googleLogin: function() {
            firebase.auth().signInWithRedirect(new firebase.auth.GoogleAuthProvider());
        }
    }
}
</script>

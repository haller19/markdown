<template>
    <div class="editor">
        <h1>エディター</h1>
        <span>{{ user.displayName }}</span>
        <button @click="logout">ログアウト</button>
        <div class="wrap-editor">
            <!-- v-model：inputやtextareaの状態をコンポーネントのデータへ格納する機能。
             v-modelで指定した変数へ格納->「データバインディング」-->
            <textarea class="markdown" v-model="markdown"></textarea>
            <!-- v-html：HTMLとして描画する機能。ブラウザのXSSの原因となるため、対策の必要あり-->
            <div class="preview" v-html="preview()"></div>
        </div>
    </div>
</template>

<script>
import marked from "marked";// marked をnpm install
export default {
    name: "editor",
    props: ["user"],//props:親コンポーネントから受け継ぐデータの定義
    data() {
        return {
            markdown: "" //data関数にマークダウンで記述されたテキストを入れる
        };
    },
    methods: {
        logout: function() {
            firebase.auth().signOut();
        },
        preview: function() {
            return marked(this.markdown); //markdownに書いたマークダウンされたHTMLを返却
        }
    }
};
</script>

<style lang="scss" scoped>
// scoped：このコンポーネントで記述したCSSはこのコンポーネントないでしか適用されない
.wrap-editor{
    display: flex;
    .markdown{
        width:50%;
        height:500px;
        margin-right:.5rem;
    }
    .preview{
        width:50%;
        text-align: left;
        margin-left: .5rem;
    }
}

</style>


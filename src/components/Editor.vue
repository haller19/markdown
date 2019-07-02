<template>
    <div class="editer">
        <h1>エディター</h1>
        <span>{{ user.displayName }}</span>
        <button @click="logout">ログアウト</button>
        <div>
            <div class="memoListWrapper">
                <!-- v-for：リストレンダリング。配列またはオブジェクトを指定しておくとそのデータの要素が生成される。
                :key：リストレンダリングで繰り返す要素それぞれに個別のkeyを設定する。
                indexがselectedIndexと等しい時、data-selected="true" -->
                <div class="memoList" v-for="(memo, index) in memos" :key="index" @click="selectMemo(index)" :data-selected="index == selectedIndex">
                    <p class="memoTitle">{{ displayTitle(memo.markdown) }}</p>
                </div>
                <button class="addMemoBtn" @click="addMemo">追加</button>
                <button class="deleteMemoBtn" v-if="memos.length > 1" @click="deleteMemo">削除</button>
            </div>
            <!-- v-model：inputやtextareaの状態をコンポーネントのデータへ格納する機能。
             v-modelで指定した変数へ格納->「データバインディング」-->
            <textarea class="markdown" v-model="memos[selectedIndex].markdown"></textarea>
            <!-- v-html：HTMLとして描画する機能。ブラウザのXSSの原因となるため、対策の必要あり-->
            <div class="preview" v-html="preview()"></div>
        </div>
    </div>
</template>

<script>
/**
 * 配列の中身のデータはmemoに格納
 * 配列の番号はindexに格納
 * memo.keyで中のデータへアクセス可能
 */
import marked from 'marked';

export default {
    name: 'editer',
    props: ['user'],
    data() {
        return {
            memos: [{ //memosの配列、中にオブジェクトを格納。オブジェクト内のテキストテータのキーをmarkdownとする
            markdown: ''
            }],
            selectedIndex: 0 //編集・プレビューしているデータはselectedIndex変数。配列の番号は０から指定して表示
        }
    },
    methods: {
        logout: function() {
        firebase.auth().signOut();
        },
    addMemo: function() {
        this.memos.push({
        markdown: '無題',//markdownに''内のテキストを渡す
        })
    },
    deleteMemo: function(){
        this.memos.splice(this.selectedIndex, 1);//spliceは配列の任意の一からデータを取り出す関数。選択中のﾒﾓを削除するためそのﾒﾓ番号を調節するためにselectedIndexから1を引く。
        if (this.selectedIndex > 0) {
            this.selectedIndex--;
        }
    },
    selectMemo: function(index) {
        this.selectedIndex = index; //indexを入力してselectedIndexを切り替え、表示・プレビュー共に切り替わるように
    },
    preview: function() {
      return marked(this.memos[this.selectedIndex].markdown);
    },
    displayTitle: function(text) {
      return text.split(/\n/)[0];//テキストを開業で分割し配列に。配列初めの値を返却する。
    },
  }
}
</script>

<style lang="scss" scoped>
.memoListWrapper {
    width: 19%;
    float: left;
    border-top: 1px solid #000;
}
.memoList {
    padding: 10px;
    box-sizing: border-box;
    text-align: left;
    border-bottom: 1px solid #000;
    &:nth-child(even) {
        background-color: #ccc;
    }
    &[data-selected="true"] {
        background-color: #ddf;
    }
}
.memoTitle {
    height: 1.5em;
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
}
.addMemoBtn {
    margin-top: 20px;
}
.markdown {
    float: left;
    width: 40%;
    height: 500px;
}
.preview {
    float: left;
    width: 40%;
    padding:0rem;
    text-align: left;
}
</style>
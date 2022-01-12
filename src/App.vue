<template>
  <div>
    <div id="currentApp">
    <button @click="read()">データの取得</button>
  
    <div class="filter-area">
      <input v-model="titleFilter" type="text" placeholder="タスク名で絞り込む" />
      <label>
        <input type="radio" v-model="doneFilter"
         :value="DONE_FILTER.UNDONE"/>未完了
      </label>
      <label>
        <input type="radio" v-model="doneFilter"
         :value="DONE_FILTER.DONE"/>完了
      </label>
      <label>
        <input type="radio" v-model="doneFilter"
         :value="DONE_FILTER.ALL"/>すべて
      </label>
    </div>  
    <ul class="list-tasks">
      <!--<li class="list-task" v-for="(task,index) in tasks" :key="task.id">--> <!-- v-bindなくても表示された。公式ガイドでは、v-for利用時は可能な限りkey属性を設定することを推奨しているとのこと。 --><!-- :key == v-bind:key -->
      <li class="list-task" v-for="(task,index) in filteredTasks" :key="task.id">
        <input type="checkbox" :id="['id' + index]" v-model="task.done" />
        <label :for="['id' + index]">{{task.title}}</label>
        <button class="btnDelete" @click="deleteTask(task.id)">削除</button>
      </li>
    </ul>
  </div>
  <!-- v-show:登録用UI -->
  <form
    v-show="newTaskFieldsVisible"
    @reset.prevent="resetNewTaskFields"
    @submit.prevent="addTask"> 
    <!-- 新規登録するタスク名 -->
    <input type="text" v-model="newTaskTitle" class="form__text"/>
    <button type="submit">追加</button> <!-- submitボタンにする -->
    <button type="reset">キャンセル</button> <!-- リセットイベントを起動する -->
  </form>
  <!-- v-show:登録用UIを表示するボタン -->
  <button
    type="button"
    v-show="!newTaskFieldsVisible"
    @click="showNewTaskFields($event)">タスク追加</button>
</div>
</template>
<script>
  // /www.codegrid.net/articles/2020-vue3-2/
  //const Todo = { //←この記述だとtasksのインスタンス作れなかった
  const DONE_FILTER = {
    DONE: true,
    UNDONE: false,
    ALL: null,
    // ...今後、新たな実施状態の概念が発生したら追加定義する
  };
  import { readonly } from 'vue'
  export default {
    data() {
      return {
        tasks: [],
        // 現在の選択値を管理（初期値は未完了）
        doneFilter: DONE_FILTER.UNDONE,
        titleFilter: '',
        newTaskTitle: '',
        newTaskFieldsVisible: false
      };
    }, 
    // created(){ これだとデータが読み込めなかった
    //   this.read()
    // },
    mounted() { 
      this.read()
    },
    computed: {
      DONE_FILTER() {
        return readonly(DONE_FILTER);
      },
      filteredTasks() {
        // ★1 絞り込みが不要な場合は全データを返す
        const noFilter = this.titleFilter === '' &&
                          this.doneFilter === DONE_FILTER.ALL;
        if (noFilter) {
          return this.tasks;
        }

        return this.tasks.filter(task => {
          // ★2 タスク名フィルタの判定
          const titleMatched = this.titleFilterExp.test(task.title);

          // ★3 タスク実施状態フィルタの判定
          const doneMatched =
            this.doneFilter === DONE_FILTER.ALL ||
            this.doneFilter === task.done;

          return titleMatched && doneMatched;
        });
      },
      // ★4 タスク実施状態フィルタの正規表現オブジェクト化
      titleFilterExp() {
        return new RegExp(this.titleFilter);
      },
    },
    methods: {
      read(){
        console.log("called read")
        fetch('http://localhost:3001/tasks')
          .then(res => res.json())
          .then(data => this.tasks = data)
          .catch(err => console.log(err.messate))
      },
      deleteTask(taskId) {
        console.log("called delete")
        fetch(`http://localhost:3001/tasks/${taskId}`, {
          method: 'DELETE'
        })
        .then( () =>  { 
          this.read()
        }) 
      },
      // ★タスクの追加
      addTask() {
        console.log("called add")
        // タスク名が未入力の場合はなにもしない
        if (this.newTaskTitle === '') return; 

        fetch(`http://localhost:3001/tasks`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            'title': this.newTaskTitle,
            'done': false
          })
        })
        .then( () =>  { 
          this.read()
        }) 
        // 登録用UIの非表示化と、新規登録用のタスク名のクリア
        this.resetNewTaskFields();
      },
      showNewTaskFields(event) {
        console.log(event.target);
        console.log(event.type);
        this.newTaskFieldsVisible = true;
      },
      resetNewTaskFields() {
        this.newTaskFieldsVisible = false;
        this.newTaskTitle = '';
      }
    }
  }

</script>
<style>
.filter-area {
  margin: 20px auto;
}
.filter-area input[type="text"] {
  margin: 0 20px 0 0;
}
.list-tasks {
  width: 40%;
  margin:40px auto;
}
.list-task {
  text-align: left;
  list-style: none;
  display: flex;
  justify-content: flex-start;
  margin: 5px 0;
}
.list-task label {
flex:1 0 auto;
}
.form__text {
  padding: 5px;
  width: 300px;
  margin-right: 20px;
}
</style>
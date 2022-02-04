<template>
  <div>
    <div id="currentApp">
    <button @click="read()">データの取得</button>
  
    <div class="filter-area">
      <input v-model="titleFilter" type="text" placeholder="タスク名で絞り込む" ref="titleFilter" />
      <label><input type="radio" :value="DONE_FILTER.UNDONE" v-model="doneFilter" />未完了</label>
      <label><input type="radio" :value="DONE_FILTER.DONE" v-model="doneFilter" />完了</label>
      <label><input type="radio" :value="DONE_FILTER.ALL" v-model="doneFilter" />すべて</label>
    </div>  
    <ul class="list-tasks">
      <!--<li class="list-task" v-for="(task,index) in tasks" :key="task.id">--> <!-- v-bindなくても表示された。公式ガイドでは、v-for利用時は可能な限りkey属性を設定することを推奨しているとのこと。 --><!-- :key == v-bind:key -->
      <li class="list-task" v-for="task in tasks" :key="task.id">
          <form
            v-if="isEditingTask(task)"
            @submit.prevent="editTask"
            @reset.prevent="hideEditingTaskFields"
            class="form"
          >
            <input
              v-model="editingTaskTitle" type="text" @keyup.esc="hideEditingTaskFields" ref="editingTaskTitle" class="input_text" 
            />
            <button type="submit">確定</button>
            <button type="reset">キャンセル</button>
          </form>
          <template v-else>
            <input type="checkbox" v-model="task.done" @change="changeDone(task)" />
            <span>{{task.title}}</span>
            <button @click="showEditingTaskFields(task)" :ref="getEditBtnId(task.id)">編集</button>
            <button @click="deleteTask(task.id)">削除</button>
          </template>
        </li>
      <li class="list-buttons">
        <form
          v-show="newTaskFieldsVisible"
          @submit.prevent="addTask"
          @reset.prevent="resetNewTaskFields"
        >
          <input
            v-model="newTaskTitle"
            ref="newTaskTitle"
            @keyup.esc="resetNewTaskFields"
            type="text"
            placeholder="新しいタスク"
            class="input_text"
          />
          <button type="submit">追加</button>
          <button type="reset">キャンセル</button>
        </form>
        <button @click="showNewTaskFields" v-show="!newTaskFieldsVisible" ref="taskAddBtn">
          タスク追加
        </button>
      </li>
    </ul>
  </div>
  <!-- v-show:登録用UI -->
</div>
</template>
<script>
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
        DONE_FILTER: readonly(DONE_FILTER),
        doneFilter: DONE_FILTER.UNDONE,
        titleFilter: '',
        newTaskFieldsVisible: false,
        newTaskTitle: '',
        editingTaskFieldsVisible: false,
        editingTaskTitle: '',
        editingTask: '',
      };
    }, 
    created(){ 
      this.read();
    },
    mounted() { 
      //this.read()
    },
    watch: {
      titleFilter() {
        let url = 'http://localhost:3001/tasks?title='+this.titleFilter
        this.fetchTasks(url);
      },
      doneFilter() {
        let url = "";
        if( this.doneFilter === DONE_FILTER.ALL){
          url = 'http://localhost:3001/tasks';
        }else{
          url = 'http://localhost:3001/tasks?done='+this.doneFilter;
        }
        this.fetchTasks(url);
      },
    },
    methods: {
      read(){
        console.log("called read")
        fetch('http://localhost:3001/tasks?done=false')
          .then(res => res.json())
          .then(data => this.tasks = data)
          .catch(err => console.log(err.messate))
      },
      fetchTasks(url) {
        console.log("fetchTasks " + url);
        fetch(url)
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
      isEditingTask(task) {
        return this.editingTaskFieldsVisible && this.editingTask.id === task.id;
      },
      getEditBtnId(taskId) {
        return `taskEditBtn${taskId}`;
      },

      showNewTaskFields() {
        this.newTaskFieldsVisible = true;
        this.$nextTick(() => {
          this.$refs.newTaskTitle.focus();
        });
      },
      resetNewTaskFields(event) {
        this.newTaskTitle = '';
        this.newTaskFieldsVisible = false;
        this.$nextTick(() => {
          this.$refs.taskAddBtn.focus();
        });
      },
      showEditingTaskFields(task) {
        this.editingTask = task;
        this.editingTaskTitle = task.title;
        this.editingTaskFieldsVisible = true;

        this.$nextTick(() => {
          this.$refs.editingTaskTitle[0].select();
        });
      },
      hideEditingTaskFields() {
        this.editingTaskFieldsVisible = false;
        const editBtnId = this.getEditBtnId(this.editingTask.id);
        this.$nextTick(() => {
          this.$refs[editBtnId][0].focus();
        });
      },
      changeDone(task) {
        fetch(`http://localhost:3001/tasks/${task.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            'title': task.title,
            'done': task.done
          })
        })
        .then( () =>  { 
          this.read()
        }) 
      },
      editTask() {
        this.editingTask.title = this.editingTaskTitle;
        this.hideEditingTaskFields();

        fetch(`http://localhost:3001/tasks/${this.editingTask.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            'title': this.editingTask.title,
            'done': this.editingTask.done
          })
        })
        .then( () =>  { 
          this.read()
        }) 
      },
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
  list-style: none;
}
.list-task {
  text-align: left;
  list-style: none;
  display: flex;
  justify-content: flex-start;
  margin: 5px 0;
}
.list-task span {
  display: inline-block;
  width: 70%;
  margin: 0 10px 0
}
.list-task button {
  margin: 0 5px;
}
.list-task label {
  flex:1 0 auto;
}
.list-buttons {
  margin: 20px 0 0;
}
.list-buttons button {
  margin: 0 5px;
}

.form {
  width: 100%;
}
.input_text {
  width: 60%;
  padding: 5px;
  margin-right: 20px;
}

.form__text {
  padding: 5px;
  width: 300px;
  margin-right: 20px;
}

</style>
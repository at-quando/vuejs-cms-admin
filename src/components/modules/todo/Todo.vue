<template>
  <div class="todo-content">
    <div v-show="!isEditing">
      <h2><div>{{ todo.title }}</div>
        <div class="side-custom" v-for="(item,index) of todo.worker" :key="index">
          <img class="side-av" src="http://api.randomuser.me/portraits/men/22.jpg" alt="avatar"> 
          <span class="side-name">{{ item.name }}</span>
        </div>
      </h2>
      <p>{{ todo.content }} </p>
      <div class="task-action"> 
        <span v-on:click="showForm">
          <i class='ti-pencil'></i>
        </span> 
        <span v-on:click="deleteTodo(todo)">
          <i class='ti-trash'></i>
        </span> 
      </div>
    </div>
    <div class="content-todo-devin" v-show="isEditing">
      <div>
        <div>
          <label>Title</label>
          <p>
            <el-input placeholder="Please input" v-model="todo.title" type='text'></el-input>
          </p>
        </div>
        <div class='field'>
          <label>Project</label>
          <p>
            <el-input v-model="todo.project" type='textarea' :rows="4" placeholder="Please input"> </el-input>
          </p>
        </div>
        <div>
          <el-button v-on:click="hideForm" type="danger">Close</el-button>
        </div>
      </div>
    </div>
    <div class="todo-completed" v-show="todo.status == 3" disabled> 
      Completed
    </div>
    <div class="todo-pending" v-on:click="completeTodo(todo)" v-show="todo.status == 0">
      Pending 
    </div>
    <div class="todo-confirm" v-on:click="completeTodo(todo)" v-show="todo.status == 1">
      Confirm
    </div>
    <div class="todo-approval" v-on:click="completeTodo(todo)" v-show="todo.status == 2">
      Approval
    </div>
  </div>
</template>
<script>
  export default {
    props: ['todo'],
    data: function () {
      return {
        isEditing: false
      }
    },
    methods: {
      completeTodo: function (todo) {
        this.$emit('complete-todo', todo)
      },
      deleteTodo: function (todo) {
        this.$emit('delete-todo', todo)
      },
      showForm: function () {
        this.isEditing = true
      },
      hideForm: function () {
        this.isEditing = false
      }
    }
  }
</script>
<style lang="scss" scoped>
.side-custom {
  padding: 0; 
  min-height: 0; 
  display: inline-block;
  margin-right: 20px;
}
.side-av {
  border-radius: 50%;
  height: 30px;
}
.side-name {
  font-size: 12px;
}
.todo-confirm {
  background: #fb7777;
}
.todo-approval {
  background: #efb137; 
}
.todo-pending {
  background: #878d99;
}
.todo-completed {
  background: #67c23a;
}
</style>
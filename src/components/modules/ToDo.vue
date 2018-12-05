<template>
  <div v-if="todos && ems.length > 0">
    <section class="content-header" >
      <h1>ToDo Apps<small><i class="ti-heart"></i><i class="ti-export"></i><i class="ti-printer"></i></small></h1>
      <ol class="breadcrumb">
        <li><router-link to="/"> <i class="ti-home"></i></router-link></li>
        <li><a href="#">Apps</a></li>
        <li class="active">To Do</li>
      </ol>
    </section>
    <section class="content">
      <div class="todo-devin">
        <div class="row">
          <div class="col-md-3">
            <div class="side-todo">
              <ul>
                  <li class="clearfix" v-if="$store.state.role">
                    <el-dropdown class="choose-ems" trigger="click" @command="handleCommand">
                      <div class="over-ems">
                        <div v-if="currentEms != null">
                          <img src="http://api.randomuser.me/portraits/men/22.jpg" alt="avatar"> 
                          <div class="about" >
                            <div class="name">{{ems[currentEms].name}}<i class="el-icon-caret-bottom el-icon--right"></i></div> 
                            <div class="status"><i class="fa fa-circle online"></i> online </div>
                          </div>
                        </div>
                        <div v-else>Boss <i class="el-icon-caret-bottom el-icon--right"></i></div>
                      </div>
                      <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item :command="index">
                          <span class="side-name">Boss</span>
                        </el-dropdown-item>
                        <el-dropdown-item v-for="(item,index) of ems" :key="index" :command="index">
                          <div class="side-custom">
                            <img class="side-av" src="http://api.randomuser.me/portraits/men/22.jpg" alt="avatar"> 
                            <span class="side-name">{{item.name}}</span>
                          </div>
                        </el-dropdown-item>
                      </el-dropdown-menu>
                    </el-dropdown>
                  </li>
                  <li class="pending warning">Warning Deadline<b>{{todos.filter(todo => {return todo.status === 3}).length}}</b></li>
                  <li class="completed">Complete task <b>{{todos.filter(todo => {return todo.status === 3}).length}}</b></li>
                  <li class="pending approval">Wait for approval <b>{{todos.filter(todo => {return todo.status ===  2}).length}}</b></li>
                  <li class="pending confirm">Confirm task <b>{{todos.filter(todo => {return todo.status ===  1}).length}}</b></li>
                  <li class="pending">Pending task <b>{{todos.filter(todo => {return todo.status === 0}).length}}</b></li>
              </ul>
              <create-todo v-on:create-todo="createTodo"></create-todo>
            </div>
          </div>
          <div class="col-md-9">
            <todo-list v-bind:todos="todos"></todo-list>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import sweetalert from 'sweetalert'
import TodoList from './todo/TodoList.vue'
import CreateTodo from './todo/CreateTodo.vue'
export default {
  name: 'ToDo',
  components: {
    TodoList,
    CreateTodo
  },
  data: function () {
    return {
      currentEms: null,
      cities: [{
        value: 'Beijing',
        label: 'Beijing'
      }, {
        value: 'Shanghai',
        label: 'Shanghai'
      }, {
        value: 'Nanjing',
        label: 'Nanjing'
      }, {
        value: 'Chengdu',
        label: 'Chengdu'
      }, {
        value: 'Shenzhen',
        label: 'Shenzhen'
      }, {
        value: 'Guangzhou',
        label: 'Guangzhou'
      }],
      value6: '',
      ems: [],
      todos: [{
        title: 'Bug Fixed V 1.0.4',
        project: 'Are we meeting today? Project has been already finished and I have results to show you.',
        assign: 'Minh Nguyen',
        done: true
      }, {
        title: 'Udate Notification',
        assign: 'Minh Nguyen',
        project: 'Used to show feedback after an activity. The difference with Notification is that the latter is often used to show a system level passive notification',
        done: true
      }, {
        title: 'Radio Function',
        assign: 'Minh Nguyen',
        project: 'Radio should not have too many options. Otherwise, use the Select component instead',
        done: false
      }, {
        title: 'Table fix',
        assign: 'Minh Nguyen',
        project: 'Display multiple data with similar format. You can sort, filter, compare your data in a table.',
        done: false
      }]
    }
  },
  created () {
    if (this.$store.state.role === 0) {
      this.$http.get(`api/todos`).then(res => {
        this.todos = res.body
      })
    } else {
      this.$http.get(`api/todos/ems/${this.$store.state.uid}`).then(res => {
        this.todos = res.body
      })
    }
    this.$http.get(`api/employees`).then(res => {
      this.ems = res.body
      this.ems = this.ems.map(function (x) {
        x.department = x.department != null ? x.department.name : 'None'
        return x
      })
    })
  },
  methods: {
    createTodo: function (newTodo) {
      this.todos.push(newTodo)
      sweetalert('Success!', 'To-Do created!', 'success')
    },
    handleCommand (command) {
      this.currentEms = command
      if (this.ems[this.currentEms]) {
        this.$http.get(`api/todos/ems/${this.ems[this.currentEms]._id}`).then(res => {
          this.todos = res.body
        })
      }
    }
  }
}
</script>
<style lang="scss" scoped>
.pending > b {
  background: #878d99 !important;
}
.warning > b {
  background: #08adb3 !important;
}
.confirm > b {
  background: #fb7777 !important;
}
.approval > b {
  background: #efb137 !important; 
}
.completed > b {
  background: #67c23a !important;
}
.choose-ems {
  border-radius: 5px;
  &:hover {
    background-color: #f0f0f0;
  }
}
.over-ems {
  overflow: hidden;
  padding: 20px;
}
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
</style>
<template>
    <div class="create-todo">
        <el-button type="info" v-on:click="openForm" v-show="!isCreating"><i class='ti-pencil-alt'></i>&nbsp;&nbsp;Add New Task</el-button>
        <div v-show="isCreating">
            <div class='content-todo-devin'>
                <div>
                    <label>Title</label>
                    <p>
                        <el-input placeholder="Please input" v-model="form.title" type='text'></el-input>
                    </p>
                </div>
                <div>
                    <label>Assignment</label>
                    <p>
                      <el-select
                        v-model="form.assign"
                        multiple
                        style="width: 100%;"
                        filterable
                        remote
                        placeholder="Please enter a keyword"
                        :remote-method="remoteMethod"
                        :loading="loading">
                        <el-option
                          v-for="item in options4"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value">
                        </el-option>
                      </el-select>
                    </p>
                </div>
                <div>
                    <label>Content</label>
                    <p>
                        <el-input v-model="form.content" type='textarea' :rows="4" placeholder="Please input"> </el-input>
                    </p>
                </div>
                
                <div>
                    <el-button v-on:click="sendForm()" type="info">Create</el-button>
                    <el-button v-on:click="closeForm" type="info">Cancel</el-button>
                  
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
  data: function () {
    return {
      titleText: '',
      projectText: '',
      isCreating: false,
      list: [],
      ems: [],
      loading: false,
      options4: [],
      form: {
        worker: [],
        title: null,
        content: null
      }
    }
  },
  created () {
    this.$http.get(`api/employees`).then(res => {
      this.ems = res.body
      this.ems = this.ems.map(function (x) {
        x.department = x.department != null ? x.department.name : 'None'
        return x
      })
      this.list = this.ems.map(item => {
        return { value: item._id, label: item.name }
      })
    })
  },
  methods: {
    openForm: function () {
      this.isCreating = true
    },
    closeForm: function () {
      this.isCreating = false
    },
    sendForm: function () {
      if (this.form.title != null && this.form.content != null && this.form.assign.length > 0) {
        this.$http.post(`api/todos`, this.form).then(res => {
          if (res.status === 201) {
            console.log(res.body)
          }
        })
      }
    },
    remoteMethod (query) {
      if (query !== '') {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options4 = this.list.filter(item => {
            return item.label.toLowerCase()
              .indexOf(query.toLowerCase()) > -1
          })
        }, 200)
      } else {
        this.options4 = []
      }
    }
  }
}
</script>

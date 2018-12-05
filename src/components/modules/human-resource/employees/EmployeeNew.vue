<template>
  <div class="box" v-if="departments">
    <new>
      <div class="box-header with-border">
        <h3 class="box-title">Create an employee</h3>
      </div>
      <el-form ref="form" :model="form" label-width="120px">
        <h3>Basic</h3>
        <el-form-item label="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="email">
          <el-input v-model="form.email"></el-input>
        </el-form-item>
        <el-form-item label="department">
          <el-select v-model="form.department" placeholder="please select department">
            <el-option v-for="(depart, index) of departments" :key="index" :label="depart.name" :value="depart._id"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div style="float: right">
        <el-button type="success" @click="onSubmit">Create</el-button>
        <el-button>Cancel</el-button>
      </div>
    </new>
  </div>
</template>
<script>
import New from '../../../partials/New.vue'

export default {
  name: 'ProductNew',
  components: {
    'new': New
  },
  data () {
    return {
      departments: [],
      form: {
        name: null,
        department: null,
        email: null
      },
      options: []
    }
  },
  created () {
    this.$http.get(`api/departments`).then(res => {
      this.departments = res.body
      console.log(this.departments)
    })
  },
  methods: {
    onSubmit () {
      this.$http.post(`api/employees`, this.form).then(res => {
        if (res.status === 201) {
          this.$router.push({name: 'EmployeesList'})
        }
      })
    }
  }
}
</script>
<style lang="scss">
</style>
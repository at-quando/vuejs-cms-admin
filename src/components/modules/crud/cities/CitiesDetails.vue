<template>
  <div class="box">
    <new :urlList="'package'">
      <div class="box-header with-border">
        <h3 class="box-title">Edit a City</h3>
      </div>
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <div class="box-header with-border">
          <h3 class="box-title">Edit Colleges</h3>
        </div>
        <div class="row">
          <el-form-item class="col-md-5" label="College Name">
            <el-input v-model="currentCollege.name"></el-input>
          </el-form-item>
          <el-form-item class="col-md-4" label="College Key">
            <el-input v-model="currentCollege.key"></el-input>
          </el-form-item>
          <el-button class="col-md-3" type="warning" @click="createCollege">Add College to this City </el-button>
        </div>
      </el-form>
      <div class="box-header with-border">
        <h3 class="box-title">Colleges List</h3>
      </div>
      <el-table :data="form.colleges">
        <el-table-column property="name" label="name" width="350"></el-table-column>
        <el-table-column property="key" label="key" width="100"></el-table-column>
        <el-table-column
          label="Actions"
          width="120">
          <template scope="scope">
            <el-button type="text" size="small" @click="removeItem(scope)">Remove</el-button>
          </template>
        </el-table-column>
      </el-table>
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
  name: 'CityNew',
  components: {
    'new': New
  },
  data () {
    return {
      form: {
        name: '',
        colleges: []
      },
      currentCollege: {
        name: '',
        key: ''
      }
    }
  },
  created () {
    this.$http.get(`api/cities/${this.$route.params.id}`)
      .then(res => {
        this.form = res.body
      })
  },
  methods: {
    onSubmit () {
      this.$http.put(`api/cities/${this.$route.params.id}`, this.form)
        .then(res => {
          if (response.status === 201) {
            this.$router.push({path: '/cities'})
          }
        })
        .catch(function (e) {
          if (e.status === 401) {
            console.log(33342)
          }
        })
    },
    createCollege () {
      this.form.colleges.push(this.currentCollege)
      this.currentCollege = {}
    },
    removeItem (scope) {
      this.form.colleges = this.form.colleges.filter(x => x.key !== scope.row.key)
    }
  }
}
</script>
<style lang="scss">
</style>
<template>
  <div>
    <bread-crumb>
      <template slot="button">
        <router-link :to="{ path: 'new'}" append><button type="button" style="float: right" class="el-button el-button--primary"><span>New {{titleList}}</span></button></router-link>
      </template>
    </bread-crumb>
    <section class="content">
      <div class="box" style="padding:30px;">
        <h3>Departments</h3>
        <div class="row">
          <div class="col-md-2" v-for="(item, index) of departments" :key="index">
            <router-link :to="{ name: 'department-details', params: {id: item._id}}" append>
              <div class="box bg" :class="color[index % 4]">
                <div class="box-header bg" :class="color[index % 4]">
                  <div class="pull-right text-white box-tools"> <span class="text-white box-btn"><i class="circle-nest-medium ti-user"></i></span>
                    &nbsp;&nbsp;&nbsp; 
                  </div>
                </div>
                <div class="box-body no-pad" style="display: block;">
                  <div class="events-nest ">
                    <div class="wrap-no-pad text-white">
                      <h1 class="text-white"><strong><animate-number
                          from="0" 
                          :to="item.numberOfUsers" 
                          duration="500" 
                          easing="easeOutQuad"
                          :formatter="formatter"
                          ></animate-number></strong>
                      </h1>
                      <span>{{item.name}}</span>
                      <p>65%</p>
                      <br>
                    </div>
                  </div>
                </div>
              </div>
            </router-link>
          </div>
        </div>
        <h3> Create New Department</h3>
        <el-form ref="form" :model="form" label-width="120px">
          <el-row>
            <el-col :span="10">
              <el-form-item label="name">
                <el-input v-model="form.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="10">
              <el-button type="success" @click="onSubmit">Create</el-button>
            </el-col>
          </el-row>
        </el-form>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  name: 'Department',
  data () {
    return {
      color: [
        'palette-Cyan-400',
        'palette-Pink-300',
        'palette-Indigo-900',
        'palette-Deep-Purple-300'
      ],
      departments: [],
      type: [],
      form: {
        name: null
      },
      options: []
    }
  },
  created () {
    this.$http.get(`api/departments/num`).then(res => {
      this.departments = res.body
    })
  },
  methods: {
    onSubmit () {
      this.$http.post('api/departments', this.form)
        .then(res => {
          if (response.status === 201) {
            console.log(12, res.body)
          }
        })
        .catch(function (e) {
          if (e.status === 401) {
            console.log(33342)
          }
        })
    }
  }
}
</script>
<style lang="scss" scoped>
.box-body {
  padding: 0 30px;
}
</style>
<template>
  <div>
    <bread-crumb>
      <template slot="button">
        <router-link :to="{ path: 'new'}" append><button type="button" style="float: right" class="el-button el-button--primary"><span>New {{titleList}}</span></button></router-link>
      </template>
    </bread-crumb>
    <section class="content">
      <div class="box" style="padding:30px;">
        <new>
          <div class="box-header with-border">
            <h3 class="box-title">Create a department</h3>
          </div>
          <el-form ref="form" :model="form" label-width="120px">
            <h3>Basic</h3>
            <el-form-item label="name">
              <el-input v-model="form.name"></el-input>
            </el-form-item>
          </el-form>
          <div style="float: right">
            <el-button type="success" @click="onSubmit">Create</el-button>
            <el-button>Cancel</el-button>
          </div>
        </new>
      </div>
    </section>
  </div>
</template>
<script>
export default {
  name: 'Department',
  data () {
    return {
      type: [],
      form: {
        name: null,
        costHire: 0,
        costBuy: 0,
        shortDesc: '',
        desc: '',
        attributes: {},
        type: null
      },
      options: []
    }
  }
}
</script>
<style lang="scss" scoped>
.box-body {
  padding: 0 30px;
}
</style>
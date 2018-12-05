<template>
  <div class="box">
    <new>
      <div class="box-header with-border">
        <h3 class="box-title">Create a product</h3>
      </div>
      <el-form ref="form" :model="form" label-width="120px">
        <h3>Basic</h3>
        <el-form-item label="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="costHire">
          <el-input v-model="form.costHire"></el-input>
        </el-form-item>
        <el-form-item label="costBuy">
          <el-input v-model="form.costBuy"></el-input>
        </el-form-item>
        <el-form-item label="shortDesc">
          <el-input v-model="form.shortDesc"></el-input>
        </el-form-item>
        <el-form-item label="desc">
          <el-input v-model="form.desc"></el-input>
        </el-form-item>
        <el-form-item label="type">
          <span>{{type.type}}</span>
        </el-form-item>
        <h3>Additional Attributes</h3>
        <el-row>
          <el-col :span="12" v-for="(item, index) of type.attrs" :key="index" >
            <el-form-item :label="item">
              <el-input v-model="form.attributes[item]"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
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
  },
  created () {
    this.$http.get(`api/productTypes/${this.$route.params.product}`).then(res => {
      this.type = res.body
      this.form.type = this.type.type
    })
  },
  methods: {
    onSubmit () {
      this.$http.post(`api/products`, this.form).then(res => {
        if (res.status === 201) {
          this.$router.push({path: this.$route.path.replace('/new', '')})
        }
      })
    }
  }
}
</script>
<style lang="scss">
</style>
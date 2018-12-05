<template>
  <div class="box">
    <editForm componentName='Products'>
      <div class="box-header with-border">
        <h3 class="box-title">Edit a product</h3>
      </div>
      <el-form ref="form" v-if="form" :model="form" label-width="120px">
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
    </editForm>
  </div>
</template>
<script>
import Edit from '../../../partials/Edit.vue'

export default {
  name: 'ProductNew',
  components: {
    'editForm': Edit
  },
  data () {
    return {
      type: [],
      form: null,
      options: []
    }
  },
  created () {
    this.$http.get(`api/productTypes/${this.$route.params.product}`).then(res => {
      this.type = res.body
    })
    this.$http.get(`api/products/info/${this.$route.params.id}`).then(res => {
      this.form = res.body
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
<template>
  <div class="box">
    <new :urlList="'package'">
      <div class="box-header with-border">
        <h3 class="box-title">Update a Package</h3>
      </div>
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="price">
          <el-input v-model="form.price"></el-input>
        </el-form-item>
        <el-form-item label="priceFirst">
          <el-input v-model="form.priceFirst"></el-input>
        </el-form-item>
        <el-form-item label="offer">
          <el-input v-model="form.offer"></el-input>
        </el-form-item>
        <el-form-item label="types">
          <el-input v-model="form.types"></el-input>
        </el-form-item>
        <el-form-item label="aspect">
          <el-input v-model="form.aspect"></el-input>
        </el-form-item>
        <el-form-item label="level">
          <el-select v-model="form.level" placeholder="please select package level">
            <el-option label="Level 0" value="0"></el-option>
            <el-option label="Level 1" value="1"></el-option>
            <el-option label="Level 2" value="2"></el-option>
            <el-option label="Level 3" value="3"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="duration">
          <el-input v-model="form.duration"></el-input>
        </el-form-item>
      </el-form>
      <div style="float: right">
        <el-button type="success" @click="onSubmit">Update</el-button>
        <el-button>Cancel</el-button>
      </div>
    </new>
  </div>
</template>
<script>
import New from '../../../partials/New.vue'

export default {
  name: 'PackageNew',
  components: {
    'new': New
  },
  data () {
    return {
      form: {
        name: '',
        price: 0,
        price_first: 0,
        offer: '',
        types: '',
        aspect: '',
        level: 0,
        duration: ''
      }
    }
  },
  created () {
    this.$http.get(`api/packages/${this.$route.params.id}`)
      .then(res => {
        this.form = res.body
      })
  },
  methods: {
    onSubmit () {
      this.$http.put(`api/packages/${this.$route.params.id}`, this.form)
        .then(res => {
          this.$router.push({path: '/packages'})
        })
    }
  }
}
</script>
<style lang="scss">
</style>
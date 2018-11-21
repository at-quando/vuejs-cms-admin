<template>
  <div class="box">
    <new>
      <div class="box-header with-border">
        <h3 class="box-title">Create a Package</h3>
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
          <el-input v-model="form.level"></el-input>
        </el-form-item>
        <el-form-item label="duration">
          <el-input v-model="form.duration"></el-input>
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
  methods: {
    onSubmit () {
      console.log('submit!')
    }
  }
}
</script>
<style lang="scss">
</style>
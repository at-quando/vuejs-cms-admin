<template>
  <div class="box">
    <list v-if="data" :attrTable="attrs" :titleList="'package'" :tableData="data" :urlList="'/package'"/>
  </div>
</template>
<script>
import List from '../../../partials/List.vue'

export default {
  name: 'PackageList',
  data () {
    return {
      attrs: ['name', 'price', 'price_first', 'offer', 'types', 'aspect', 'level', 'duration'],
      data: []
    }
  },
  created () {
    this.$http.get('api/packages').then(res => {
      this.data = res.body
    })
  },
  components: {
    'list': List
  }
}
</script>
<style lang="scss">
</style>
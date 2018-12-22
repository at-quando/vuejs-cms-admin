<template>
  <div class="box">
    <list v-if="data" :attrTable="attrs" :titleList="'city'" :tableData="data" :urlList="'/cities'"/>
  </div>
</template>
<script>
import List from '../../../partials/List.vue'

export default {
  name: 'PackageList',
  data () {
    return {
      attrs: ['name', 'colleges'],
      data: []
    }
  },
  created () {
    this.$http.get('api/cities').then(res => {
      this.data = res.body
      this.data.forEach((x) => {
        x.colleges = x.colleges.map(x => `${x.name} - ${x.key}`)
      })
    })
  },
  components: {
    'list': List
  }
}
</script>
<style lang="scss">
</style>
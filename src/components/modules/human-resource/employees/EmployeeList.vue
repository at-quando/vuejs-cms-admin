<template>
  <div class="box">
    <list :attrTable="attrs" :titleList="'contract'" :tableData="ems" :urlList="'/contract'"/>
  </div>
</template>
<script>
import List from '../../../partials/List.vue'

export default {
  name: 'ContractList',
  components: {
    'list': List
  },
  data () {
    return {
      ems: [],
      attrs: ['name', 'email', 'department', 'phone']
    }
  },
  created () {
    this.$http.get(`api/employees`).then(res => {
      this.ems = res.body
      this.ems = this.ems.map(function (x) {
        x.department = x.department != null ? x.department.name : 'None'
        return x
      })
    })
  }
}
</script>
<style lang="scss">
</style>
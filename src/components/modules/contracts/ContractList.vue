<template>
  <div class="box">
    <list :attrTable="['representative', 'phone1', 'group', 'client', 'packages', 'taken_dates']" :titleList="'contract'"  :tableData="tableData" :urlList="'/contract'"/>
  </div>
</template>
<script>
import List from '../../partials/List.vue'

export default {
  name: 'ContractList',
  data () {
    return {
      tableData: []
    }
  },
  components: {
    'list': List
  },
  created () {
    this.$http.get('api/contracts').then(res => {
      this.tableData = res.body
      this.tableData.map((x) => {
        x.taken_dates = x.taken_dates.map(x => x.start_time)
      })
    })
  }
}
</script>
<style lang="scss">
</style>
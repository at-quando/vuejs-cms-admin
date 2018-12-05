<template>
  <div class="box">
    <list :attrTable="['a','b','c']" :titleList="'package'"  :urlList="'/package'"/>
  </div>
</template>
<script>
import List from '../../../partials/List.vue'

export default {
  name: 'PackageList',
  components: {
    'list': List
  }
}
</script>
<style lang="scss">
</style>
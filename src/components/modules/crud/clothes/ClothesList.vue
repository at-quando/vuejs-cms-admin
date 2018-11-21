<template>
  <div class="box">
    <list :attrTable="['a','b','c']" :titleList="'clothes'"  :urlList="'clothes'"/>
  </div>
</template>
<script>
import List from '../../../partials/List.vue'

export default {
  name: 'ClothesList',
  components: {
    'list': List
  }
}
</script>
<style lang="scss">
</style>
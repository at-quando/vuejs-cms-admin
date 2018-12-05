<template>
  <div>
    <h1>1231232</h1>
  </div>
</template>
<script>
export default {
  name: 'EmployeeDetail',
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
  }
}
</script>
<style lang="scss" scoped>
.box-body {
  padding: 0 30px;
}
</style>
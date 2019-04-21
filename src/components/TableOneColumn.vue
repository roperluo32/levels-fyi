<template>
  <div class="TableOneColumn">
    <el-button round>Google</el-button>
    <el-table :data="tableData" border style="width: 100%" :show-header="false">
      <el-table-column prop="name" width="180"> </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'TableOneColumn',
  data () {
    return {
      tableData: [{
        name: 'L3 SWE II'
      }, {
        name: 'L4 SWE III'
      }, {
        name: 'L5 Senior SWE'
      }, {
        name: 'L6 Staff SWE'
      }]
    }
  }
}
</script>

<style scoped>
</style>

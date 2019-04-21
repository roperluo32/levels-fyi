<template>
  <span class="companybuttons">
    <!-- <p>{{ jobType }}</p> -->
    <el-button round>Intercom</el-button>
    <el-button round>Google</el-button>
    <el-button round>Facebook</el-button>
    <el-button round>Microsoft</el-button>
    <el-button round>Amazon</el-button>
    <el-button round>More</el-button>
  </span>
</template>

<script>
export default {
  name: 'CompanyButtons',
  props: ['jobType', 'companyLevelInfo']
}
</script>

<style scoped>
.select-round {
  border-radius: 20px;
  padding: 12px 23px;
}
</style>

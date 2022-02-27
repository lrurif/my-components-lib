<template>
  <div id="app">
    <div id="nav">
        <VirtualList></VirtualList>
        <f-table :data="[{name: '123'}]">
          <el-table-column prop="name"></el-table-column>
        </f-table>
    </div>
    <router-view/>
  </div>
</template>
<script>
  import VirtualList from "@/components/VirtualList"
  import FTable from "@/components/FTable"
  export default {
    components: {
      VirtualList,
      FTable
    }
  }
</script>
<style lang="scss">

</style>

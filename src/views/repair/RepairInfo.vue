<template>
  <el-container>
    <el-header></el-header>
    <el-main>
      <el-card
        style="margin: 30px 40px; "
        class="box-card"
        v-for="(repiar,index) in repiars"
        :key="index"
      >
        <div class="card-text">报修: {{repair.content}}</div>
        <div class="card-text">描述: {{notice.publisher}}</div>
        <div class="card-text">报修时间: {{notice.build_name}}</div>
      </el-card>
    </el-main>
  </el-container>
</template>

<script>
export default {
  name: "RepairInfo",
  data(){
    return{
      repiars:[]
    }
  }
}
</script>

<style scoped>
.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

.box-card {
  width: 480px;
}



.card-text{
  padding-bottom: 20px;
}

</style>

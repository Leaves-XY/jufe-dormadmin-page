<template>
  <el-container>
    <el-header></el-header>
    <el-main>
      <div class="card-container">
        <el-card
          v-for="(repair, index) in repiars"
          :key="index"
          class="box-card"
        >
          <div slot="header" class="clearfix">
            <span>报修: {{repair.title}}</span>
            <el-button style="float: right; padding: 3px 0" type="text" @click="handleDeleteRepair(repair.id)">已处理/删除</el-button>
          </div>
          <div class="card-text">描述: {{repair.description}}</div>
          <div class="card-text">报修时间: {{repair.createAt}}</div>
        </el-card>
      </div>
    </el-main>
  </el-container>
</template>

<script>
import {deleteRequest, getRequest} from "../../api/api";

export default {
  name: "RepairInfo",
  data() {
    return {
      repiars: []
    };
  },
  mounted() {
    this.initRepairs();
  },
  methods: {
    initRepairs() {
      getRequest("/api/repair/list")
        .then(resp => {
          this.repiars = resp;
          console.log(this.repiars);
        })
        .catch(error => console.error("Error:", error));
    },
    handleDeleteRepair(id){
      deleteRequest('/api/repair/del',{id:id})
        .then(response=>{
          this.initRepairs();
        })
        .catch(error => console.error('Error:', error));
    }
  }
};
</script>

<style scoped>
.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.box-card {
  width: 480px;
  margin: 20px;
}

.card-text {
  padding-bottom: 20px;
}

@media (max-width: 768px) {
  .box-card {
    width: 100%;
  }
}
</style>

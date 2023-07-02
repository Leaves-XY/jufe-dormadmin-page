<template>
  <el-container>
    <el-header></el-header>
    <el-main>
      <div class="card-container">
        <el-card
          v-for="(feedback, index) in feedbacks"
          :key="index"
          class="box-card"
        >
          <div class="card-text">反馈学生: {{feedback.stuName}}</div>
          <div class="card-text">描述: {{feedback.reason}}</div>
          <div class="card-text">反馈时间: {{feedback.createAt}}</div>
        </el-card>
      </div>
    </el-main>
  </el-container>
</template>

<script>
import { getRequest } from "../../api/api";

export default {
  name: "FeedbackInfo",
  data() {
    return {
      feedbacks: []
    };
  },
  mounted() {
    this.initFeedbacks();
  },
  methods: {
    initFeedbacks() {
      getRequest("/api/feedback/list")
        .then(resp => {
          this.feedbacks = resp;
          console.log(this.feedbacks);
        })
        .catch(error => console.error("Error:", error));
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

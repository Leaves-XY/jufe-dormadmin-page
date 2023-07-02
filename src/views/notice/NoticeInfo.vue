<template>
  <div>
    <el-container>
      <el-header>
        <el-button style="float: right;" type="primary" @click="showForm = true">发布通知</el-button>
        <el-dialog title="发布通知" :visible.sync="showForm" @close="resetForm">
          <el-form ref="form" :model="newNotice" label-width="120px">
            <el-form-item label="通知类型">
              <el-input v-model="newNotice.type"></el-input>
            </el-form-item>
            <el-form-item label="通知内容">
              <el-input v-model="newNotice.content"></el-input>
            </el-form-item>
            <el-form-item label="发布人">
              <el-input v-model="newNotice.publisher"></el-input>
            </el-form-item>
            <el-form-item label="寝室楼">
              <el-select v-model="newNotice.build_id" placeholder="请选择">
                <el-option
                  v-for="building in buildings"
                  :key="building.id"
                  :label="building.name"
                  :value="building.id">
                </el-option>
              </el-select>
            </el-form-item>
          </el-form>
          <span slot="footer" class="dialog-footer">
            <el-button @click="showForm = false">取 消</el-button>
            <el-button type="primary" @click="submitForm('form')">确 定</el-button>
          </span>
        </el-dialog>
      </el-header>
      <el-main style="display: flex; justify-content: flex-start; align-items: flex-start; flex-wrap: wrap;">
        <el-card
          style="margin: 30px 40px; "
          class="box-card"
          v-for="(notice,index) in notices"
          :key="index"
        >

          <div slot="header" class="clearfix">
            <span>{{notice.type}}</span>
            <el-button style="float: right; padding: 3px ;padding-right: 20px " type="text" @click="handleEditNotice(notice)">编辑</el-button>
            <el-button style="float: right; padding: 3px 0" type="text" @click="handleDeleteNotice(notice.id)">删除</el-button>
          </div>

          <div class="card-text">通知内容: {{notice.content}}</div>
          <div class="card-text">发布人: {{notice.publisher}}</div>
          <div class="card-text">寝室楼: {{notice.build_name}}</div>
          <div class="card-text">发布时间: {{notice.createAt}}</div>

        </el-card>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import {deleteRequest, getRequest, postRequest, putRequest} from "../../api/api";

export default {
  name: "NoticeInfo",
  data(){
    return{
      showForm: false,
      newNotice: {
        type: '',
        content: '',
        publisher: '',
        build_id: '',
      },
      notices:[],
      buildings: [],  // 在这里添加 buildings 属性
      editMode: false,  // 添加这个属性
    };
  },
  mounted() {
    this.initNotices();
    this.getBuildings();
  },
  methods:{
    initNotices(){
      getRequest("/api/notice/list")
        .then(resp => {
          if(resp){
            this.notices=resp;
          }
        })
        .catch(error => console.error('Error:', error));
    },
    handleDeleteNotice(id) {
      // 确认用户是否真的想要删除这个通知
      // 如果用户确认，调用后端的删除API
      deleteRequest('/api/notice/del', { id: id })
        .then(response => {
          this.initNotices();
        })
        .catch(error => console.error('Error:', error));
    },
    handleEditNotice(notice) {
      this.newNotice = Object.assign({}, notice);
      this.editMode = true;
      this.showForm = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          const noticeData = Object.assign({}, this.newNotice); // 创建新对象并复制属性值
          if (this.editMode) {
            // 先创建一个新的通知
            postRequest('/api/notice/add', noticeData)
              .then(response => {
                // 如果添加成功，再删除旧的通知
                deleteRequest('/api/notice/del', { id: this.newNotice.id })
                  .then(response => {
                    this.showForm = false;
                    this.resetForm();
                    this.initNotices();
                  })
                  .catch(error => console.error('Error:', error));
              })
              .catch(error => console.error('Error:', error));
          } else {
            // 否则，调用创建通知的 API
            postRequest('/api/notice/add', noticeData)
              .then(response => {
                this.showForm = false;
                this.resetForm();
                this.initNotices();
              })
              .catch(error => console.error('Error:', error));
          }
        } else {
          return false;
        }
      });
    },
    resetForm(){
      this.newNotice = {
        type: '',
        content: '',
        publisher: '',
        build_id: '',
      };
      this.editMode = false;  // 重置 editMode
    },
    getBuildings() {
      getRequest("/api/building/all")
        .then(response => {
          this.buildings = response;
        })
        .catch(error => console.error('Error:', error));
    },

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

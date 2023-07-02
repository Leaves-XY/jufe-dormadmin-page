<template>
  <el-container class="el-main">
    <el-header class="homeHeader " style="background-color: rgb(193,38,44) ;">

      <div style="font-size: 16px;font-weight: bold ;color: white">
        {{currentDate}}
      </div>

      <div>
        <img src="../assets/logo.png"
             style="height: 50px ;width: 50px"
        >
      </div>

      <div class="title">江西财经大学宿舍管理系统</div>
      <div>
        <el-dropdown class="profile" @command="commandHandler">
          <span class="el-dropdown-link">
            {{user.name}}
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="profile">个人中心</el-dropdown-item>
            <el-dropdown-item
              command="logout"
              divided
            >注销登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-header>
    <el-container>
      <el-aside width="200px">
        <el-menu
          router
          unique-opened
        >
          <el-submenu
            :index="index+''"
            v-for="(item,index) in routes"
            v-if="!item.hidden"
            :key="index"
          >
            <template slot="title">
              <i
                style="color: #409eff;margin-right: 5px;"
                :class="item.iconCls"
              ></i>
              <span style="font-size:18px">{{item.name}}</span>
            </template>
            <el-menu-item
              :index="child.path"
              v-for="(child,indexj) in item.children"
              :key="indexj"
              style="font-size:16px"
            >
              {{child.name}}
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <el-breadcrumb
          separator-class="el-icon-arrow-right"
          v-if="this.$router.currentRoute.path!='/home'"
        >
          <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item>{{this.$router.currentRoute.name}}</el-breadcrumb-item>
        </el-breadcrumb>
        <div
          class="homeWelcome"
          v-if="this.$router.currentRoute.path=='/home'"
        >
          系统主页
        </div>
        <router-view class="homeRouterView" />


      </el-main>
      <template>


      </template>
    </el-container>

  </el-container>
</template>

<script>
export default {
  name: "Home",
  data() {

    return {
      currentDate: "", // 当前日期
      // user: JSON.parse(window.sessionStorage.getItem("user"))
    };
  },
  mounted() {
    this.getCurrentDate(); // 页面加载时获取当前日期
    this.startClock(); // 开始实时更新日期
  },
  computed: {
    routes() {
      return this.$store.state.routes;
    },
    user() {
      return this.$store.state.currentUser;
    }
  },
  methods: {
    getCurrentDate() {
      const date = new Date();
      this.currentDate = this.formatDate(date);
    },
    formatDate(date) {
      // 格式化日期为 "yyyy-MM-dd HH:mm:ss" 格式
      const year = date.getFullYear();
      const month = this.padZero(date.getMonth() + 1);
      const day = this.padZero(date.getDate());
      const hours = this.padZero(date.getHours());
      const minutes = this.padZero(date.getMinutes());
      const seconds = this.padZero(date.getSeconds());
      return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
    },
    padZero(value) {
      // 补零
      return value < 10 ? `0${value}` : value;
    },
    startClock() {
      // 开始实时更新日期
      setInterval(() => {
        this.getCurrentDate();
      }, 1000);
    },
    commandHandler(cmd) {
      if (cmd == "logout") {
        this.$confirm("此操作将注销登录, 是否继续?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
          .then(() => {
            this.getRequest("/api/logout");
            window.sessionStorage.removeItem("user");
            this.$store.commit("initRoutes", []);
            this.$router.replace("/");
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "已取消操作"
            });
          });
      } else if (cmd == "profile") {
        this.$router.push("/profile");
      }
    }
  }
};
</script>

<style>


.homeRouterView {
  background-color: white;
  margin-top: 10px;

}
.el-main{

  /*background-image: url("http://lib.jxufe.cn/sites/all/themes/jxcdlib/images/bg-resource.jpg");*/
  min-height: 100vh; /* 设置最小高度为视口高度，确保占满整个屏幕 */
}


/* 以下是左侧菜单栏样式 */
.el-aside {
  background-color: #f5f5f5; /* 你可以选择你喜欢的颜色 */
}

.el-menu-item, .el-submenu__title {
  font-size: 16px; /* 改变字体大小 */
  border-bottom: 1px solid #e5e5e5; /* 为每个菜单项添加底边框 */
  transition: background-color 0.3s ease; /* 为背景颜色变化添加过渡效果 */
}

.el-menu-item:hover, .el-submenu__title:hover {
  background-color: rgba(223,5,37,0.3); /* 当鼠标悬停在菜单项上时，改变背景色 */
}

.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 150px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}

.homeWelcome {
  text-align: center;
  font-size: 30px;
  font-family: 华文行楷;
  color: #409eff;
  padding-top: 50px;

}

.homeHeader {
  background-color: #409eff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 15px;
  box-sizing: border-box;
}

.homeHeader .title {
  font-size: 30px;
  font-family: 华文行楷;
  color: #ffffff;
}

.homeHeader .profile {
  font-size: 16px;
  color: #ffffff;
  cursor: pointer;
}

.el-dropdown-link {
  display: flex;
  align-items: center;
}
</style>

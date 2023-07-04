<template>
  <div>
    <div>
       系统设置
    </div>

    <div class="block" style="margin-top: 50px">
      <div class="carousel-wrapper">
        <el-carousel height="500px" style="width: 80%">
          <el-carousel-item v-for="image in carouselImages" :key="image">
            <img :src="image" alt="carousel-image" class="carousel-image" />
          </el-carousel-item>
        </el-carousel>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      carouselImages: [
        'https://www.jxufe.edu.cn/uploadfile/69/Attachment/cec198155c.png',
        "https://sns-img-qc.xhscdn.com/01028b01ke594re76ua010p6gkvdpu3ncx?imageView2/2/w/1920/format/webp|imageMogr2/strip",
        'https://img1.baidu.com/it/u=1289705404,2948421387&fm=253&fmt=auto&app=138&f=JPEG?w=887&h=500',
        'http://lib.jxufe.cn/sites/all/themes/jxcdlib/images/bg-resource.jpg'
      ]
    }
  }
}
</script>

<style scoped>
.block {
  width: 70%;
  display: flex;
  justify-content: center;
}

.carousel-wrapper {
  width: 100%;
  display: flex;
  justify-content: center;
}

.carousel-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
</style>

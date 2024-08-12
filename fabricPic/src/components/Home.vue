<template>
  <div class="home">
    <div class="pic-wall-wrap">
     <div>
       <p><el-button style="width: 80px;" @click="visible = true" type="primary" round>照片库</el-button></p>
       <p style="margin-top: 10px;"><el-button style="width: 80px;" @click="clear" round>清空</el-button></p>
     </div>
      <div class="canvas-wrap">
        <canvas id="canvas" width="1000" height="600"></canvas>
      </div>
      <div style="width: 0;height: 0;overflow: hidden;">
        <img v-for="(item, index) in imgList" :key="index" :src="item" :id="`imgEl${index+1}`">
      </div>
    </div>
    <el-dialog title="图片库" :visible.sync="visible">
      <div class="img-list-wrap">
        <div class="img-list">
          <div v-for="(item, index) in imgAlertList" :key="index" class="img-item"
               @click="activeIndex = index"
               :class="activeIndex === index ? 'active' : ''">
            <img :src="item.url">
          </div>
        </div>
        <div class="img-info-wrap">
          <div class="img-box">
            <img :src="imgAlertList[activeIndex].url">
          </div>
          <p style="padding: 10px;">{{ imgAlertList[activeIndex].desc }}</p>
          <el-button style="width: 100%;" round @click="useImg">使用照片</el-button>
        </div>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import * as fabric from 'fabric'

export default {
  name: 'Home',
  data () {
    return {
      canvas: null,
      imgList: [],
      imgAlertList: [
        {
          id: 1,
          url: require('../assets/1.jpeg'),
          desc: '1111111111',
        },
        {
          id: 2,
          url: require('../assets/2.jpg'),
          desc: '222222222222',
        },
        {
          id: 3,
          url: require('../assets/3.jpg'),
          desc: '3333333333333',
        }
      ],
      visible: false,
      activeIndex: 0,
    }
  },
  mounted() {
    this.canvas = new fabric.Canvas('canvas');
    //设置不能群体选择
    this.canvas.selection = false;
  },
  methods: {
    useImg() {
      if (this.imgList.length) {
        const filterArr = this.imgList.filter(item=>{
          return item === this.imgAlertList[this.activeIndex].url
        })
        if (filterArr && filterArr.length) {
          this.$message({
            message: '该图片已经选过了！',
            type: 'warning'
          });
        } else {
          this.imgList.push(this.imgAlertList[this.activeIndex].url);
          this.visible = false;
          this.activeIndex = 0;
          this.$nextTick(()=>{
            this.refreshImgList();
          })
        }
      } else {
        this.imgList.push(this.imgAlertList[this.activeIndex].url);
        this.visible = false;
        this.activeIndex = 0;
        this.$nextTick(()=>{
          this.refreshImgList();
        })
      }
    },
    refreshImgList() {
      if (this.imgList.length) {
        let imgElement = document.getElementById(`imgEl${this.imgList.length}`);
        let imgInstance = new fabric.Image(imgElement,{
          left:100,
          top:10,
        });
        imgInstance.set({
          scaleX: 0.2,
          scaleY: 0.2
        });
        this.canvas.add(imgInstance);
      }
    },
    clear() {
      this.canvas.clear();
    },
  },
}
</script>
<style scoped>
*{
  box-sizing: border-box;
  padding:0;
  margin:0;
}
.home{
  padding: 50px;
}
.pic-wall-wrap{
  position: relative;
}
.canvas-wrap{
  width: 1000px;
  height: 600px;;
  position: absolute;
  left: 100px;
  top: 0px;
  background: url('../assets/bg.png') no-repeat;
  background-size: 100% 100%;
}
.img-list-wrap{
  display: flex;
}
.img-list{
  width: 700px;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  border: 1px solid #efefef;
  margin-right: 20px;
}
.img-item{
  width: 200px;
  height: 200px;
  padding: 20px;
  border: 1px solid #efefef;
  border-radius: 5px;
}
.img-item.active{
  border: 1px solid #e87e11;
  background-color: #e87e11;
}
.img-item img{
  width: 160px;
  height: 160px;
}
.img-info-wrap{
  width: 220px;
  padding: 10px;
  border: 1px solid #efefef;
}
.img-box{
  width: 200px;
  height: 200px;
  padding: 20px;
}
.img-box img{
  width: 160px;
  height: 160px;
}
</style>

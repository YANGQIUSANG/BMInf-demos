<template>
  <div class="head">
    <div class="right">
      <ul class="my">
        <li class="img">内存使用率<span>{{memory_rate}}</span>%</li>
        <li class="img">GPU使用率<span>{{gpu_rate}}</span>%</li>
        <li class="img">显存使用率<span>{{meme_used_rate}}</span>%</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    memory_rate: 0,
    gpu_rate: 0,
    meme_used_rate: 0,

    timer_handle : null
  }),
  created() {
    this.get_gpu_info();
    this.timer_handle = setInterval(this.get_gpu_info, 3000);
  },
  destroyed() {
    clearInterval(this.timer_handle);
  },
  methods: {
    get_gpu_info() {
      axios.get("/api/gpuinfo")
      .then(res => {
        if (res.data.code == 200) {
          this.gpu_rate = res.data.data.gpu_rate;
          this.meme_used_rate = res.data.data.meme_used_rate;
          this.memory_rate = res.data.data.memory_rate;
        }
      })
      .catch(err => {
        console.error(err);
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.head {
  border-bottom: 1px solid #ededed;
  width: 100%;
  height: 80px;
  z-index: 500;
  position: relative;
  background-color: #FFF;

  // background-color: #0E1E37;
  // box-shadow: 0px 3px 6px rgba(20, 90, 254, 0.04);
  box-sizing: border-box;
  // display: flex;
  // align-items: center;
  // justify-content: space-between;
  .left {
    display: flex;
    align-items: center;
    img {
      margin-right: 12px;
    }
    .span {
      font-size: 20px;
      font-family: Source Han Sans CN;
      font-weight: 400;
      color: #ffffff;
    }
  }
  .right {
    height: 100%;
    position: relative;
  }
}
.my {
  display: flex;
  align-items: center;
  height: 100%;
  position: absolute;
  right: 0px;
}
.my > li {
  height: 100%;
  display: flex;
  align-items: center;
  cursor: pointer;
}
.img {
  // width: 36px;
  height: 36px !important;
  // border-radius: 50%;
  overflow: hidden;
  margin-right: 30px;
  span {
    font-weight: bold;
    display: inline-block;
    margin: 0px 2px 0px 8px;
  }
}
.img > img {
  width: 36px;
  height: 36px;
}
.show {
  position: relative;
  font-size: 12px;
  font-family: Microsoft YaHei;
  font-weight: 400;

  color: #949494;
}
.sw {
  position: absolute;
  z-index: 10;
  width: 90px;
  height: 64px;
  border: 1px solid #e8eefc;
  box-shadow: 0px 3px 6px rgba(20, 90, 254, 0.16);
  box-sizing: border-box;
  padding: 11px 16px;
  top: 56px;
  background-color: white;
}
.sw > li {
  font-size: 14px;
  font-family: Source Han Sans CN;
  font-weight: 400;
  margin-bottom: 5px;
  color: #262a32;
  cursor: pointer;
}
.el-icon-caret-top,
.el-icon-caret-bottom {
  color: #949494;
}
</style>

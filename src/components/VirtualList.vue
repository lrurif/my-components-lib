<template>
    <div class="wrapper" ref="wrapper" @wheel="wheel">
      <div class="list" ref="list">
        <div class="item" v-for="(item, index) in curList" :key="index">
          {{item}}
        </div>
      </div>
    </div>
</template>

<script>
// 原理: 在鼠标进行滚动时，内部元素也进行滚动（利用transform）来实现，当滚动完一个元素的距离时，
// 需要进行切割原数组，并赋值给当前数组，并将translate属性置为0。（当前数组需要比页面所能容纳的数组长度+1）
export default {
    data() {
        return {
          totalList: [],
          curList: [],
          total_height: 0,
          scrollTop: 0,
          itemHeight: 0,
          maxSize: 0,
          canDraw: true
        };
    },
    methods: {
      getCurList(scrollTop = 0) {
        let min = Math.floor(scrollTop / this.itemHeight);
        let max = min + this.maxSize + 1;
        this.curList = this.totalList.slice(min, max)
        let offset = (this.itemHeight - (min * this.itemHeight - scrollTop))% this.itemHeight;
        this.$refs.list.style.transform = `translate(0, -${offset}px)`;
      },
      wheel() {
        if(!this.canDraw) return;
        this.canDraw = false;
        requestAnimationFrame(() => {
          this.canDraw = true;
        })
        this.scrollTop += event.deltaY;
        if(this.scrollTop < 0) {
          this.scrollTop = 0
        }
        if(this.scrollTop > this.itemHeight *(this.totalList.length - this.maxSize)) {
          this.scrollTop = this.itemHeight *(this.totalList.length - this.maxSize)
        }
        this.getCurList(this.scrollTop)
      }
    },
    created() {
      let i = 1;
      this.totalList = new Array(100).fill(0).map(item => i++)
      this.total_height = 100 * this.itemHeight;
      this.curList = this.totalList.slice(0, 1)
    },
    mounted() {
      this.itemHeight = parseInt(window.getComputedStyle(document.querySelector(".item")).height)
      this.maxSize = Math.floor(300 / this.itemHeight)
      this.getCurList(this.scrollTop)
    }
    }
</script>
<style lang='scss' scoped>
  .wrapper {
    height: 300px;
    overflow: hidden;
    position: relative;
    font-size: 16px;
    .list {
      position: absolute;
      top: 0;
      & > div {
        height: 50px;
      }
    }
  }
</style>
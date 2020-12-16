<template>
  <div ref="list" class="gj-list-container" @scroll="scrollEvent($event)">
    <div class="gj-list-phantom" :style="{ height: listHeight + 'px' }"></div>
    <div class="gj-list" :style="{ transform: getTransform }">
      <ArticleItem
        class="gj-list-item"
        v-for="item in visibleData"
        :key="item._id"
        :article="item"
        :style="{ height: size + 'px' }"
      ></ArticleItem>
    </div>
  </div>
</template>

<script>
import ArticleItem from "./ArticleItem.vue";
export default {
  components:{ArticleItem},
  props: {
    listData: {
      type: Array,
      default: () => []
    },
    size: {
      type: Number,
      default: 200
    }
  },
  computed: {
    listHeight() {
      return this.listData.length * this.size;
    },
      //偏移量对应的style
    getTransform(){
      return `translate3d(0,${this.startOffset}px,0)`;
    },
    visibleCount() {
      return Math.ceil(this.screenHeight / this.size);
    },
    visibleData() {
      return this.listData.slice(
        this.start,
        Math.min(this.end, this.listData.length)
      );
    }
  },
  data() {
    return {
      screenHeight: 800,
      startOffset: 0,
      start: 0, // 开始的索引
      end: 2 // 结束索引
    };
  },
  mounted(){ 
    this.end = this.start + this.visibleCount;
  },
  methods: {
    scrollEvent() {
      let scrollTop = this.$refs.list.scrollTop;
      this.start = Math.floor(scrollTop / this.size);
      this.end = this.start + this.visibleCount;
      this.startOffset = scrollTop - (scrollTop % this.size);
    }
  }
};
</script>

<style scoped>
.gj-list-container {
  height: 100%;
  overflow: auto;
  position: relative;
  -webkit-overflow-scrolling: touch;
}
.gj-list-phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}
.gj-list {
  left: 0;
  top: 0;
  right: 0;
  position: relative;
}
.gj-list-item {
  padding: 10px;
  color: #555;
  border-bottom: 1px solid #999;
}
</style>

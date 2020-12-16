<template>
  <div class="gj-container">
    <VirtualList :listData="articles"></VirtualList>
    <!-- <div v-for="(article,index) in articles" :key="index">
      {{article.title}}
    </div> -->
  </div>
</template>

<script>
import VirtualList from '~/components/VirtualList.vue'
export default {
  data() {
    return {
      articles: []
    }
  },
  components: {
    VirtualList,
  },
  async mounted () {
    let ret = await this.$http.get('/article')
    if(ret.code == 0){
      this.articles = ret.data
    }
  }
}
</script>

<style>
html{
  height: 100%;
}
body{
  height: 100%;
  margin:0;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

</style>

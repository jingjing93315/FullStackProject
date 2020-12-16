<template>
  <div>
    <div class="write-btn">
      <el-button @click="submit" type="primary">提交</el-button>
    </div>
    <el-row>
      <el-col :span="12">
        <textarea
          ref="editor"
          :value="content"
          @input="update"
          class="md-editor"
        ></textarea>
      </el-col>
      <el-col :span="12">
        <div class="markdown-body" v-html="compiledContent"></div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import marked from "marked";
import hljs from 'highlight.js'
import javascript from 'highlight.js/lib/languages/javascript'
import 'highlight.js/styles/monokai-sublime.css'
export default {
  data() {
    return {
      content: `# 一线蓝光
* 上课
* 吃饭
* 睡觉
* 写代码
\`\`\`javascript

    let a = 1
    console.log(a)
\`\`\`
                `
    };
  },
  mounted() {
    this.timer = null;

    this.bindEvnets();

    marked.setOptions({
        rendered: new marked.Renderer(),
        highlight(code){
           return hljs.highlightAuto(code).value
        }
    })
  },
  computed: {
    compiledContent() {
      let ret = marked(this.content, {});
      return ret;
    }
  },
  // lodash/debounce
  methods: {
    bindEvnets() {
      this.$refs.editor.addEventListener("paste", async e => {
          const files = e.clipboardData.files
          console.log(files)
          // todo 上传文件  
      });

      this.$refs.editor.addEventListener('drop', async e => {
          const files = e.dataTransfer.files
        // todo 文件上传
          e.preventDefault()

      })



      
    },
    async submit() {
      // 文章列表 点赞 关注 草稿
      // user -> article 一对多
      let ret = await this.$http.post('/article/create',{
        content: this.content, // selected: false
        compiledContent: this.compiledContent // 显示只读取这个
      })

    },
    update(e) {
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.content = e.target.value;
      }, 350);
    }
  }
};
</script>

<style scoped>
.md-editor {
  width: 100%;
  height: 100vh;
  outline: none;
}
.write-btn {
  position: fixed;
  z-index: 100;
  right: 30px;
  top: 10px;
}
</style>

<template>
  <div class='page'>
    <form action="" v-if='page'>
      <label for="title">标题</label><br>
      <input type="text" v-model='page.title' calss='title' name='title'><br>
      <label for="content">内容</label><br>
      <textarea type="text" v-model='page.content' class='content' name='content'></textarea>
      <button @click='deletePage()'>删除</button>
      <button @click='savePage()' @keyup.enter='savePage()'>保存</button>
    </form>
    <div v-else>
      <h1>&larr; 开始创建你的笔记吧!</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Page',
  props: ['page'],
  methods: {
    deletePage() {
      this.$emit('delete-page')
    },
    savePage() {
      this.$emit('save-page')
    }
  }
}
</script>

<style scoped>
  .page{
    width: 100%;
    padding: 2rem;
    box-shadow: 3rem 0 5rem 3rem #c1f5f5;
    text-align:left;
  }

  .content, .title {
    border-style: none;
    border-radius: 0.2rem;
    border: solid 1px lightgray;
    width: 100%;
    box-sizing: border-box;
    margin-bottom:1.25rem;
  }

  .content:focus, .title:focus {
    outline:0;
  }
  .content {
    font-family:Arial, Helvetica, sans-serif;
    resize: vertical;
    font-size: 1.5rem;
    padding: 0.5rem;
    height:20rem;
  }

  .title {
    font-size:2rem;
    padding: 0.5rem 1rem;
  }

  label {
    margin-bottom: .5rem;
    display: inline-block;
  }
  button {
    border-style: none;
    padding: .5rem .75rem;
    background-color: #44abc3;
    margin-right: 1rem;
    border-radius: .25rem;
    color: white;
    font-size: 1rem;
    cursor: pointer;
  }

  button:hover {
    background-color: #368ea2;
  }
</style>

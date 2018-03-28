<template>
  <div id="app">
    <notebook @change-page='changePage' @new-page='newPage' :pages='pages' :activePage='index'></notebook>
    <page @save-page='savePage' @delete-page='deletePage' :page='pages[index]'></page>
  </div>
</template>

<script>
import page from './components/notepage'
import notebook from './components/notebook'
import Wilddog from 'wilddog'

var config = {
  authDomain: 'wd7354779649nlrsqt.wilddog.com',
  syncURL: 'https://wd7354779649nlrsqt.wilddogio.com/'
}

var defApp  = Wilddog.initializeApp(config)
var ref = Wilddog.sync().ref()



export default {
  name: 'App',
  components: {
    notebook,
    page
  },
  data: ()=>({
    pages: [],
    index: 0,
  }),
  mounted(){
    ref.once('value', (pages)=> {
      pages.forEach((page)=> {
        this.pages.push({
          key: page.key(),
          title: page.child('title').val(),
          content: page.child('content').val()
        })
      })
    })
  },
  methods: {
    newPage() {
      this.pages.push({
        title:'',
        content: ''
      })
      this.index = this.pages.length - 1
    },
    changePage(index) {
      this.index = index
    },
    savePage() {
      var page = this.pages[this.index]
      if (page.key) {
        this.updatePage(page)
      } else {
        this.insertNewPage(page)
      }
    },
    updatePage(page) {
      var hopperRef = ref.child(page.key)
      hopperRef.update({
        'title': page.title,
        'content': page.content,
      })
    },
    insertNewPage(page) {
      ref.push({
        title: page.title,
        content: page.content,
      })
    },
    deletePage() {
      var target = this.pages[this.index].key
      ref.child(target).remove()
      this.pages.splice(this.index, 1)
      this.index = Math.max(this.index - 1, 0)
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: row;
}
html, body, #app {
  height:100%;
}
body {
  margin: 0;
}
</style>

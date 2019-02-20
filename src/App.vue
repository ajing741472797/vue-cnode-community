<template>
  <div id="app">
    <Header></Header>
    <div class="main">
    <router-view name="main"></router-view>
    <router-view name="slidebar"></router-view> 

    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import PostList from './components/PostList'
import Article from './components/Article'
import SlideBar from './components/SlideBar'
export default {
  name: 'App',
  components:{
    Header,
    PostList,
    Article,
    SlideBar
  }
}
</script>

<style scoped>

*{
  margin:0;
  padding:0;
}
 
.main{
  width:85%;
  margin: 0 auto;
  display: flex;
  flex-direction: row;
}
 #app{
  background-color: #e1e1e1;
  
}
</style>

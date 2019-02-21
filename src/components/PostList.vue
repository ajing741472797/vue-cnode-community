<template>
  <div class="PostList">
    <!-- 数据未返回时，显示这个正在加载的gif -->
    <div class="loading" v-if="isLoading">
      <img  src="../assets/loading.gif" alt="">
    </div>
    <!-- 代表我们的主题帖子列表 -->
    <div class="posts" v-else >
      <ul>
        <li>
        <div class="toobar">
          <span>全部</span>
          <span>精华</span>
          <span>分享</span>
          <span>问答</span>
          <span>招聘</span>
        </div>
      </li>
      <li v-for="post in posts" :key='post.num'> 
        <!-- 头像 -->
        <img :src="post.author.avatar_url" alt="">
        <!-- 回复浏览量 -->
        <span class="allcount">
          <span class="reply_count">{{post.reply_count}}</span>/
         {{post.visit_count}}
        </span>

        <!-- 帖子的分类 -->
        <span :class="[{put_good:(post.good  == true),put_top:(post.top  == true),'topiclist-tab':(post.good  !== true &&post.top  !== true)}]">
           {{post | tabFormatter}}
        </span>
        <!-- 标题 -->
        <router-link :to="{
          name:'post_content',
          params:{
            id:post.id,
            name:post.author.loginname
          }
          }">
          <span>
          {{post.title}}
        </span>
        </router-link>
        
        <!-- 最终回复时间 -->
        <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
      </li>
    <li>

      <pagination @handle="changePage"></pagination>
    </li>

    </ul>
    </div>
  </div>
</template>

<script> 
import pagination from './Pagination.vue'
export default {
  name: 'PostList',
  data () {
    return {
      isLoading:false,
      posts:[],
      pageData:1
    }
  },
  components:{
    pagination
  },
  methods:{
    getData:function(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
        page:this.pageData,
        limit:20
        }
        
      })
      .then((res)=>{
        this.isLoading = false
        this.posts = res.data.data
        // console.log(res.data.data)
      })
      .catch((err)=>{
        console.log(err)
      })
    },
    changePage:function(value){
      this.pageData = value
      this.getData()
    }
  },
  beforeMount:function(){
    this.isLoading = true //加载成功之前显示加载动画
    this.getData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
  margin:0;
  padding:0;
}
 .PostList{
    background-color: #e1e1e1;
    width: 100%;
    margin-top:15px;
    padding-bottom: 100px;
  }

  .PostList img {
    height: 30px;
    width: 30px;
    vertical-align: middle;
  }

  ul {
    list-style: none;
    width: 100%;
    max-width: 1344px;
    margin: 0 auto;

  }

  ul li:not(:first-child) {
    padding: 9px;
    font-size: 15px;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-weight: 400;
    background-color: white;
    color: #333;
    border-top: 1px solid #f0f0f0;
  }

  li:not(:first-child):hover {
    background: #f5f5f5;;
  }

  li:last-child:hover {
    background: white;
  }

  li span {
    line-height: 30px;
  }

  .allcount {
    width: 75px;
    display: inline-block;
    text-align: center;
    font-size: 12px;
  }

  .reply_count {
    color: #9e78c0;
    font-size: 14px;
  }

  .put_good, .put_top {
    background: #80bd01;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    color: #fff;
    font-size: 12px;
    margin-right: 10px;
  }

  .topiclist-tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }

  .last_reply {
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
    float: right;
    color: #778087;
    font-size: 12px;
  }

  .toobar {
    height: 40px;
    background-color: #f5f5f5;
  }

  .toobar span {
    font-size: 14px;
    color: #80bd01;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
  }

  .toobar span:hover {
    color: #9e78c0;
  }

  a {
    text-decoration: none;
    color: black;
  }

  a:hover {
    text-decoration: underline;
  }

  .loading {
    text-align: center;
    padding-top: 300px;
    
  }
</style>

<template>
  <div class="article">
    <!-- 数据未返回时，显示这个正在加载的gif -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" alt>
    </div>
    <div v-else>
      <div class="topic_header">
        <div class="topic_title">{{post.title}}</div>
        <ul>
          <li>• 发布于{{post.create_at | formatDate}}</li>
          <li>• 作者 : {{post.author.loginname}}</li>
          <li>• {{post.visit_count}}次浏览</li>
          <li>• 来自 {{post | tabFormatter}}</li>
        </ul>

        <div v-html="post.content" class="topic_content"></div>
      </div>

      <div id="reply">
         <div class="topbar">回复</div>

      <div v-for="(reply,index) in post.replies" :key="index" class="replySec">
        <div class="replyup">

          <router-link :to="{
          name:'user_info',
          params:{
            name:reply.author.loginname
          }}">
             <img :src="reply.author.avatar_url" alt>
          </router-link>
          
          <router-link :to="{
          name:'user_info',
          params:{
            name:reply.author.loginname
          }}">
          <span>{{reply.author.loginname}}</span>
          </router-link>

          <span>{{index+1}}楼</span> •
          <span>{{reply.create_at | formatDate}}</span>
          <span  class="ups" v-if="reply.ups.length>0">👍{{reply.ups.length}}</span>
          <span v-else></span>
          <p v-html="reply.content"></p>
        </div>
      </div>

      </div>
     
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data() {
    return {
      isLoading: false,
      post: {} //代表当前文章页的所有属性
    };
  },
  methods: {
    getArticleData: function() {
      this.$http
        .get("https://cnodejs.org/api/v1/topic/" + this.$route.params.id) // 字符串模板形式 `https://cnodejs.org/api/v1/topic/${this.$route.params.id}`
        .then(res => {
          if (res.data.success == true) {
            this.isLoading = false;
            console.log(res);
            this.post = res.data.data;
          }
        })
        .catch(err => {
          console.log(err);
        });
    },

  },
  beforeMount: function() {
    this.isLoading = true; //加载成功之前显示加载动画
    this.getArticleData();
  },
     watch:{ //检查路由发生变化
    '$route'(to,from){
      this.getArticleData()

    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
 
<style>
@import url("../assets/markdown-github.css"); /* 引入外部css需去掉scoped */
.article {
  background-color: #e1e1e1;
  position: relative;
  width: 72%;
  margin-left: 5%;
  margin-top: 15px;
  margin-bottom: 100px;
  max-width: 697px;
}
.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 16px;
  font-size: 12px;
  margin-top: 10px;
}

.article:not(:first-child) {
  margin-right: 340px;
  margin-top: 15px;
}

#reply,
.topic_header {
  background-color: #fff;
}

#reply {
  margin-top: 15px;
}

#reply img {
  width: 30px;
  height: 30px;
  position: relative;
  bottom: -9px;
}
#reply .ups{
  display: flex;
  justify-content:flex-end;
  margin-right: 15px;
  

}
.replyup p{
  max-width: 630px;
}
#reply a,
#reply span {
  font-size: 13px;
  color: #666;
  text-decoration: none;
}
.replySec {
  border-bottom: 1px solid #e5e5e5;
  padding: 0 10px;
}

.loading {
  text-align: center;
  padding-top: 300px;
}
.loading img{
    height: 30px;
    width: 30px;
    vertical-align: middle;
    position: absolute;
    left: 50%;
    right: 50%;
}

.replyUp a:nth-of-type(2) {
  margin-left: 0px;
  display: inline-block;
}

.topic_header {
  padding: 10px;
}

.topic_title {
  font-size: 20px;
  font-weight: bold;
  padding-top: 8px;
}

.topic_header ul {
  list-style: none;
  padding: 0px 0px;
  margin: 6px 0px;
}

.topic_header li {
  display: inline-block;
  font-size: 12px;
  color: #838383;
}

.topic_content {
  border-top: 1px solid #e5e5e5;
  padding: 0 10px;
  margin-top: 10px;
}

.markdown-text img {
  width: 92% !important;
  margin-right: -8%;
}
.markdown-text img {
  display: block;
}

</style>
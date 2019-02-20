<template>
  <div class="autherinfo">
      <div class="autherSummary">
        <div class="topbar">作者</div>
        <router-link :to="{
          name:'user_info',
          params:{
            name:userinfo.loginname
          }}">
          <img :src="userinfo.avatar_url" alt="">
          </router-link>

          <router-link :to="{
          name:'user_info',
          params:{
            name:userinfo.loginname
          }}">
          <span class="authorName">{{userinfo.loginname}}</span>  
          </router-link>
          
      </div>
      <div class="recent_topics">
        <div class="topbar">作者最近主题</div>
         <ul>
        <li v-for="item in topicLimit5" :key="item.num">
          <router-link :to="{
          name:'post_content',
          params:{
            id:item.id,
            name:item.author.loginname
          }
          }" v-if="item">
            {{item.title}}
          </router-link>
       
        </li>
      </ul>
      </div>
      <div class="recent_replies">
        <div class="topbar">作者最近回复</div>
        <ul>
        <li v-for="item in replyLimit5" :key="item.num">
          <router-link :to="{
          name:'post_content',
          params:{
            id:item.id,
            name:item.author.loginname
          }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
      </div>
  </div>
</template>

<script>
export default {
  name: 'SlideBar',
  data () {
    return {
       userinfo:{}
    }
  },
  computed:{
    topicLimit5:function(){
      if(this.userinfo.recent_topics){
        return this.userinfo.recent_topics.slice(0,5)//取数组的前5个
      }
    },
    replyLimit5:function(){
      if(this.userinfo.recent_replies){
        return this.userinfo.recent_replies.slice(0,5)
      }
    },
  },
  methods: {
    getData: function() {
      this.$http
        .get("https://cnodejs.org/api/v1/user/" + this.$route.params.name) // 字符串模板形式 `https://cnodejs.org/api/v1/topic/${this.$route.params.id}`
        .then(res => {
          if (res.data.success == true) {
            console.log(res);
            this.userinfo = res.data.data ;
          
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
 
  beforeMount: function() {
    this.isLoading = true; //加载成功之前显示加载动画
    this.getData();
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .autherSummary, .recent_replies, .recent_topics {
    background-color: #fff;
  }
  .autherSummary{
    position: relative;
  }
  .autherinfo {
    width: 299px;
    float: right;
    margin-top: 5px;
    margin-left: 10px;
  }
  .authorName{
    display: inline-block;
    position: absolute;
    top: 60px;
    left: 70px;
    color: #778087;
    font-size: 15px
  }
  li{
    padding: 3px 0 ;
  }
  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }

  ul a {
    font-size: 12px;
    text-decoration: none;
    color: #778087;
  }

  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    height: 16px;
    font-size: 12px;
    margin-top: 10px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin: 10px;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 22px;
    margin-right: 159px;
    font-size: 14px;
  }

  .loginname a {
    text-decoration: none;
    color: #778087;
  }

  
</style>

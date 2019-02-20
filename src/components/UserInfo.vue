<template>
  <div class="UserInfo">
      <div class="loading" v-if="isLoading">
    <img src="../assets/loading.gif" >
  </div>
<div class="userInfomation" v-else>
  <router-link :to="{
      name:'root',
      }">
  <p class="homepage">主页 <span class="line">/</span></p>
  </router-link>
  <section>
    <img :src="userinfo.avatar_url" alt="">
    <span class="authorName">{{userinfo.loginname}}</span>
      <p>
        {{userinfo.score}}积分
      </p>
      <p>
        注册时间：{{userinfo.create_at | formatDate}}
      </p>
  </section>
   <div class="replies" v-if="hasReply">
      <p>回复的主题</p>
      <ul>
        <li v-for="item in userinfo.recent_replies" :key="item.num">
          <router-link :to="{
          name:'post_content',
          params:{
            id:item.id
          }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="topics" v-if="hasTopic">
      <p>创建的主题</p>
      <ul>
        <li v-for="item in userinfo.recent_topics" :key="item.num">
          <router-link :to="{
          name:'post_content',
          params:{
            id:item.id
          }
          }" v-if="item">
            {{item.title}}
          </router-link>
       
        </li>
      </ul>
    </div>
</div>
  
  </div>
</template>

<script>
export default {
  name: 'UserInfo',
  data () {
    return {
            isLoading:false,//是否正在加载
            hasReply:true,
            hasTopic:true,
            userinfo:{}//代表当前文章页的所有内容，所有属性
    }
  },
    methods: {
    getData: function() {
      this.$http
        .get("https://cnodejs.org/api/v1/user/" + this.$route.params.name) // 字符串模板形式 `https://cnodejs.org/api/v1/topic/${this.$route.params.id}`
        .then(res => {
          if (res.data.success == true) {
            this.isLoading = false;
            console.log(res);
            this.userinfo = res.data.data ;
            if(this.userinfo.recent_replies.length<=0){
              return this.hasReply = false
            }
            if(this.userinfo.recent_topics.length<=0){
              return this.hasTopic = false
            }
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
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.UserInfo{
  width: 100%;
}
 
 .homepage {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
    color: #80bd01;
    font-size: 16px;
  }
.line{
  color: #ddd
}
.authorName{
  color:  #778087;
}
section>p{
  color:#333;
  font-size: 14px
}
 
 .userInfomation {
    background: white;
    width: 75%;
    margin: 10px auto;
  }
  .userInfomation section {
    padding: 12px;
  }
  .userInfomation img {
    width: 30px;
  }
  .userInfomation li {
    list-style:none;
  }
  .userInfomation .replies,
  .userInfomation .topics {
    font-size: 0.72rem;
    border-top: 10px #DDDDDD solid;
  }
  .userInfomation > div > p {
    padding: 12px 0 12px 12px;
    background-color: rgba(212, 205, 205, 0.17);
    font-size: 0.75rem;
    margin: 0;
  }
  .userInfomation > div >ul > li {
    padding: 4px 0 4px 12px;
    white-space: nowrap;
    font-size: 0.72rem;
    text-overflow: ellipsis;
    overflow: hidden;
    line-height: 30px;
    vertical-align: middle;
  }
  .userInfomation > div >ul > li > a {
    color: #094E99;
    text-decoration: none;
  }
  .router-link-active{
        text-decoration:none;
  }
</style>

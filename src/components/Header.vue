<template>
  <div class="header">
  <router-link :to="{
      name:'root',
      }">
  <img src="../assets/cnodejs_light.svg" alt="logo">
  </router-link>
    
    <ul>
     <router-link :to="{
      name:'root',
      }"> <li><a href="http://localhost:8080/#/">首页</a></li>  
      </router-link>
      
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">注册</a></li>
      <li><a href="#">登录</a></li>

    </ul>
  </div>
</template>

<script>
export default {
  name: 'Header',
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .header{
   background: #5a5555;
   height: 50px;
 }
img{
   max-width: 120px;
   margin-left: 50px;
   margin-top: 10px;
 }
 ul{
   list-style: none;
   float: right;
   margin-right:40px ;

 }
 li{
   display: inline-block;
   padding: 0 10px;
 }
 a{
    text-decoration: none;
    color: #CCC;
    font-size: 14px;
    text-shadow: none;
 }
</style>

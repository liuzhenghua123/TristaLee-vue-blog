<style scoped>
.detail-wrap{
  border-left: 3px solid #C6CA53;
  padding: 10px 20px 10px;
  overflow: auto;
}
.title{
  font-size: 20px;
  color: #CC6699;
  margin-left:0px;
  transition:all .2s ease-in 0s;
}
.title:hover{
  color: #669966;
  margin-left: 30px;
  transition:all .2s ease-in 0s;
}

.content{
  font-size: 14px;
  line-height: 1.5rem;
  margin-top: 15px;
  padding: 10px;
  /* display: inline-block; */
}

.recommend {
  margin-top: 25px;
}
.recommend h4{
  font-size: 18px;
  font-weight: 400;
}

.recommend ul li{
  line-height: 20px;
  font-size: 14px;
}
.edui-default p {
  font-size: 20px;

}
.edui-default pre{
  background-color: #ccc;

}
</style>

<style>
.ueditor-content pre{
    background-color: #ccc;
    word-wrap: break-word;
    white-space: pre-wrap;
    padding: 5px 20px; 
}

.ueditor-content div {
    background-color: #f5f5f5
}

.ueditor-content pre {
    word-wrap: break-word;
    white-space: pre-wrap
}

.ueditor-content span {
    /* font-family: Courier New!important; */
    /* font-size: 12px!important; */
    line-height: 1.5!important
}
.ueditor-content ol li {
    list-style-image: none
}

.ueditor-content ol li span {
    color: #000
}

</style>


<template>
  <div class="detail m-t">
    <div class="detail-wrap">
      <h1 class="title">{{detail.title}}</h1>
      <div class="content ueditor-content" v-html="detail.content">
        
      </div>
    </div>
    <div class="recommend">
      <h4>推荐阅读</h4>
      <ul class="m-t-sm">
        <li v-for="rec in recommends">
          <a v-bind:href="'/tristalee/detail?type='+type+'&id='+rec.id" target="_blank" >{{rec.title}}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

import axios from 'axios';
import qs from 'qs';

export default {
  name: 'detail',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      params:'',
      type:'',
      currId:'',
      detail:{
        // id:"1",
        // title:"文章一 就会发觉很多风景",
        // content:"hjhjdhfakjdhfjdfjdkshfjkfjhdsjkfhjkdshfjdhsfjhdsjkfhjdkshfjkdhjfdvbncxbvnbcnvbcnbvncbvncbxnvbxcnufii反馈倒海翻江大煞风景的是翻江倒海积分活动十分好的设计规范，活动时间发货的设计风华绝代舒服，风刀霜剑还翻江倒海时间发货的设计风，放假都开始放假的设计风看见的",
      },
      content_wrap:'',
      recommends:[
        // {
        //   id:2,
        //   title:"文章二 回家大煞风景"
        // },
        // {
        //   id:2,
        //   title:"文章二 回家大煞风景"
        // },
        // {
        //   id:2,
        //   title:"文章二 回家大煞风景"
        // },
        // {
        //   id:2,
        //   title:"文章二 回家大煞风景"
        // }
      ]
    }
  },
  mounted(){
    this.content_wrap = this.$refs.abc;
    console.log(this.$refs.abc);
  },
  created:function(){
    this.getURLParam();
    this.getDetail();
    this.getRecommend();
  },
  methods:{
    getURLParam: function(){
      this.param = window.location.search.split('?')[1].split('&');
      this.type = this.param[0].split('=')[1];
      this.currId = this.param[1].split('=')[1];
    },
    getDetail:function(){
      var self = this;
       axios.post('/api/tec.php',qs.stringify({
        type:this.type,
        id :this.currId,
        operate:"getdetail",
      }))
      .then(function (response) {
        if(response.data.status.code === 1){
          self.detail = response.data.data;
        }
       
      })
      .catch(function (error) {
        console.log(error);
      });
    },
    getRecommend:function(){
      var self = this;
       axios.post('/api/tec.php',qs.stringify({
        type:this.type,
        operate:"getrecommend",
      }))
      .then(function (response) {
        if(response.data.status.code === 1){
          self.recommends = response.data.data;
        }
       
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  }
}
</script>



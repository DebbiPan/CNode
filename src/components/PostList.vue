<template>
  <div>
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif" >
    </div>
    <div e-else class="posts">
      <ul>
        <div class="toobar">
          <span>全部</span>
          <span>精华</span>
          <span>分享</span>
          <span>问答</span>
          <span>招聘</span>
          <span>客户端测试</span>
        </div>
      </ul>
      <ul class="userShare">
        <li v-for="post in posts">
          <img :src="post.author.avatar_url" alt="">
          <span>
            <span class="allcount">
              <span class="reply_count">{{post.reply_count}}/</span>
              <span class="visit_count">{{post.visit_count}}</span>
            </span>
            <span :class="[{put_good:(post.good === true),put_top:(post.top === true),'topiclist_tab':(post.good !==true && post.top !== true)}]">
              <span class="put">{{post | tabFormatter}}</span>
            </span>
            <router-link :to="{name:'post_content',params:{id:post.id,name:post.author.loginname}}" class="a">
              <span class="title">{{post.title}}</span>
            </router-link>
            <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
          </span>
        </li>
        <li>
          <pagination @handleList="renderList"></pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import pagination from './Pagination'
export default {
  name: "PostList",
  data(){
    return{
      isLoading:false,
      posts:[],
      postPage:1
    }
  },
  components:{
    pagination
  },
  methods:{
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
          page:this.postPage,
          limit:20
        }
      })
        .then(res=>{
          this.isLoading = false
          this.posts = res.data.data
        })
      .catch((err=>{
        console.log(err);
      }))
    },
    renderList(value){
      this.postPage = value
      this.getData()
    }
  },
  beforeMount(){
    this.isLoading = true
    this.getData()
  }
}
</script>

<style scoped>
.posts{
  width:80%;
  margin-top:10px;
  background-color: #ffffff;
  display: flex;
  flex-direction: column;
}
ul{
   list-style:none;
   margin:0;
    padding:0;
 }
img{
  width:30px;
  height:30px;
}
.toobar{
  background-color:#f6f6f6;
  height: 40px;
  padding:0 10px;
}
.toobar span{
  color:#80bd01;
  cursor: pointer;
  font-size:14px;
  line-height:40px;
  padding:10px;
  cursor: pointer;
}
.toobar span:hover {
  background-color: #80bd01;
  color:#fff;
}
.userShare{
  padding-top:5px
}
.userShare li{
  border-bottom: 1px solid #888;
  line-height: 2em;
  padding:5px 10px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}
img{
  border-radius: 3px;
}
.a{
  text-decoration: none;
  color:#000;
}
.a:hover {
  text-decoration: underline;
}
.allcount {
  width: 70px;
  display: inline-block;
  text-align: center;
  font-size: 12px;
}
.reply_count {
  color: #9e78c0;
  font-size: 14px;
}
.put {
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
.last_reply{
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}
.loading {
  text-align: center;
  padding-top: 300px;
}
</style>

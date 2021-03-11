<template>
  <div class="authorinfo">
    <div class="authersummay">
      <div class="topbar">作者</div>
      <router-link :to="{name:'user_info',params:{
            name:userinfo.loginname
          }}" class="userInfo">
        <img :src="userinfo.avatar_url">
        <span class="authorname">{{userinfo.loginname}}</span>
      </router-link>
    </div>
    <div class="rent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
        <li v-for="list in topiclimit">
          <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname
            }
          }" class="a">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="recent_replices">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="list in reply_limit">
          <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname
            }
          }" class="a">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "SlideBar",
  data(){
    return{
      userinfo:{}
    }
  },
  methods:{
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false
          this.userinfo = res.data.data
          console.log(this.userinfo)
        })
        .catch((err) => {
          console.log(err);
        })
    }
  },
  beforeMount(){
    this.isLoading = true
    this.getData()
  },
  computed:{
    topiclimit(){
      if(this.userinfo.recent_topics){
        return this.userinfo.recent_topics.slice(0,5)
      }else{
        return this.userinfo.recent_topics
      }
    },
    reply_limit(){
      if(this.userinfo.recent_replies){
        return this.userinfo.recent_replies.slice(0,5)
      }else{
        return this.userinfo.recent_replies
      }
    }
  }
}
</script>

<style scoped>
.authorinfo{
  width:20%;
  float: right;
}
.authersummay{
  background:#ffffff;
}
.topbar{
  padding:10px 10px;
  background:#f6f6f6;
}
img{
  width:60px;
  height:60px;
  padding:10px;
}
.userInfo{
  text-decoration: none;
}
.authorname{
  position: relative;
  bottom:30px;
  color:#778087;
}
.rent_topics{
  background:#ffffff;
  margin-top:10px;
}
ul{
  list-style:none;
  padding:0 10px 10px 10px;
}
li{
  padding:3px 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color:#778087;
}
.a{
  text-decoration: none;
  color:#778087;
}
.recent_replices{
  background:#ffffff;
  margin-top:10px;
}
</style>

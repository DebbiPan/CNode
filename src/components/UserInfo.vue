<template>
<div class="UserInfo">
  <div class="loading" v-if="isLoading">
    <img src="../assets/loading.gif">
  </div>
  <div v-else class="userInfomation">
    <section>
      <div>主页</div>
      <img :src="userInfo.avatar_url">
      <span>{{userInfo.loginname}}</span>
      <p>{{userInfo.score}}积分</p>
      <p>注册时间：{{userInfo.create_at | formatDate}}</p>
    </section>
    <div class="topics">
      <p>创建的主题</p>
      <ul>
        <li v-for="item in userInfo.recent_topics" >
          <router-link :to="{
            name:'post_content',
            params:{
              id:item.id
            }
          }" class="a">
            <div>{{item.title}}</div>
          </router-link>
        </li>
      </ul>
    </div>
    <div class="replies">
      <p>回复的主题</p>
      <ul>
        <li v-for="item in userInfo.recent_replies">
          <router-link :to="{
            name:'post_content',
            params:{
              id:item.id
            },
          }" class="a">
            <div>{{item.title}}</div>
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "UserInfo",
  data(){
    return{
      isLoading:false,
      userInfo:{}
    }
  },
  methods: {
    getData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          this.isLoading = false
          this.userInfo = res.data.data
        })
        .catch((err) => {
          console.log(err);
        })
      }
    },
    beforeMount(){
      this.isLoading = true
      this.getData()
  }
}
</script>

<style scoped>
.userInfomation{
  margin:10px;
}
section{
  background:#ffffff;
  position: relative;
}
section div{
  border-bottom:1px solid #666;
  padding:10px 10px;
  background:#f6f6f6;
}
section img{
  width:100px;
  height: 100px;
  margin:10px;
}
section span{
  position: absolute;
  top:100px;
  left:150px;
  font-size:30px;
  font-weight: 700;
}
section p{
  padding:0 0 10px 10px;
}
.replies{
  background:#ffffff;
  padding-bottom: 10px;

}
.replies p{
  border-bottom:1px solid #666;
  padding:10px 10px;
  background:#f6f6f6;
}
.topics{
  background:#ffffff;
  padding-bottom: 10px;
}
.topics p{
  border-bottom:1px solid #666;
  padding:10px 10px;
  background:#f6f6f6;
}

.a{
  text-decoration: none;
  color:#0088cc;
}
.a:hover{
  color:#005580;
  text-decoration: underline;
}
ul{
  list-style:none;
}


</style>

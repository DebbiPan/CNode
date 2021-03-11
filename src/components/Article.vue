<template>
  <div class="article">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <div v-else class="allinfo">
      <div class="topic_header">
        <span>{{ post | tabFormatter }}</span>
        <span class="topic_title">{{ post.title }}</span>
        <ul>
          <li>发布于：{{ post.create_at | formatDate }}</li>
          <li>作者：{{ post.author.loginname }}</li>
          <li>{{ post.visit_count }}次浏览</li>
          <li>来自：{{ post | tabFormatter }}</li>
        </ul>
        <div v-html="post.content" class="topic_content"></div>
      </div>
      <div id="reply">
        <div class="topBar">回复</div>
        <div v-for="(reply,index) in post.replies" :key="index" class="replySec">
          <div class="replyUp">
            <router-link :to="{name:'user_info',params:{
            name:reply.author.loginname
          }}">
              <img :src="reply.author.avatar_url">
            </router-link>
            <span class="replyInfo">
                  <router-link :to="{name:'user_info',params:{
            name:reply.author.loginname
          }}" class="a">
                 <span class="replyname">{{ reply.author.loginname }}</span>
                  </router-link>
                  <span>第{{ index + 1 }}楼</span>
                  <span v-if="reply.ups.length>0">
                  赞 {{ reply.ups.length }}
                  </span>
                  <span v-else></span>
            </span>
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
      post: {}
    }
  },
  methods: {
    getArticleData() {
      this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then((res) => {
          if (res.data.success === true) {
            //

            this.post = res.data.data
            console.log(this.post)
            this.isLoading = false

          }
        })
        .catch((err) => {
          console.log(err);
        })
    }
  },
  beforeMount() {
    this.isLoading = true
    this.getArticleData()
  },
  watch:{
    '$route'(to , from){
      this.getArticleData()
    }
  }
}
</script>

<style >
@import url('../assets/markdown-github.css');

.topic_header {
  width: 75%;
  background-color: #ffffff;
  min-width: 600px;
  margin: 10px 0;
  padding: 30px 20px;
}

.topic_header > span:first-child {
  background: #80bd01;
  padding: 3px;
  border-radius: 3px;
  color: #fff;
  font-size: 12px;
}

.topic_title {
  font-size: 22px;
  font-weight: 700;
  margin: 8px 0 8px 8px;
}

.topic_header > ul {
  margin: 8px 0 8px 0;
  padding: 0;
}

.topic_header > ul > li {
  display: inline-block;
  padding-right: 10px;
  color: #838383;
  font-size: 10px
}

#reply {
  width: 75%;
  background-color: #ffffff;
  min-width: 600px;
  margin: 10px 0;
  padding: 1px 20px 40px 20px;
}

.topBar {
  background-color: #f6f6f6;
  padding: 5px 15px;
  margin: 20px 0;
  border-radius: 3px;
}

.replyUp {
  border: 1px solid transparent;
  position: relative;

}
.replyUp p{
  margin-left:25px;
  margin-top:30px;
}
.replyUp img {
  width: 30px;
  height: 30px;
  position: absolute;
  top: 5px;
  left: 5px;
}
.replyInfo{
  position: absolute;
  top: 5px;
  left: 45px;
}
.replyInfo>span{
  font-size: 12px;
}
.a{
  text-decoration: none;
}
.replyname {
  font-size: 12px;
  color: #666666;
  font-weight: 700;
}
  .markdown-text img {
  width: 92% !important;
}
</style>

<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="beforePage" class="pagebtn">...</button>
    <button v-for="page in pagebtns"
            @click="changeBtn(page)"
            :class="[{currentPage:page === currentPage},'pagebtn']">
      {{ page }}
    </button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: "pagination",
  data() {
    return {
      pagebtns: [1, 2, 3, 4, 5,  '...'],
      currentPage: 1,
      beforePage:false
    }
  },
  methods: {
    changeBtn(page) {
      if (typeof page !== 'number') {
        switch (page.target.innerText) {
          case '上一页':
            $('button.currentPage').prev().click()
            break
          case '下一页':
            $('button.currentPage').next().click()
            break
          case '首页':
            this.pagebtns = [1, 2, 3, 4, 5, '...']
            this.changeBtn(1)
            break
        }
        return
      }
      if (page > 4) {
        this.beforePage = true
      } else {
        this.beforePage = false
      }
      this.currentPage = page;
      if (page === this.pagebtns[4]) {
        this.pagebtns.shift();
        this.pagebtns.splice(4, 0, this.pagebtns[3] + 1)
      } else if (page === this.pagebtns[0] && page !== 1) {
        this.pagebtns.unshift(this.pagebtns[0] - 1)
        this.pagebtns.splice(5, 1)
      }
      this.$emit('handleList', this.currentPage)
    }
  }
}
</script>

<style scoped>
.currentPage {
  background-color: #80bd01;
  color: #fff;
}

.pagebtn {
  position: relative;
  bottom: 1px;
  width: 40px;
  margin: 0 4px;
}
</style>

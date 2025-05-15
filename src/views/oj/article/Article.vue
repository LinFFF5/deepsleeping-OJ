<template>
  <div>
    <NavBar :active="`/blog`"></NavBar>
    <div class="main">
      <el-row class="article">
        <el-col :span="24" class="title">{{ article.title }}</el-col>
        <el-col :span="24" class="msg">作者：{{ article.authorName }} &nbsp;&nbsp;
          发布时间：{{ article.time }}&nbsp;&nbsp;
          阅读量：{{ article.readNum }}
          <el-button type="danger" size="small" style="float: right"  v-if="user.username===article.authorName" @click="deleteArticle">删除</el-button>
          <el-button type="primary" size="small" style="float: right;margin-right: 10px;"  v-if="user.username===article.authorName" @click="goToModify">编辑</el-button>
        </el-col>
        <el-col :span="24" class="content" v-html="article.content">
        </el-col>

      </el-row>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>
import NavBar from "@/components/oj/common/NavBar.vue";
import Footer from "@/components/oj/common/Footer";
import axios from "axios";
import {ElMessage, ElMessageBox} from "element-plus";
import Cookies from "js-cookie";
import router from "@/router";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Article",
  data() {
    return {
      articleId: 0,
      article: {},
      user: {}
    };
  },
  components: {
    NavBar,
    Footer
  },
  mounted() {
    this.articleId = this.$route.params.id
    this.getArticle()
    if (Cookies.get('user')!==undefined && Cookies.get('user')!==null){
      this.user=JSON.parse(Cookies.get('user'));
      console.log(this.user)
    }
  },
  methods: {
    deleteArticle(){
      ElMessageBox.confirm(
          '确定删除吗?',
          'Warning',
          {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning',
          }
      ).then(()=>{
        axios.delete("/api/article/delete/"+this.articleId)
            .then(response=>{
              if (response.data.code===200){
                ElMessage({
                  message: '删除成功',
                  type: 'success',
                })
                router.push("/blog")
              }else{
                ElMessage({
                  message: '删除失败',
                  type: 'warning',
                })
              }
            })
            .catch(()=>{
              ElMessage({
                message: '删除失败',
                type: 'warning',
              })
            })
      })

    },

    goToModify(){
      router.push("/article/modify/"+this.articleId)
    },

    getArticle() {
      console.log(this.articleId)
      axios.get("/api/article/" + this.articleId)
          .then(response => {
            this.article = response.data.data
            console.log(this.article)
          })
          .catch(error => {
            ElMessage({
              message: '获取文章信息失败',
              type: 'warning',
            })
            console.log(error);
          })
    },
  }
}
</script>

<style scoped>
.main{
  background-color: #EFF3F5;
  margin: 0 auto;
  text-align: center;
}
.article {
  margin-top: 60px;
  width: 80%;
  background-color: #FFFFFF;
  margin-left: 140px;
  padding: 20px;
}
.title{
  float: left;
  text-align: left;
  font-weight: bold;
  font-size: 25px;
}
.msg{
  text-align: left;
  float: left;
  padding-top: 10px;
  font-size: 16px;
}
.content{
  text-align: left;
}
</style>

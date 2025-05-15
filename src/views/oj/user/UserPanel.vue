<template>
  <div class="main">
    <NavBar></NavBar>
    <div style="padding-top:7%;padding-bottom:10%;width:80%;margin-left: 10%;background-color: #FFFFFF;">
      <el-tabs
          class="demo-tabs"
          tab-position="left"
          :stretch="true"
          v-model="activeName"
          @tab-click="handleClick"
      >
        <el-tab-pane label="个人信息">
          <el-descriptions
              class="margin-top"
              :column="1"
              size="10px"
              border
          >
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <user/>
                  </el-icon>
                  用户名
                </div>
              </template>
              {{ user.username }}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <Male />
                  </el-icon>
                  性别
                </div>
              </template>
              {{user.sex}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <View />
                  </el-icon>
                  个人介绍
                </div>
              </template>
              {{user.introduction}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <Cellphone />
                  </el-icon>
                  电子邮箱
                </div>
              </template>
              {{user.email}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <Memo />
                  </el-icon>
                  提交次数
                </div>
              </template>
              {{user.submitNum}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <DataAnalysis />
                  </el-icon>
                  解题总数
                </div>
              </template>
              {{user.solvedNum}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <office-building/>
                  </el-icon>
                  学校
                </div>
              </template>
              {{user.school}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <Finished />
                  </el-icon>
                  注册时间
                </div>
              </template>
              {{user.registerTime}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <Refresh />
                  </el-icon>
                  上次登录时间
                </div>
              </template>
              {{user.accessTime}}
            </el-descriptions-item>
            <el-descriptions-item>
              <template #label>
                <div class="cell-item">
                  <el-icon class="iconStyle">
                    <PieChart />
                  </el-icon>
                  默认使用的语言
                </div>
              </template>
              {{user.language}}
            </el-descriptions-item>
          </el-descriptions>
        </el-tab-pane>
        <el-tab-pane label="修改密码" class="changePass">
          <el-form
              label-position="top"
              label-width="100px"
              style="max-width: 460px"
          >
            <el-form-item label="请输入旧密码">
              <el-input  type="password"  show-password v-model="password.oldpass" />
            </el-form-item>
            <el-form-item label="请输入新密码">
              <el-input  type="password"  show-password v-model="password.newpass1" />
            </el-form-item>
            <el-form-item label="确认密码">
              <el-input  type="password" v-model="password.newpass2" />
            </el-form-item>
            <el-button type="primary" @click="change">
              确认修改
            </el-button>
          </el-form>

        </el-tab-pane>
        <el-tab-pane label="个人资料设置" class="changePass">
          <el-form
              label-position="top"
              label-width="100px"
              style="max-width: 460px"
          >
            <el-form-item label="头像">
              <el-upload
                  class="avatar-uploader"
                  action="http://localhost:8080/cos/upload"
                  :show-file-list="false"
                  :on-success="handleAvatarSuccess"
                  :before-upload="beforeAvatarUpload"
              >
                <img w-full v-if="user.photo" :src="user.photo"   class="avatar" />
                <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
              </el-upload>
            </el-form-item>
            <el-form-item label="性别">
              <el-radio-group v-model="sex">
                <el-radio :label="1" size="large">男</el-radio>
                <el-radio :label="0" size="large">女</el-radio>
              </el-radio-group>
            </el-form-item>
            <el-form-item label="简介">
              <el-input  type="textarea" v-model="user.introduction" />
            </el-form-item>
            <el-form-item label="学校">
              <el-input  type="text" v-model="user.school" />
            </el-form-item>
            <el-form-item label="默认使用语言">
              <el-select v-model="user.language" class="m-2" placeholder="Select">
                <el-option
                    v-for="item in languages"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                />
              </el-select>
            </el-form-item>
            <el-button type="primary" @click="changeAccount">
              确认修改
            </el-button>
          </el-form>
        </el-tab-pane>
      </el-tabs>
    </div>

    <Footer></Footer>
  </div>
</template>

<script>
import NavBar from "@/components/oj/common/NavBar.vue";
import Footer from "@/components/oj/common/Footer";
import Cookies from "js-cookie";
import {ElMessage} from "element-plus";
import axios from "axios";
import qs from "qs";

export default {
  name: "UserPanel",
  data() {
    return {
      user: {},
      password:{
        oldpass:'',
        newpass1:"",
        newpass2:"",
      },
      language:"",
      languages:[
        {
          value: 'C',
          label: 'C',
        },
        {
          value: 'C++',
          label: 'C++',
        },
        {
          value: 'Java',
          label: 'Java',
        },
        {
          value: 'Python',
          label: 'Python',
        },
        {
          value: 'Golang',
          label: 'Golang',
        },
        {
          value: 'JavaScript',
          label: 'JavaScript',
        },
      ],
      sex:"",
    };
  },
  components: {
    NavBar,
    Footer
  },
  methods: {
    handleAvatarSuccess( response){
        this.user.photo = response.data.url
    },

    beforeAvatarUpload(rawFile) {
      console.log(rawFile.type)
      if (rawFile.type !== 'image/jpeg' && rawFile.type !== 'image/png') {
        ElMessage.error('Avatar picture must be JPG format!')
        return false
      } else if (rawFile.size / 1024 / 1024 > 2) {
        ElMessage.error('Avatar picture size can not exceed 2MB!')
        return false
      }
      return true
    },
    getUser() {
      const user = Cookies.get('user')
      this.user = JSON.parse(user)
      if (user!==null){
        axios.get("/api/user/"+this.user.id).then(response => {
          this.user=response.data.data
          if (this.user.sex==="男"){
            this.sex=1
          }else{
            this.sex=0
          }
        })
      }
      console.log(this.user)
    },
    changeAccount(){
      Cookies.set('user',JSON.stringify(this.user))
      axios.put("/api/user/modify",{
        id:this.user.id,
        sex:this.sex===1?"男":"女",
        introduction:this.user.introduction,
        school:this.user.school,
        language:this.user.language,
        photo:this.user.photo
      }).then(response => {
        if (response.data.code===200){
          ElMessage({
            message: '修改成功',
            type: 'success',
          })
          this.getUser()
        }else{
          ElMessage({
            message: response.data.msg,
            type: 'error',
          })
        }
      })


    },
    change(){
      if (this.password.oldpass===""){
        ElMessage({
          message: '请输入原密码',
          type: 'error',
        })
        return
      }
      if (this.password.newpass1!==this.password.newpass2){
        ElMessage({
          message: '两次输入的密码不一样',
          type: 'error',
        })
        return
      }
      let postData = qs.stringify({
        id:this.user.id,
        password: this.password.oldpass,
        newPassword:this.password.newpass1
      })
      axios.put("/api/user/cwd", postData)
          .then(response => {
            console.log(response.data)
            if (response.data.code === 200) {
              ElMessage({
                message: '密码修改成功',
                type: 'success',
              })
              Cookies.set('user', JSON.stringify(response.data.data),'7d')
            } else {
              ElMessage({
                message: response.data.msg,
                type: 'error',
              })
            }
          })
          .catch(error => {
            ElMessage({
              message: '修改失败',
              type: 'warning',
            })
            console.log(error);
          })
    }
  },
  mounted() {
    this.getUser()
  }
}
</script>

<style scoped>
  .avatar-uploader .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }

  .avatar-uploader .el-upload {
    border: 1px dashed var(--el-border-color);
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: var(--el-transition-duration-fast);
  }

  .avatar-uploader .el-upload:hover {
    border-color: var(--el-color-primary);
  }

  .el-icon.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    text-align: center;
  }
</style>

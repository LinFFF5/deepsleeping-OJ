<template>
  <div class="footer">
    <el-row>
      <el-col :span="8">
        <div class="logo">
          <router-link :to="{name:`main`}"><img src="@/assets/logo_footer.png"></router-link>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="link">
           JOJ Smart Man's Game&nbsp;© &nbsp;2025 &nbsp;<a href="https://cn.xauat.edu.cn/" target="_blank" class="author-link">DeepSleep</a>
        </div>
      </el-col>
      <el-col :span="8">
        <div class="link"></div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Footer",
  data() {
    return {
      activeIndex: "",
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    }
  },
  created() {
    this.activeIndex = this.active
  }
}
</script>

<style scoped>
.footer {
  width: 100%;
  height: 10%;
  background-color: #fff;
  margin-top: 30px;
  position: fixed;
  bottom: 0;
}

.el-col {
  border-radius: 0px;
  height: 100%;
}

.author-link {
  color: black;
}

.logo {
  height: 5%;
}

.logo img {
  position: relative;
  width: 15%;
  text-align: center;
}

.link {
  height: 100%;
  text-align: center;
  color: #000;
  font-size: 18px;
  margin-top: 5%;
}
</style>

<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
   <el-header>
     <div>
       <img src="../assets/img/home/byk_1.jpg" alt="">
       <span>电商后台管理系统</span> 
      </div>
     <el-button type="info" @click="loginOut">退出</el-button>
   </el-header>
   <!-- 页面主题区域 -->
   <el-container>
     <!-- 侧边栏 -->
    <el-aside :width="isColleapse ? '64px': '200px' ">
     <div class="toggle-bottom" @click="toggleColapse">||||</div>
      <!-- 侧边栏菜单区 -->
      <el-menu  
        background-color="#333744"
        text-color="#fff"
        active-text-color="#409EFF"
        :unique-opened='true'
        :collapse='isColleapse'
        :collapse-transition='false'
        :router="true"
        :default-active="activePath">
        <!-- 一级菜单 -->
      <el-submenu :index="item.id + ''" v-for="item in menuList" :key="item.id">
        <!-- 一级菜单模板区域 -->
        <template slot="title">
          <!-- 图标 -->
          <i :class="iconObj[item.id]"></i>
          <!-- 文本 -->
          <span>{{item.authName}}</span>
        </template>
        <!-- 二级菜单 --> 
          <el-menu-item 
            :index="'/' + subItem.path + ''" 
            v-for="(subItem, index) in item.children" 
            :key="index"
            @click="saveNavState('/' + subItem.path)">
            <!-- 图标 -->
            <i class="el-icon-menu"></i>
            <!-- 文本 -->
            <span>{{subItem.authName}}</span>
          </el-menu-item>

      </el-submenu>
    </el-menu>
    </el-aside>
    <!-- 右侧主体 -->
    <el-main>
      <router-view></router-view>
    </el-main>
  </el-container>

</el-container>

</template>

<script>
export default {
  name: '',
  data() {
    return {
      menuList: '',
      iconObj: {
        '125': 'iconfont icon-yonghu',
        '103': 'iconfont icon-zhangshangcaifuyemianshoujiban345',
        '101': 'iconfont icon-shangpinguanli',
        '102': 'iconfont icon-dingdanguanli',
        '145': 'iconfont icon-shujutongji'
      },
      // 是否折叠菜单
      isColleapse: false,
      // 被激活的链接
      activePath: ''
    }
  },
  created() {
    this.getMenuList(),
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    loginOut() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const {data: res} = await this.$http.get('menus')
      console.log(res)
      if(res.meta.status !== 200) {
        return this.$message.error(res.mata.msg)
      }
      this.menuList = res.data
    },
    // 点击按钮切换菜单的折叠和展开
    toggleColapse() {
      this.isColleapse = !this.isColleapse
    },
    // 保存链接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #eaedf1;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    > span {
      margin-left: 15px;
    }
  }
}
.el-header div img {
  height: 55px;
  width: 55px;
  border-radius: 50%;

}
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #eaedf1;
}
.home-container {
  height: 100%;
}
.iconfont {
  margin-right: 10px;
}
.toggle-bottom {
  background-color: #4A5064;
  text-align: center;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
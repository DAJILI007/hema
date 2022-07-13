<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/icon_f.ead0997c.png" alt="" />
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <!-- 侧边栏菜单区域 -->
        <div class="toggle-button" @click="toggleCollspse">|||</div>
        <el-menu
          background-color="#eb7e8c"
          text-color="#fff"
          active-text-color="#fff"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="$router.currentRoute.path"
        >
          <!-- 一级菜单 -->
          <el-submenu
            :index="item.id + ''"
            v-for="item in menulist"
            :key="item.id"
          >
            <!-- 一级菜单的模板区垣 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{ item.authName }}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
            >
              <!-- 图标 -->
              <i class="el-icon-menu"></i>
              <!-- 文本 -->
              <span>{{ subItem.authName }}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view> </router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      //左侧菜单目录
      menulist: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollapse: false
    }
  },
  created() {
    this.grtMenuList()
    //
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async grtMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    // 点击按钮，切换菜单的折叠与展开
    toggleCollspse() {
      this.isCollapse = !this.isCollapse
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  display: flex;
  justify-content: space-between;
  padding-left: 10;
  align-items: center;
  color: #fff;
  font-weight: 700;
  font-size: 20px;
  background: linear-gradient(90deg, #348ac7, #7474bf);
  > div {
    display: flex;
    align-items: center;
  }
  img {
    width: 90px;
    height: 90px;
  }
  span {
    margin-left: 5px;
  }
}
.el-aside {
  background-color: #eb7e8c;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #e3fafa;
}
.iconfont {
  margin-right: 10px;
  color: #fff;
}
.toggle-button {
  background-color: #7474bf;
  font-size: 10px;
  //行高
  line-height: 24px;
  color: #fff;
  //字体居中
  text-align: center;
  //字体边距
  letter-spacing: 0.2em;
  //鼠标手
  cursor: pointer;
}
</style>

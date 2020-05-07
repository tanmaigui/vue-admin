<template>
  <div id="nav-wrap">
    <el-menu
      default-active="1-4-1"
      class="el-menu-vertical-demo"
      @open="handleOpen"
      @close="handleClose"
      :collapse="isCollapse"
      background-color="transparent"
      text-color="#fff"
      active-text-color="#fff"
      router
    >
      <template v-for="(item,index) in routesList">
        <el-submenu :key="index" v-if="!item.hidden" :index="item.path">
          <template slot="title">
            <svg-icon :iconClass="item.meta.icon" :className="item.meta.name"/>
            <span slot="title">{{item.meta.name}}</span>
          </template>
            <el-menu-item
              v-for="(children,index) in item.children"
              :key="index"
              :index="children.path"
            >{{children.meta.name}}</el-menu-item>
        </el-submenu>
      </template>
    </el-menu>
    
  </div>
</template>
<script>
export default {
  name: "navMenv",
  data() {
    return {
      isCollapse: false,
      routesList: this.$router.options.routes //路由数组
    };
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    }
  },
  mounted() {
  }
};
</script>
<style scoped lang="scss">
#nav-wrap {
  position: fixed;
  top: 0;
  left: 0;
  width: $nanMenu;
  height: 100vh;
  background-color: #344a5f;
  svg {
    font-size: 20px;
    margin-right: 10px;
  }
}
// .el-menu-vertical-demo:not(.el-menu--collapse) {
//     width: 200px;
//     min-height: 400px;
// }
</style>
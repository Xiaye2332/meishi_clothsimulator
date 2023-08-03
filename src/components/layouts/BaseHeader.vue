<script lang="ts" setup>
import {toggleDark} from "~/composables";
import {ElMessage} from "element-plus";

function gotoMsdzls() {
  window.open('http://msdzls.cn', "_self")
}

const Send_Message = () => {
  ElMessage({
    type: 'info',
    message: `欢迎使用美食换装模拟器！如有问题可加群301119931，感谢爱在春风中在开发过程中提供的支持！`,
  })
}

</script>

<template>
  <el-menu class="el-menu-demo" mode="horizontal" :ellipsis="false">
    <el-menu-item @click=gotoMsdzls index="0"><img src="/logo.png" style="width: 180px">
    </el-menu-item>
    <div class="flex-grow"/>
    <el-menu-item @click="Send_Message" index="1">美食换装模拟器v1.0(By 夏夜浅酌)
    </el-menu-item>
    <el-menu-item index="2" @click="toggleDark()">
      <button
          class="border-none w-full bg-transparent cursor-pointer"
          style="height: var(--ep-menu-item-height)"
      >
        <i inline-flex i="dark:ep-moon ep-sunny"/>
      </button>
    </el-menu-item>
  </el-menu>

</template>
<style scoped>
</style>
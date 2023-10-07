<template>
  <template v-if="user">
    <van-cell title="当前用户" :value="user?.username"/>
    <van-cell title="修改信息" is-link to="/user/update"/>
    <van-cell title="我创建的队伍" is-link to="/user/team/create"/>
    <van-cell title="我加入的队伍" is-link to="/user/team/join"/>
  </template>
  <!--  <van-form @submit="onExit">-->
  <van-button round block type="danger" @click="onExit">退出登录</van-button>
  <!--  </van-form>-->
  <van-dialog v-model:show="DialogShow" title="请确认是否退出" show-cancel-button @confirm="doExit">
  </van-dialog>
</template>

<script setup lang="ts">
import {useRouter} from "vue-router";
import {onMounted, ref} from "vue";
import myAxios from "../plugins/myAxios";
import {Dialog, Toast} from "vant";
import {getCurrentUser} from "../services/user";
import MyAxios from "../plugins/myAxios";


const user = ref();

const router = useRouter();

const DialogShow = ref(false);

onMounted(async () => {
  user.value = await getCurrentUser();
});

const onExit = () => {
  DialogShow.value = true;
};

const doExit =  async () => {
  const res = await myAxios.post("user/logout");
  console.log(res, '用户退出');
  if (res.code === 0) {
    Toast.success('退出成功');
    // 跳转到之前的页面
    const redirectUrl = "user/login";//route.query?.redirect as string ?? '/';
    window.location.href = redirectUrl;
  } else {
    Toast.fail('退出失败');
  }
};

</script>

<style scoped>

</style>

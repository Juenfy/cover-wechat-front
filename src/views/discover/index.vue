<script setup>
import { onMounted } from "vue";
import { useAppStore } from "@/stores/app";
import { useRoute } from "vue-router";
const appStore = useAppStore();
const route = useRoute();
onMounted(() => {
  appStore.initHeader({ title: "发现", navbar: true, search: false });
  console.log(route.query);
});
</script>
<template>
  <main class="main">
    <van-cell-group>
      <van-cell :center="true" is-link to="/discover/moment">
        <template #title>
          <van-image height="1.5rem" width="1.5rem" src="/public/moment.png" />
          <div class="left-box" style="">
            <span>朋友圈</span>
            <span></span>
          </div>
        </template>
      </van-cell>
      <van-cell :center="true" is-link to="">
        <template #title>
          <van-image height="1.5rem" width="1.5rem" src="/public/scan-o.png" />
          <div class="left-box" style="">
            <span>扫一扫</span>
            <span></span>
          </div>
        </template>
      </van-cell>
    </van-cell-group>
  </main>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

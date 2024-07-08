<!--聊天设置页面-->
<script setup>
import { onMounted, reactive, ref, watch } from "vue";
import { useRouter } from "vue-router";
import { useAppStore } from "@/stores/app";
import { SearchChatRecord } from "@/enums/app";
import * as chatApi from "@/api/chat";
const router = useRouter();
const showClearChat = ref(false);
const appStore = useAppStore();
const props = defineProps({ show: Boolean, info: Object });
const chatInfo = ref({});
//调用父组件关闭弹窗
const emit = defineEmits(["hide"]);

const onSearchChatRecordClick = () => {
  appStore.setShowCommonSearch(true);
  appStore.initCommonSearch({
    action: SearchChatRecord,
    placeholder: "搜索",
  });
};

watch(
  () => props.info,
  (info) => {
    chatInfo.value = info;
  }
);

const updateChatInfo = (key, value) => {
  const data = {
    is_group: chatInfo.value.is_group,
    to_user: chatInfo.value.to_user,
    key: key,
    value: value,
  };
  chatApi.putUpdate(data).then((res) => {
    console.log(res);
  });
};

onMounted(() => {});
</script>

<template>
  <van-popup
    v-model:show="props.show"
    position="bottom"
    :style="{ height: '100%', width: '100%' }"
    duration="0.2"
  >
    <header>
      <van-nav-bar title="聊天详情" left-arrow @click-left="$emit('hide')" />
    </header>
    <main class="main">
      <van-cell-group :style="{ padding: '1rem 0' }">
        <van-grid
          :column-num="5"
          icon-size="3.5rem"
          :border="false"
          :square="true"
        >
          <van-grid-item
            :icon="item.avatar"
            :text="item.nickname"
            :to="'/friend/info?keywords=' + item.wechat"
            v-for="item in chatInfo.users"
            :key="item.id"
          />
          <van-grid-item icon="/add.png" text="邀请" to="" />
        </van-grid>
      </van-cell-group>
      <van-cell-group v-if="chatInfo.is_group == 1">
        <van-cell
          title="群聊名称"
          is-link
          clickable
          size="large"
          :value="chatInfo.group_name"
        />
        <van-cell
          title="群二维码"
          is-link
          clickable
          size="large"
          :value="chatInfo.group_name"
        />
        <van-cell
          title="群公告"
          is-link
          clickable
          size="large"
          :value="chatInfo.group.notice ? chatInfo.group.notice : '未设置'"
        />
        <van-cell
          title="备注"
          is-link
          clickable
          size="large"
          :value="chatInfo.group.name"
        />
      </van-cell-group>

      <van-cell-group>
        <van-cell
          title="查找聊天内容"
          is-link
          clickable
          @click="onSearchChatRecordClick"
          size="large"
        />
      </van-cell-group>
      <van-cell-group>
        <van-cell title="消息免打扰" size="large">
          <template #right-icon>
            <van-switch
              v-model="chatInfo.muted"
              @click="updateChatInfo('muted', chatInfo.muted)"
            />
          </template>
        </van-cell>
        <van-cell title="置顶聊天" size="large">
          <template #right-icon>
            <van-switch
              v-model="chatInfo.top"
              @click="updateChatInfo('top', chatInfo.muted)"
            />
          </template>
        </van-cell>
        <!-- <van-cell title="提醒" size="large">
          <template #right-icon>
            <van-switch v-model="chatSetting.remind" />
          </template>
        </van-cell> -->
      </van-cell-group>
      <van-cell-group v-if="chatInfo.is_group == 1">
        <van-cell
          title="我在本群的昵称"
          is-link
          clickable
          size="large"
          :value="chatInfo.group.nickname"
        />
        <van-cell title="显示群成员昵称" size="large">
          <template #right-icon>
            <van-switch
              v-model="chatInfo.display_nickname"
              @click="updateChatInfo('display_nickname', chatInfo.muted)"
            />
          </template>
        </van-cell>
      </van-cell-group>
      <van-cell-group>
        <van-cell title="设置当前聊天背景" is-link to="/chat" size="large" />
      </van-cell-group>
      <van-cell-group>
        <van-cell
          title="清除聊天记录"
          clickable
          size="large"
          @click="showClearChat = true"
        />
      </van-cell-group>
      <van-cell-group>
        <van-popup
          v-model:show="showClearChat"
          round
          position="bottom"
          :style="{ background: '#f9f9f9' }"
        >
          <span
            :style="{
              background: '#fff',
              display: 'block',
              textAlign: 'center',
              color: 'red',
              fontSize: 'var(--van-cell-large-title-font-size)',
              lineHeight: '4rem',
              marginBottom: '0.4rem',
            }"
            >清空聊天记录</span
          >
          <span
            @click="showClearChat = false"
            :style="{
              background: '#fff',
              display: 'block',
              textAlign: 'center',
              fontSize: 'var(--van-cell-large-title-font-size)',
              lineHeight: '4rem',
            }"
            >取消</span
          >
        </van-popup>
      </van-cell-group>
    </main>
  </van-popup>
</template>

<style lang="less" scoped>
.van-cell-group {
  margin-bottom: 0.5rem;
}
</style>
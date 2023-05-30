<template>
  <a-list item-layout="horizontal" :data-source="props.postList">
    <template #renderItem="{ item }">
      <a-list-item>
        <a-list-item-meta :description="truncateDescription(item.content)">
          <template #title>
            <div class="title-wrapper">
              <a>{{ item.title }}</a>
            </div>
          </template>
          <template #avatar>
            <!-- 这个标签下可以放置头像 -->
            <a-avatar :src="init" />
          </template>
        </a-list-item-meta>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import init from "../assets/init.png";
import { withDefaults, defineProps } from "vue";

interface Props {
  postList: any[];
}

const props = withDefaults(defineProps<Props>(), {
  postList: () => [],
});

const maxDescriptionLength = 100; // 最大显示长度为100个字符

const truncateDescription = (content: any) => {
  if (content.length <= maxDescriptionLength) {
    return content;
  } else {
    return content.substring(0, maxDescriptionLength) + "...";
  }
};
</script>

<style scoped>
.gege {
  width: 200px;
}
.title-wrapper {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
  font-size: 15px;
  font-weight: bold;
  color: black;
}
</style>

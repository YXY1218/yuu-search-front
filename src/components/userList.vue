<template>
  <a-list
    item-layout="horizontal"
    :grid="{ column: 3, gutter: 36, xs: 1, sm: 1, md: 3, lg: 3, xl: 3, xxl: 3 }"
    :data-source="props.userList"
  >
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card hoverable style="width: 240px">
          <template #cover>
            <img alt="example" :src="init" />
          </template>
          <a-card-meta :title="item.userName">
            <template #description>{{ item.userProfile }}</template>
          </a-card-meta>
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import init from "../assets/init.png";
import { withDefaults, defineProps } from "vue";

interface Props {
  userList: any[];
}

const props = withDefaults(defineProps<Props>(), {
  userList: () => [],
});
</script>

<style scoped>
.img {
  width: 200px;
}
</style>

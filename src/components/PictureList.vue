<template>
  <a-list
    item-layout="horizontal"
    :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 4, xl: 4, xxl: 3 }"
    :data-source="props.pictureList"
  >
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card hoverable>
          <template #cover>
            <div class="picture-con">
              <img style="width: 100%; height: 100%" :src="item.url" />
            </div>
          </template>
          <a-card-meta :title="item.title" />
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>

<script setup lang="ts">
import { withDefaults, defineProps } from "vue";

interface Props {
  pictureList: any[];
}

const props = withDefaults(defineProps<Props>(), {
  pictureList: () => [],
});
</script>

<style scoped>
.picture-con {
  height: 200px;
  overflow: hidden;
}
</style>

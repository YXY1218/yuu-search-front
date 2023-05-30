<!-- 主页面 -->
<template>
  <div class="index-page">
    <div class="search-container">
      <a-input-search
        v-model:value="searchText"
        placeholder="请输入搜索关键词"
        enter-button="搜索"
        size="large"
        class="rounded-input"
        @search="onSearch"
      />
      <!-- 这个是为了在页面直接显示返回的是什么{{ JSON.stringify(postList) }} -->
      <span class="search-icon">
        <a-icon type="search" />
      </span>
    </div>
    <div class="tab-container">
      <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
        <a-tab-pane key="post" tab="文章">
          <PostList :post-list="postList" />
        </a-tab-pane>
        <a-tab-pane key="picture" tab="图片">
          <PictureList :picture-list="pictureList" />
        </a-tab-pane>
        <a-tab-pane key="user" tab="用户">
          <UserList :user-list="userList" />
        </a-tab-pane>
      </a-tabs>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/userList.vue";
import PictureList from "@/components/PictureList.vue";
import { message } from "ant-design-vue";
const pictureList = ref([]);
const postList = ref([]);
const userList = ref([]);
const searchText = ref("");
const route = useRoute();
const router = useRouter();
const activeKey = route.params.category;
/**
 * 加载数据
 * @param params
 */
const loadDataOld = (params: any) => {
  const postQuery = {
    ...params,
    searchText: params.text,
  };
  myAxios.post("post/list/page/vo", postQuery).then((res: any) => {
    postList.value = res.records;
  });

  const userQuery = {
    ...params,
    userName: params.text,
  };
  myAxios.post("user/list/page/vo", userQuery).then((res: any) => {
    userList.value = res.records;
  });

  const pictureQuery = {
    ...params,
    searchText: params.text,
  };
  myAxios.post("picture/list/page/vo", pictureQuery).then((res: any) => {
    pictureList.value = res.records;
  });
};

/**
 * 加载单类数据
 * @param params
 */
const loadData = (params: any) => {
  const query = {
    ...params,
    searchText: params.text,
  };
  myAxios.post("search/all", query).then((res: any) => {
    postList.value = res.postList;
    userList.value = res.userList;
    pictureList.value = res.pictureList;
  });
};

const initSearchParams = {
  type: activeKey,
  text: "",
  pageSize: 10,
  pageNum: 1,
};

const searchParams = ref(initSearchParams);

//实现监听功能，当路由改变时改变searchParams
watchEffect(() => {
  searchParams.value = {
    ...initSearchParams,
    text: route.query.text,
    type: route.params.category,
  } as any;
  loadData(searchParams.value);
});

const onSearch = (value: string) => {
  router.push({
    query: {
      ...searchParams.value,
      text: value,
    },
  });
};

const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    //指定页面地址
    query: searchParams.value,
  });
};
</script>

<style scoped>
.index-page .search-container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 600px;
  margin: 0 auto;
  padding: 10px;
  background-color: #f5f5f5;
  border-radius: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px; /* 添加搜索框和标签卡之间的间距 */
}

.index-page .search-container .ant-input-search {
  width: 100%;
  border: none;
  background-color: transparent;
}

.index-page .search-container .ant-input-search .ant-input {
  border: none;
  background-color: transparent;
  box-shadow: none;
}

.index-page .search-container .search-icon {
  position: absolute;
  right: 10px;
  color: #999;
  cursor: pointer;
}

.index-page .search-container .search-icon:hover {
  color: #555;
}

.index-page .tab-container {
  margin-bottom: 20px; /* 添加标签卡和下方内容之间的间距 */
}

.index-page .ant-tabs-nav .ant-tabs-tab {
  padding: 10px 20px;
  background-color: #f5f5f5;
  border-radius: 10px 10px 0 0;
}

.index-page .ant-tabs-nav .ant-tabs-tab-active {
  background-color: #fff;
  border-bottom: none;
}

.index-page .ant-tabs-nav .ant-tabs-tab-active::after {
  border-bottom: 2px solid #1890ff;
}
</style>

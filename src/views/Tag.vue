<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length">
      <PostList :posts="postsWithTag" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<script>
import getPosts from "../composables/getPosts";
import { computed } from "@vue/runtime-core";
import { useRoute } from "vue-router";
import PostList from "../components/PostList.vue";
import Spinner from "../components/Spinner.vue";

export default {
  name: "Tag",
  components: {
    PostList,
    Spinner,
  },
  setup() {
    const { posts, error, load } = getPosts();
    load();

    const route = useRoute();

    const postsWithTag = computed(() => {
      return posts.value.filter((item) => item.tags.includes(route.params.tag));
    });

    return { posts, error, load, postsWithTag };
  },
};
</script>

<style></style>

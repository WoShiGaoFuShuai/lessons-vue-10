<template>
  <div class="tag">
    <div v-if="error">{{ error }}</div>
    <div v-if="posts.length" class="layout">
      <PostList :posts="postsWithTag" />
      <TagCloud :posts="posts" />
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
import TagCloud from "../components/TagCloud.vue";

export default {
  name: "Tag",
  components: {
    PostList,
    Spinner,
    TagCloud,
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

<style scoped>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}

.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
</style>

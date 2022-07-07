<template>
  <div class="home">
    <h1>Home</h1>
    <div v-if="error">
      {{ error }}
    </div>
    <div v-if="posts.length">
      <PostList v-if="showPosts" :posts="posts" />
    </div>
    <div v-else>
      <Spinner />
    </div>
    <button @click="showPosts = !showPosts">toggle posts</button>
    <button @click="posts.pop()">del a post</button>
  </div>
</template>

<script>
import PostList from "../components/PostList.vue";
import getPosts from "../composables/getPosts";
import Spinner from "../components/Spinner.vue";
import { ref } from "@vue/reactivity";

export default {
  name: "Home",
  components: {
    PostList,
    Spinner,
  },

  setup() {
    const { posts, error, load } = getPosts();
    load();

    const showPosts = ref(true);

    return { posts, showPosts, error };
  },
};
</script>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>

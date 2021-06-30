<template>
  <div>
    <h1>Current Post</h1>
    <h2>{{ post.title }}</h2>
    <p>{{ post.body }}</p>
    <div>
      <img v-bind:src="post.image" alt="" />
    </div>
    <a v-if="post.is_owner" v-bind:href="`/posts/${post.id}/edit`">Edit this post</a>
    <button v-if="post.is_owner" v-on:click="destroyPost()">Destroy post</button>
    <a href="/posts">Back to all posts</a>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    this.showPost();
  },
  methods: {
    showPost: function () {
      axios.get("/posts/" + this.$route.params.id).then((response) => {
        console.log("show posts", response.data);
        this.post = response.data;
      });
    },
    destroyPost: function () {
      console.log("destroy post", this.post);
      axios.delete("/posts/" + this.post.id).then((response) => {
        console.log("Destroy success", response.data);
        this.$parent.flashMessage = "Post has been deleted.";
        this.$router.push("/posts");
      });
    },
  },
};
</script>

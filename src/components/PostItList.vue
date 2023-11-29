<template>
  <div class="postit-list">
    <PostIt
      v-for="postIt in postItProp"
      :key="postIt.id"
      :id="postIt.id"
      :title="postIt.title"
      :imageUrl="postIt.imageUrl"
      :date="postIt.date"
      :time="postIt.time"
      :dueDate="postIt.dueDate"
      :dueTime="postIt.dueTime"
      :priority="postIt.priority"
      :content="postIt.content"
      :featured="postIt.featured"
      :labels="postIt.labels"
      @delete-postit="handleDeletePostIt"
    />
  </div>
</template>

<script>
import PostIt from "./PostIt.vue";
export default {
  components: {
    PostIt,
  },
  props: {
    postItProp: Array,
  },
  methods: {
    handleDeletePostIt(id) {
      this.$emit("delete-postit", id);
    },
  },
};
</script>

<style scoped>
.postit-list {
  display: grid;
  width: 100%;
  grid-template-columns: 1fr 1fr 1fr;
}
</style>

<template>
  <div class="postit-form">
    <form class="postit-form__form" @submit.prevent="createPostIt">
      <!-- Title Field -->
      <div class="postit-form__form-group">
        <label for="title">Title</label>
        <input type="text" id="title" v-model="postItData.title" required />
      </div>

      <!-- Image URL Field -->
      <div class="postit-form__form-group">
        <label for="image">Image</label>
        <input type="text" id="image" v-model="postItData.imageUrl" />
      </div>

      <!-- Date Field -->
      <div class="postit-form__form-group">
        <label for="date">Date</label>
        <input type="date" id="date" v-model="postItData.date" />
      </div>

      <!-- Time Field -->
      <div class="postit-form__form-group">
        <label for="time">Time</label>
        <input type="time" id="time" v-model="postItData.time" />
      </div>

      <!-- Priority Field -->
      <div class="postit-form__form-group">
        <label for="priority">Priority</label>
        <select id="priority" v-model="postItData.priority">
          <option value="low">Low</option>
          <option value="medium">Medium</option>
          <option value="high">High</option>
        </select>
      </div>

      <!-- Due Date Field -->
      <div class="postit-form__form-group">
        <label for="due-date">Due Date</label>
        <input type="date" id="due-date" v-model="postItData.dueDate" />
      </div>

      <!-- Content Field -->
      <div class="postit-form__form-group">
        <label for="content">Content</label>
        <textarea id="content" v-model="postItData.content" required></textarea>
      </div>

      <!-- Labels Field -->
      <div class="postit-form__form-group">
        <label for="labels">Labels</label>
        <input
          type="text"
          id="labels"
          v-model="postItData.labels"
          placeholder="Comma-separated values"
        />
      </div>

      <!-- Featured Field -->
      <div class="postit-form__form-group">
        <label for="featured">Featured</label>
        <input type="checkbox" id="featured" v-model="postItData.featured" />
      </div>

      <!-- Submit Button -->
      <div class="postit-form__form-group">
        <button type="submit" class="postit-form__submit">Add Post-It</button>
      </div>
    </form>
  </div>
</template>

<script>
import { uuid } from "vue-uuid";

export default {
  data() {
    return {
      postItData: {
        id: "",
        title: "",
        imageUrl: "",
        date: "",
        time: "",
        dueDate: "",
        dueTime: "",
        priority: "",
        content: "",
        labels: [],
        featured: false,
      },
    };
  },
  methods: {
    createPostIt() {
      // Check required fields are not empty
      if (
        !this.postItData.title ||
        !this.postItData.date ||
        !this.postItData.priority ||
        !this.postItData.content
      ) {
        alert("Rellena los campos obligatorios");
        return;
      }

      // Generate unique ID
      const uniqueId = uuid.v4();

      // Create postIt Object with trimmed labels
      let postIt = {
        ...this.postItData,
        id: uniqueId,
        labels: this.postItData.labels.split(",").map((label) => label.trim()),
      };

      // Emit event with the postIt object created
      this.$emit("add-postit", postIt);

      console.log(postIt);

      // Reset form for new entries
      this.resetForm();
    },

    generateId() {
      return uuid.v4();
    },

    resetForm() {
      // Restore postItData to initial state
      this.postItData = {
        id: "",
        title: "",
        imageUrl: "",
        date: "",
        time: "",
        dueDate: "",
        dueTime: "",
        priority: "",
        content: "",
        labels: [],
        featured: false,
      };
    },
  },
};
</script>

<style scoped>
.postit-form__form {
  display: flex;
  flex-wrap: wrap;
}
.postit-form__form-group {
  margin-bottom: 20px;
  width: 50%;
  display: flex;
  flex-direction: column;
}
.postit-form__form-group label {
  display: block;
  margin-bottom: 10px;
  text-align: left;
  font-size: 14px;
}
.postit-form__form-group input,
.postit-form__form-group textarea,
.postit-form__form-group select {
  width: 80%;
  padding: 10px;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  outline: none;
  text-align: left;
}
.postit-form__form-group input:focus,
.postit-form__form-group textarea:focus {
  border-color: #000;
}
.postit-form__form-group textarea {
  height: 100px;
}
.postit-form__submit {
  padding: 10px;
  border: 1px solid #e5e5e5;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  min-width: 100px;
  background-color: #4caf50;
  color: #fff;
}
.postit-form__submit:hover {
  background-color: #43a047;
}
</style>

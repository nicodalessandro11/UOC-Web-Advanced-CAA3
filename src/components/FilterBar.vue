<template>
  <!-- Filter bar for sorting and ordering -->
  <div class="filter-bar">
    <!-- Dropdown to select sorting criteria -->
    <div class="filter-bar__select">
      <label for="sort-by">Sort by</label>
      <select id="sort-by" v-model="sortBy" @change="SortBy">
        <option value="date">Date</option>
        <option value="title">Title</option>
        <option value="priority">Priority</option>
      </select>
    </div>

    <!-- Dropdown to select sort order (ascending or descending) -->
    <div class="filter-bar__select">
      <label for="order-by">Order by</label>
      <select id="order-by" v-model="orderBy" @change="OrderBy">
        <option value="ascending">Ascending</option>
        <option value="descending">Descending</option>
      </select>
    </div>

    <!-- Checkbox to filter for favorite items -->
    <div class="filter-bar__check">
      <label for="favorite">Favorite</label>
      <input
        type="checkbox"
        id="favorite"
        v-model="favouriteSelected"
        @change="favourite"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sortBy: "date",
      orderBy: "ascending",
      favouriteSelected: false,
    };
  },
  methods: {
    SortBy() {
      this.$emit("sort-items", this.sortBy);
    },
    OrderBy() {
      this.$emit("order-items", this.orderBy);
    },
    favourite() {
      this.$emit("favourite-items", this.favouriteSelected);
    },
  },
};
</script>

<style scoped>
.filter-bar {
  display: flex;
  align-items: center;
  padding: 20px;
  background-color: #f5f5f5;
  border-bottom: 1px solid #ccc;
}
.filter-bar__select {
  display: flex;
  align-items: center;
  margin-right: 20px;
}
.filter-bar__select label {
  margin-right: 10px;
}
.filter-bar__select select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}
.filter-bar__check {
  display: flex;
  align-items: center;
}
.filter-bar__check input {
  margin-right: 10px;
}
.filter-bar__check label {
  cursor: pointer;
}
</style>

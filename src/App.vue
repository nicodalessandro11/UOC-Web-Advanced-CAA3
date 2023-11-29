<template>
  <div class="main">
    <!-- Header contains logo and application title -->
    <header class="header">
      <div class="header__left">
        <img src="./assets/uoc-logo.png" class="logo" alt="UOC Logo" />
        <span class="title">Post It Manager</span>
      </div>
    </header>
    <!-- Main content -->
    <div class="content">
      <!-- Components for search and filter functionality -->
      <SearchBar @show-form="toggleForm" @search="setSearchTerm" />
      <FilterBar
        @sort-items="setSortBy"
        @order-items="setOrderBy"
        @favourite-items="setFavouriteItems"
      />
      <!-- List display of Post-Its -->
      <main class="main">
        <PostItList
          :postItProp="postItListFiltered"
          @delete-postit="deletePostIt"
        />
      </main>
      <!-- Modal for adding a new Post-It, shown based on a boolean value -->
      <ModalLayer v-if="showModal" @close-modal="toggleForm">
        <!-- Slots for inserting custom header and body content into the modal -->
        <template v-slot:header> Add New Post-It </template>
        <template v-slot:body>
          <PostItForm @add-postit="addPostIt" @submit="toggleForm" />
        </template>
      </ModalLayer>
    </div>
  </div>
</template>

<script>
import FilterBar from "./components/FilterBar.vue";
import SearchBar from "./components/SearchBar.vue";
import PostItList from "./components/PostItList.vue";
import PostItForm from "./components/PostItForm.vue";
import ModalLayer from "./components/ModalLayer.vue";

export default {
  components: {
    SearchBar,
    FilterBar,
    PostItList,
    PostItForm,
    ModalLayer,
  },
  data() {
    return {
      showModal: false,
      searchTerm: "",
      sortBy: "",
      orderBy: "",
      favouriteItems: false,
      postItList: [
        {
          id: "1",
          title: "Important Meeting",
          imageUrl:
            "https://img.freepik.com/fotos-kostenlos/unzufriedener-kandidat-der-waehrend-des-vorstellungsgespraechs-im-buero-mit-der-personalabteilung-ueber-seinen-lebenslauf-diskutiert_637285-6945.jpg?w=1060&t=st=1699485108~exp=1699485708~hmac=e10579d0232161f34eb3d6df3b98e2aaacd5bf0e7d96b62d5371bff53fcadcec",
          date: "2023-10-30",
          time: "09:00 AM",
          dueDate: "2023-11-15",
          dueTime: "00:00",
          priority: "high",
          content: "Prepare for the quarterly review meeting with the team.",
          labels: ["Work", "Meeting", "Urgent"],
          featured: true,
        },
        {
          id: "2",
          title: "Grocery List",
          imageUrl:
            "https://img.freepik.com/fotos-kostenlos/essensplanung-und-verpflegung_23-2149099843.jpg?w=1060&t=st=1699484904~exp=1699485504~hmac=4a87f960d8a8fa0a6cfff72af274f8dc34d9e5deecc2a7805d166983e9f4d10fF",
          date: "2023-10-30",
          time: "10:00 AM",
          dueDate: "2023-10-31",
          dueTime: "00:00",
          priority: "medium",
          content:
            "Buy groceries for the week: milk, eggs, bread, and vegetables.",
          labels: ["Personal", "Groceries"],
          featured: true,
        },
        {
          id: "3",
          title: "Project Deadline",
          imageUrl:
            "https://img.freepik.com/fotos-premium/arbeit-von-zu-hause-aus-junger-freiberufler-oder-geschaeftsmann-der-zu-hause-mit-smartphone-tablet-arbeitet-arbeiten_24901-2306.jpg",
          date: "2023-10-31",
          time: "02:00 PM",
          dueDate: "2023-11-10",
          dueTime: "00:00",
          priority: "high",
          content:
            "Finish the project report and submit it before the deadline.",
          labels: ["Work", "Project", "Urgent"],
          featured: false,
        },
      ],
    };
  },
  methods: {
    addPostIt(postIt) {
      this.postItList.push(postIt);
    },
    deletePostIt(postItId) {
      this.postItList = this.postItList.filter(
        (postIt) => postIt.id !== postItId
      );
    },
    toggleForm() {
      this.showModal = !this.showModal;
    },
    setSearchTerm(term) {
      this.searchTerm = term;
    },
    setSortBy(criteria) {
      this.sortBy = criteria;
    },
    setOrderBy(order) {
      this.orderBy = order;
    },
    setFavouriteItems(favourite) {
      this.favouriteItems = favourite;
    },
  },
  computed: {
    postItListFiltered() {
      let filtered = [...this.postItList];

      if (this.searchTerm) {
        const lowerCaseSearchTerm = this.searchTerm.toLowerCase();
        filtered = filtered.filter(
          (postIt) =>
            postIt.title.toLowerCase().includes(lowerCaseSearchTerm) ||
            postIt.content.toLowerCase().includes(lowerCaseSearchTerm) ||
            postIt.labels.some((label) =>
              label.toLowerCase().includes(lowerCaseSearchTerm)
            )
        );
      }

      if (this.favouriteItems) {
        filtered = filtered.filter((postIt) => postIt.featured);
      }

      if (this.sortBy) {
        filtered.sort((a, b) => {
          let comparison = 0;
          if (a[this.sortBy] < b[this.sortBy]) {
            comparison = -1;
          } else if (a[this.sortBy] > b[this.sortBy]) {
            comparison = 1;
          }
          return this.orderBy === "ascending" ? comparison : comparison * -1;
        });
      }

      return filtered;
    },
  },
};
</script>

<style scoped>
body {
  padding: 0;
  margin: 0;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  background-color: #f5f5f5;
  border-bottom: 1px solid #e5e5e5;
}
.header__left {
  display: flex;
  align-items: center;
}
.logo {
  height: 40px;
  margin-right: 10px;
}
.title {
  font-size: 24px;
  font-weight: 400;
}
</style>

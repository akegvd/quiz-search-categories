<template>
  <div id="app">
    <input v-model="keywords" type="text" placeholder="Search keywords..." />
    <ul v-if="!isLoading">
      <li v-for="category in filteredCategories" :key="category">
        {{ category }}
      </li>
    </ul>
    <p v-else>Loading...</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isLoading: false,
      keywords: "",
      categories: [],
    };
  },
  computed: {
    filteredCategories() {
      return this.keywords === ""
        ? this.categories
        : this.categories.filter(
            (category) => category.search(this.keywords) !== -1
          );
    },
  },
  async mounted() {
    this.isLoading = true;

    const res = await fetch("https://api.publicapis.org/categories");

    this.categories = (await res.json())?.categories;
    this.isLoading = false;
  },
};
</script>

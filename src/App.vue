<template>
  
  <h1>VENDRE</h1>
  
  <div class="persons">
    <ul v-for="item in list" :key="item.id">
      <img :src="item.avatar" alt="pic">
      <li>{{ item.first_name }} {{ item.last_name }}</li>
      <a :href="item.email">Contact</a>
    </ul>
  </div>

  <div class="buttons">
    <button @click="previousPage">1</button>
    
    <button @click="nextPage">2</button>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      list: [],
      currentPage: 1,
      pageSize: 10,
      totalResults: 0
    };
  },
  async mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        let response = await axios.get(
          `https://reqres.in/api/users?page=${this.currentPage}`
        );
        this.list = response.data.data;
        this.totalResults = response.data.total;
      } catch (error) {
        console.error(error);
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.fetchData();
      }
    },
    nextPage() {
      const maxPage = Math.ceil(this.totalResults / this.pageSize);
      if (this.currentPage < maxPage) {
        this.currentPage++;
        this.fetchData();
      }
    }
  }
};
</script>




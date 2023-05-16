<template>
  <div>
    <input
      type="text"
      v-model="searchTerm"
      placeholder="Enter search term"
      class="search-input"
    />
    <button @click="search" class="search-button">Search</button>
    <p></p>
    <p v-if="invalidInput" style="color: red">Please enter a search term</p>
    <div v-if="results.length > 0">
      <div
        v-for="(person, index) in results"
        :key="person.id"
        :class="{ 'bg-alternate': index % 2 === 0 }"
        class="person-details"
      >
        <p><b>Name: </b>{{ person.first_name }} {{ person.last_name }}</p>
        <p><b>Email: </b>{{ person.email }}</p>
        <p><b>Gender: </b>{{ person.gender }}</p>
      </div>
      <p class="copyright">© Lisbon Lions 1967 😉</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import "./styles.css";

export default {
  data() {
    return {
      searchTerm: "",
      results: [],
      invalidInput: false,
    };
  },
  methods: {
    async search() {
      this.results = []; // Clear previous results

      if (this.searchTerm.trim() === "") {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;
      const response = await axios.get(
        `http://localhost:5288/api/Search?searchTerm=${this.searchTerm}`
      );
      this.results = response.data;
    },
  },
};
</script>

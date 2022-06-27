<script>
export default {
  name: "SchoolSearch",
  data() {
    return {
      searchInput: ''
    }
  },
  emits: ["search-started", "school-results"],
  methods: {
    search() {
      this.$emit('search-started')
      this.fetchData()
    },
    async fetchData() {
      const apiKey = import.meta.env.VITE_COLLEGE_API_KEY
      const url = `https://api.data.gov/ed/collegescorecard/v1/schools.json?api_key=${apiKey}&fields=id,school.name,location&per_page=100&school.name=${this.searchInput}`
      this.searchInput = ''
      const res = await fetch(url)
      const jsonResponse = await res.json()
      this.schools = jsonResponse["results"].map(school => {
        return {
          ...school,
          name: school["school.name"],
          position: {
            lat: school["location.lat"],
            lng: school["location.lon"]
          }
        }
      })
      this.$emit('school-results', this.schools)
    }
  }
}
</script>

<template>
  <input v-model="searchInput" @keyup.enter="search" placeholder="Search by College Name"/>
  <button @click="search">Search</button>
</template>

<style scoped>
input {
  width: 200px;
}

input:focus {
  outline-width: 0;
}
</style>

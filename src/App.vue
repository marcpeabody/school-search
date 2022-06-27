<script>
import SchoolSearch from './components/SchoolSearch.vue'
import SchoolList from './components/SchoolList.vue'

export default {
  components: { SchoolSearch, SchoolList },
  data() {
    return {
      searching: false,
      schools: null,
      selectedSchool: null
    }
  },
  methods: {
    searchStarted() {
      this.searching = true
    },
    schoolResults(schools) {
      this.schools = schools
      this.selectedSchool = this.schools[0]
      this.searching = false
    },
    schoolSelected(school) {
      this.selectedSchool = school
    }
  },
  computed: {
    center() {
      return this.selectedSchool?.position
    }
  }
}
</script>

<template>
  <header>
    <span id="collegeSearchLogo">College Search</span>
    <SchoolSearch v-on:search-started="searchStarted" v-on:school-results="schoolResults"></SchoolSearch>
  </header>

  <div class="prompt" v-if="searching">Searching...</div>
  <main v-else-if="selectedSchool">
    <SchoolList :schools="schools" :selectedSchool="selectedSchool" v-on:school-selected="schoolSelected"></SchoolList>
    <GMapMap :center="center" :zoom="7" map-type-id="terrain" style="width: 900px; height: 600px">
      <GMapMarker v-for="school in schools" :key="school.id" :position="school.position" @click="schoolSelected(school)"
        :clickable="true">
        <GMapInfoWindow :opened="selectedSchool.id === school.id">
          <div class="infoWindow">{{school.name}}</div>
        </GMapInfoWindow>
      </GMapMarker>
    </GMapMap>
  </main>
  <div class="prompt" v-else>Search for a College</div>
</template>

<style>

body {
  margin: 0;
}

#app {
  margin: 0 auto;
  width: 100%;
  font-weight: normal;

  display: flex;
  flex-direction: column;
  height: 100vh;
}

header {
  width: 100%;
  line-height: 1.5;
  text-align: center;
  padding: 8px;
  background-image: linear-gradient(to right top, #2f9e4b, #49b246, #66c53d, #86d92f, #a8eb12);;
  color: white;

  font-weight: 700;
  font-size: xx-large;
  text-shadow: 0 1px 0 #ccc,
      0 3px 5px rgba(0, 0, 0, .2),
      0 5px 10px rgba(0, 0, 0, .25),
      0 10px 10px rgba(0, 0, 0, .2),
      0 20px 20px rgba(0, 0, 0, .15);
}

#collegeSearchLogo {
  padding-right: 30px;
}

.infoWindow {
  color: hsla(160, 100%, 37%, 1);
  font-weight: bold;
}

@media (min-width: 1024px) {
  main {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
    text-align: center;
  }

  div.prompt {
    display: block;
    text-align: center;
    flex: 1;
    padding-top: 200px;
  }
}
</style>

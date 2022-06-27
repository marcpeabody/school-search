<script>
import { nextTick } from 'vue'

export default {
  name: "SchoolList",
  props: {
    schools: {
      type: Array,
      required: false
    },
    selectedSchool: {
      type: Object,
      required: false
    }
  },
  emits: ["school-selected"],
  methods: {
    focusSchool(school) {
      this.$emit('school-selected', school)
    }
  },
  watch: {
    async selectedSchool() {
      await nextTick()

      const active = this.$refs.list.getElementsByClassName('active')[0]
      active.scrollIntoView()
    }
  }
}
</script>

<template>
  <ul ref="list">
    <li v-for="school in schools" :key="school.id" :class="{ active: school.id == selectedSchool?.id }"
      @click="focusSchool(school)">{{ school.name }}</li>
  </ul>
</template>

<style scoped>
ul {
  height: 600px;
  overflow-y: scroll;
  padding: 0px;
  margin: 0px;
}

li {
  list-style-type: none;
  border: 1px solid grey;
  cursor: pointer;
  margin: 4px;
  padding-left: 8px;
  font-weight: bold;
}

li.active {
  background-color: hsla(160, 100%, 37%, 0.2);
  border: 1px solid rgb(59, 92, 10);
}

@media (hover: hover) {
  li:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
    transition: 0.2s;
  }
}
</style>

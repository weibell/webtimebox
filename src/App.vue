<template>
  <div id="app">
    <Timebox v-if="startTimestamp && endTimestamp" :start="startTimestamp" :end="endTimestamp"/>
    <div v-else>Error</div>
  </div>
</template>

<script>
  import Timebox from "@/components/Timebox";

  export default {
  name: 'App',
  components: {
    Timebox
  },

  computed: {
    startTimestamp() {
      return this.timestampsFromHash()[0]
    },

    endTimestamp() {
      return this.timestampsFromHash()[1]
    }
  },

  methods: {
    timestampsFromHash() {
      try {
        const [start, end] = location.hash.slice(1).split('-').map(this.toTimestamp)
        return [start, end]
      } catch (e) {
        return undefined
      }
    },

    toTimestamp(timeAsString) {
      const [hours, minutes] = timeAsString.split(':').map(Number)
      return new Date().setHours(hours, minutes, 0, 0)
    }
  }
}
</script>

<style>
  html {
    background: #eee;
  }
</style>

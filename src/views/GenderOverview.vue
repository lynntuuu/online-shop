<template>
  <div>
    <h1 class="wrapper">{{ pageTitle }}</h1>
  </div>
</template>

<script>
export default {
  name: 'genderOverview',
  computed: {
    gender () {
      return this.$route.params.gender
    },
    pageTitle () {
      return `${this.gender[0].toUpperCase()}${this.gender.slice(1)}`
    }
  }
}
</script>

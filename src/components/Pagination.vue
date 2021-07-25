<template>
  <div class="_pagination-wrapper">
    <span v-if="showPreviousLink" class="_pagination-btn" @click="updatePage(currentPage - 1)"
      ><</span
    >
    {{ currentPage }}
    <span v-if="showNextLink" class="_pagination-btn" @click="updatePage(currentPage + 1)">></span>
  </div>
</template>

<script>
export default {
  name: 'Pagination',
  props: ['visibleBreweries', 'currentPage', 'perPage'],
  methods: {
    updatePage(pageNum) {
      this.$emit('page-update', pageNum)
    },
  },
  computed: {
    showPreviousLink() {
      return this.currentPage === 1 ? false : true
    },
    showNextLink() {
      return true
    },
  },
}
</script>

<style lang="sass" scoped>
._pagination-btn
  cursor: pointer
</style>
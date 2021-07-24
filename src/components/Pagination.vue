<template>
  <div v-if="totalPages() > 0" class="_pagination-wrapper">
    <span v-if="showPreviousLink()" class="_pagination-btn" @click="updatePage(currentPage - 1)"
      ><</span
    >
    {{ currentPage + 1 }} of {{ totalPages() }}
    <span v-if="showNextLink()" class="_pagination-btn" @click="updatePage(currentPage + 1)"
      >></span
    >
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
    totalPages() {
      return Math.ceil(this.visibleBreweries.length / this.perPage)
    },
    showPreviousLink() {
      return this.currentPage === 0 ? false : true
    },
    showNextLink() {
      return this.currentPage === this.totalPages() - 1 ? false : true
    },
  },
}
</script>

<style lang="sass" scoped>
._pagination-btn
  cursor: pointer
</style>
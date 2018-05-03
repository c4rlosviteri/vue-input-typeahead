<template>
  <ul
    class="typeahead__list"
    v-if="show"
    role="listbox"
  >
    <item
      v-for="(result, index) in results"
      @click.native="onItemSelected(index)"
      :selected="isSelected(index)"
      :aria-selected="isSelected(index)"
      :key="index"
      :value="result"
    />
  </ul>
</template>

<script>
import Item from './Item'

export default {
  name: 'list',
  props: {
    results: Array,
    selectedItemIndex: Number,
    show: Boolean
  },
  components: {
    Item
  },
  methods: {
    onItemSelected (index) {
      this.$emit('item-selected', index)
    },
    isSelected (index) {
      return index === this.selectedItemIndex
    }
  }
}
</script>

<style scoped>
.typeahead__list {
  border: 1px solid palevioletred;
  max-height: 10.625rem;
  list-style-type: none;
  margin: -1px 0 0 0;
  overflow-y: auto;
  padding-left: 0;
}
</style>

<template>
  <li
    role="option"
    class="typeahead__item"
    :class="{ selected }"
  >
    {{ value }}
  </li>
</template>

<script>
export default {
  name: 'item',
  props: {
    selected: Boolean,
    value: String
  },
  updated () {
    this.scrollToItem()
  },
  methods: {
    scrollToItem () {
      if (this.$el.classList.length) {
        const itemEl = this.$el
        const itemHeight = itemEl.offsetHeight
        const listEl = itemEl.parentNode
        const listHeight = listEl.clientHeight
        listEl.scrollTop = itemEl.offsetTop - (listHeight - itemHeight) / 2
      }
    }
  }
}
</script>

<style scoped>
.typeahead__item {
  cursor: pointer;
  padding: .5rem .625rem;
  transition: all .25s ease;
}
.typeahead__item:hover {
  background: rgba(216, 112, 147, .4);
}
.typeahead__item:active,
.selected,
.selected:hover {
  background: rgb(216, 112, 147);
  color: #ffffff;
}
</style>

<template>
  <div
    class="typeahead"
    aria-haspopup="listbox"
    role="combobox"
  >
    <input
      class="typeahead__input"
      @keyup.down="onArrowDown"
      @keyup.up="onArrowUp"
      @keyup.enter="onEnter"
      :placeholder="placeholder"
      role="searchbox"
      type="text"
      ref="input"
      v-model="value"
    >
    <list
      :results="results"
      :selectedItemIndex="selectedItemIndex"
      @item-selected="setSelectedItem"
      :show="show"
    />
  </div>
</template>

<script>
import List from './List'

export default {
  name: 'input-autocomplete',
  components: {
    List
  },
  props: {
    placeholder: String,
    baseUrl: String
  },
  data () {
    return {
      show: false,
      results: [],
      selectedItemIndex: -1,
      value: ''
    }
  },
  watch: {
    value (value) {
      if (value.length) {
        this.getResults(value)
      } else {
        this.resetData()
      }
    }
  },
  methods: {
    getResults (value) {
      fetch(`${this.baseUrl}${value}`)
        .then(blob => blob.json())
        .then(results => {
          results = results.map(result => result.name)
          this.results = results
          this.show = true
        })
        .catch(console.error)
    },
    resetData () {
      this.show = false
      this.results = []
      this.selectedItemIndex = -1
    },
    onArrowUp () {
      const currentItemIndex = this.selectedItemIndex
      let previousItemIndex = currentItemIndex - 1
      if (previousItemIndex < 0) {
        previousItemIndex = this.results.length - 1
      }
      this.selectedItemIndex = previousItemIndex
      this.updateInputValue(previousItemIndex)
    },
    onArrowDown () {
      const currentItemIndex = this.selectedItemIndex
      let nextItemIndex = currentItemIndex + 1
      if (nextItemIndex >= this.results.length) {
        nextItemIndex = 0
      }
      this.selectedItemIndex = nextItemIndex
      this.updateInputValue(nextItemIndex)
    },
    onEnter () {
      const index = this.selectedItemIndex
      this.setSelectedItem(index)
    },
    setSelectedItem (index) {
      this.updateInputValue(index)
      this.resetData()
    },
    updateInputValue (index) {
      this.$refs.input.value = this.results[index]
    }
  }
}
</script>

<style scoped>
.typeahead {
  font-family: system-ui;
  max-width: 18rem;
}
.typeahead__input {
  border: 1px solid palevioletred;
  box-sizing: border-box;
  font-family: system-ui;
  outline: none;
  padding: .5rem .625rem;
  width: 100%;
}
</style>

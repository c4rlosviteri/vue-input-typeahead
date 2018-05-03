<template>
  <div id="app">
    <input-autocomplete
      placeholder="Search states"
      base-url="http://localhost:3000/states?q="
    />
  </div>
</template>

<script>
import InputAutocomplete from '@/components/InputAutocomplete'

export default {
  name: 'App',
  components: {
    InputAutocomplete
  }
}
</script>

<style>

</style>

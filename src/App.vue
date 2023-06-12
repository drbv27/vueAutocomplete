<template>
  <div class="ppal">
    <h1 class="title">Autocomplete email manager</h1>
    <AutocompleteInput></AutocompleteInput>
  </div>
</template>

<script>
import AutocompleteInput from './components/AutocompleteInput.vue';

export default {
  components: {
    AutocompleteInput,
  },
};
</script>
<style>
 .title {
  color: blue;
  text-align: center;
  font-size: 28px;
  font-weight: bold;
  margin: 0;
  padding: 0;
  border: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
 }
</style>

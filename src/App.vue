<template>
  <div class="px-7 py-10">
    <h1 class='font-serif text-4xl font-semibold text-center text-gray-500'>List of persons : </h1>
    <!-- DataListing is the component what will render the list of persons and manipulate Data inside it -->
    <DataListing />
  </div>
</template>

<script>
import DataListing from './components/DataListing.vue'


export default {
  name: 'App',
  components: {
    DataListing
  },    
}

</script>
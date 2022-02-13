<script setup lang="ts">
import { useHead } from '@vueuse/head';

// https://github.com/vueuse/head
// you can use this to manipulate the document head in any components,
// they will be rendered correctly in the html results with vite-ssg
useHead({
  title: 'Experimental Backoffice',
  meta: [
    { name: 'description', content: 'Multi-Purpose Back-Office for Experimental/Testing App' },
  ],
})
</script>

<template>
  <RouterView />
</template>

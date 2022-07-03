<template>
  <p class="singleP">You've chosen movie id: {{id}}</p>
</template>

<script>
export default {
props: ['id']
}
</script>

<style>
.singleP {
    text-align: center;
    margin: 2em;
}
</style>
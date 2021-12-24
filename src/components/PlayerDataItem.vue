<template>
  <p>{{this.data}}</p>
</template>

<script>
export default {
    name: 'PlayerDataItem',
    props: {
        data: Number
    }
}
</script>

<style>
  p {
    margin: 2px;
  }  
</style>
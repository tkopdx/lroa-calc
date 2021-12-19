<template>
  <div>
      <h2>
          Let's get calculating!
      </h2>
      <FormPage
        :type="this.page"
      />
  </div>
</template>

<script>
import FormPage from './FormPage.vue';

export default {
    name: 'CalcForm',
    data() {
        return {
            page: 1
        }
    },
    components: {
        FormPage   
    },
    props:{
        players: Array
    }
}
</script>

<style>

</style>
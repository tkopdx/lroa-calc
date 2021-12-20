<template>
  <div>
      <p>How many of these do you have?</p>
      <img>
      <div>
          <input v-for="input in inputs" :key="input" type="number">
          </div>
      <button v-if="isNotFirstPage">back</button>
      <button v-if="isNotLastPage">next</button>
      <button v-else>calculate!</button>
  </div>
</template>

<script>
export default {
    name: 'FormPage',
    data() {
        return {
            inputs: []
        }
    },
    props: {
        page: Number
    },
    created() {
        switch(this.page) {
            case 1: 
                this.inputs.push(1, 2);
                console.log(this.inputs);
                break;
            case 2:
                this.inputs.push(3, 4, 5);
                console.log(this.inputs);
                break;
            case 3:
                this.inputs.push(6, 7, 8);
                console.log(this.inputs);
                break;
            case 4:
                this.inputs.push(9, 10, 11);
                console.log(this.inputs);
                break;
            default:
                this.inputs.push(12);
                console.log(this.inputs);
        }
    },
    computed: {
        isNotLastPage() {
            return this.page < 5;
        },
        isNotFirstPage() {
            return this.page === 1;
        }
    }
}
</script>

<style>

</style>
<template>
  <div>
      <p>How many of these do you have?</p>
      <img>
      <input>
      <button>back</button>
      <button>enter</button>
  </div>
</template>

<script>
export default {
    name: 'FormPage',
    props: {
        page: Number
    },
    computed() {
        img() {
            switch(this.page) {
                case 1: 
            }
        }
    }
}
</script>

<style>

</style>
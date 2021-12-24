<template>
  <div class="form-page">
        <div v-for="input in calcInputs" :data-key="input" :key="input" class="form-page-item">
            <FormInput :firstRef="firstRef" :type="input" :value="this.player.data.inputs[input]"/>
        </div>
  </div>
</template>

<script>
import FormInput from './FormInput.vue';

export default {
    name: 'FormPage',
    components: {
        FormInput
    },
    props: {
        page: Number,
        player: Object
    },
    computed: {
        calcInputs: function () {
            switch(this.page) {
            case 1: 
                return [1, 2];
            case 2:
                return [3, 4, 5];
            case 3:
                return [6, 7, 8];
            case 4:
                return [9, 10, 11];
            default:
                return [12];
            }
        },
        firstRef: function () {
            switch(this.page) {
                case 1:
                    return 1;
                case 2:
                    return 3;
                case 3:
                    return 6;
                case 4:
                    return 9;
                default:
                    return 12;
            }
        }
    },
    methods: {
    }
}
</script>

<style>

    .form-page {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: stretch;
        font-size: 1rem;
    }

    .form-page-item {
        margin: 10px auto;
        width: 80%;
    }

</style>
<template>
    <li>
        <input type="text" :placeholder="name" @input="emitNameChange"/>
    </li>
</template>

<script>

export default {
    name: 'Player',
    props: {
        name: String,
        data: Object,
        id: Number
    },
    methods: {
        emitNameChange: function (e) {
            console.log('name changed!', e.target.value, this.id);
            this.emitter.emit('handlePlayerNameChange', {value: e.target.value, id: this.id});
        }
    }
}
</script>

<style>

</style>
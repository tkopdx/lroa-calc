<template>
    <li>
        <input type="text" :placeholder="name" @input="emitNameChange"/>
        <div v-for="item in Object.keys(this.data)" :key=item>
            <GameIcons :type="parseInt(item)"/>
            <p>{{this.data[item]}}</p>
        </div>
        <p>
            Total: {{this.total}}
        </p>
    </li>
</template>

<script>
import images from '../functions/imagesPlayer';
import GameIcons from './GameIcons.vue';

export default {
    name: 'Player',
    components: {
        GameIcons
    },
    props: {
        name: String,
        data: Object,
        id: Number,
        total: Number
    },
    methods: {
        emitNameChange: function (e) {
            console.log('name changed!', e.target.value, this.id);
            this.emitter.emit('handlePlayerNameChange', {value: e.target.value, id: this.id});
        },
        imgSrc: function (item) {
            console.log(images[item]);
            return require(images[item]);
        }
    }
}
</script>

<style>

    li {
        display: flex;
        align-items: center;
        justify-content: space-evenly;
    }

</style>
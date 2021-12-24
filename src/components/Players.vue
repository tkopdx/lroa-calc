<template>
    <div>
        <h2>
            Players
        </h2>
        <div v-if="!this.isCalculating">
            <button v-if="this.numOfPlayers > 1" @click="emitDecreasePlayers">remove player</button>
            <button v-if="this.numOfPlayers < 4" @click="emitIncreasePlayers">add player</button>
        </div>
        <ul>
            <Player v-for="player in players" :key="player.id" :name="player.name" :data="player.data.calculatedPoints" :id="player.id" :total="player.total"/>
        </ul>
    </div>
  
</template>

<script>
import Player from './Player.vue';

export default {
    name: 'Players',
    props: {
        players: Array,
        numOfPlayers: Number,
        isCalculating: Boolean
    },
    components: {
        Player
    },
    methods: {
        emitDecreasePlayers() {
            this.emitter.emit('handleDecreasePlayers');
        },
        emitIncreasePlayers() {
            this.emitter.emit('handleIncreasePlayers');
        },
    }
}
</script>

<style>

    ul {
        padding-left: 0;
    }

</style>
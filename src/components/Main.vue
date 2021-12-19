

<template>
    <h1>
        Welcome to the Lost Ruins of Arnak Calculator!
    </h1>
    <button v-if="this.numOfPlayers > 1" v-on:click="decreasePlayers">remove player</button>
    <button v-if="this.numOfPlayers < 4" v-on:click="increasePlayers">add player</button>
    <Players
        :players="this.players"
        @update:title="updatePlayerName"
    />
    <button @click="setIsCalculating(true)" >Begin Calc!</button>
    <CalcForm
        v-if="isCalculating"
    />
</template>

<script>
const playerTemplate = {
    name: '',
    id: 0,
    data: {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
        5: 0,
        6: 0,
        7: 0,
        8: 0
    }
}

import Players from './Players.vue';
import CalcForm from './CalcForm.vue';

export default {

    name: 'Calculator',
    components: {
        Players,
        CalcForm
    },
    data() {
        return {
            numOfPlayers: 0,
            players: [],
            isCalculating: false
        }
    },
    created() {
        this.increasePlayers();
    },
    mounted() {
        this.emitter.on('handlePlayerNameChange', this.updatePlayerName);
    },
    methods: {
        increasePlayers: function () {
            console.log(this.numOfPlayers);
            this.numOfPlayers++;
            console.log(this.numOfPlayers);
            return this.addPlayer(this.numOfPlayers);
        },
        decreasePlayers: function () {
            this.removePlayer(this.numOfPlayers);
            this.numOfPlayers--;
        },
        addPlayer: function (player) {
            const newPlayer = {...playerTemplate};
            newPlayer.name = `Player ${player}`
            newPlayer.id = player;
            this.players.push(newPlayer);
            console.log(this.players);
        },
        removePlayer: function (id) {
            const playerI = this.players.findIndex(player => player.id === id);
            this.players.splice(playerI, 1);
        },
        updatePlayerName: function (info) {
            console.log(info);
            console.log(this.players);
            const playerI = this.players.findIndex(player => player.id === info.id);
            this.players[playerI].name = info.value;
        },
        setIsCalculating(state) {
            this.isCalculating = state;
            console.log(this.isCalculating);
        }
    }
}
</script>

<style>

</style>
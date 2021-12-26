

<template>
    <div class="app-box">
        <div class="main-content">
            <h1>
                The Lost Ruins of Arnak Calculator!
            </h1>
            <Players
                :players="this.players"
                @update:title="updatePlayerName"
                :isCalculating="this.isCalculating"
                :numOfPlayers="numOfPlayers"
            />
            <button v-if="isCalculating" @click="setIsCalculating(false)" >close calculator</button>
            <button v-else @click="setIsCalculating(true)" >open calculator</button>
            <CalcForm
                v-if="this.isCalculating"
                :player="calculatingPlayerObject"
                :isFinalPlayer="isFinalPlayer"
                :isFirstPlayer="isFirstPlayer"
            />
            <div class="attributions">Icons made by <a href="https://www.flaticon.com/authors/ibrandify" title="ibrandify">ibrandify</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a></div>
        </div>
    </div>
</template>

<script>
import Players from './Players.vue';
import CalcForm from './CalcForm.vue';
import Player from '../functions/Player';
import { computed } from 'vue';

export default {

    name: 'Calculator',
    components: {
        Players,
        CalcForm
    },
    setup() {
        
    },
    data() {
        return {
            numOfPlayers: 0,
            players: [],
            isCalculating: false,
            calculatingPlayer: 1
        }
    },
    provide() {
        return {
            numOfPlayers: computed(() => this.numOfPlayers),
        }
    },
    created() {
        this.increasePlayers();
    },
    mounted() {
        this.emitter.on('handlePlayerNameChange', this.updatePlayerName);
        this.emitter.on('handleCalcFormInputChange', this.handleCalcFormInputChange);
        this.emitter.on('handleCalculateTotal', this.calculateTotal);
        this.emitter.on('nextPlayer', this.nextPlayer);
        this.emitter.on('previousPlayer', this.previousPlayer);
        this.emitter.on('handleDecreasePlayers', this.decreasePlayers);
        this.emitter.on('handleIncreasePlayers', this.increasePlayers);
        this.emitter.on('handleCloseCalcForm', this.setIsCalculating);
    },
    computed: {
        calculatingPlayerObject: function () {
            const playerI = this.players.findIndex(player => player.id === this.calculatingPlayer);
            return this.players[playerI];
        },
        isFinalPlayer() {
            return this.calculatingPlayer >= this.players.length;
        },
        isFirstPlayer() {
            return this.calculatingPlayer === 1;
        }
    }, 
    methods: {
        increasePlayers: function () {
            // console.log(this.numOfPlayers);
            this.numOfPlayers++;
            // console.log(this.numOfPlayers);
            return this.addPlayer(this.numOfPlayers);
        },
        decreasePlayers: function () {
            this.removePlayer(this.numOfPlayers);
            this.numOfPlayers--;
        },
        addPlayer: function (player) {
            const newPlayer = new Player(player);
            newPlayer.name = `Player ${player}`
            newPlayer.id = player;
            this.players.push(newPlayer);
            // console.log(this.players);
        },
        removePlayer: function (id) {
            const playerI = this.players.findIndex(player => player.id === id);
            this.players.splice(playerI, 1);
        },
        updatePlayerName: function (info) {
            // console.log(info);
            // console.log(this.players);
            const playerI = this.players.findIndex(player => player.id === info.id);
            this.players[playerI].name = info.value;
        },
        setIsCalculating(state) {
            this.isCalculating = state;
            // console.log(this.isCalculating);
        },
        nextPlayer() {
            this.calculatingPlayer++;
            this.emitter.emit('setCalcFormPage', {page: 1});
        },
        previousPlayer() {
            this.calculatingPlayer--;
            this.emitter.emit('setCalcFormPage', {page: 5});
        },
        handleCalcFormInputChange: function (info) {
            const playerI = this.players.findIndex(player => player.id === this.calculatingPlayer);
            this.players[playerI].data.inputs[info.type] = info.value;
            // console.log(this.players[playerI]);
            this.calculatePoints(info.type);
        },
        calculatePoints: function (type) {
            const playerI = this.players.findIndex(player => player.id === this.calculatingPlayer);
            const inputValue = this.players[playerI].data.inputs[type];
            let value;
            switch (type) {
                case 1:
                    value = inputValue;
                    break;
                case 2:
                    value = inputValue;
                    break;
                case 3:
                    value = inputValue * 11;
                    break;
                case 4:
                    value = inputValue * 6;
                    break
                case 5:
                    value = inputValue * 2;
                    break;
                case 6:
                    value = inputValue * 3;
                    break;
                case 7:
                    if (inputValue === 0) {
                        value = 0;
                    } else if (inputValue === 1) {
                        value = 4;
                    } else if (inputValue === 2) {
                        value = 7;
                    } else if (inputValue === 3) {
                        value = 9;
                    } else {
                        value = 10;
                    }
                    break;
                case 8:
                    value = inputValue * 5;
                    break;
                case 9:
                    value = inputValue * 3;
                    break;
                case 10:
                    value = inputValue * 2;
                    break;
                case 11:
                    value = inputValue;
                    break;
                default:
                    value = inputValue * -1;
            }
            this.players[playerI].data.calculatedPoints[type] = value;
        },
        calculateTotal: function () {
            this.setIsCalculating(false);
            this.players.map(player => {
                const total = Object.keys(player.data.calculatedPoints).reduce( (acc, key) => {
                    acc += player.data.calculatedPoints[key];
                    return acc;
                }, 0);

                player.total = total;
            });

            
            this.emitTotalCalculated();
        },
        emitTotalCalculated: function () {
            this.emitter.emit('handleTotalCalculated');
        }
    }
}
</script>

<style>

    h1 {
        margin: 0;
    }

    button {
        border-radius: 5px;
        padding: 10px;
        margin: 3px;
        background-color: #2EA879;
        border: none;
        box-shadow: 0 0 3px 1px black;
        transition: all .1s;
    }

    button:hover {
        background-color: #3fca95;
        box-shadow: 0 0 5px 1px black;
    }

    .app-box {
        display: flex;
        margin: 0;
        align-items:center;
        justify-content: center;
    }

    .main-content {
        width: 95%;
        margin: 10px auto;
        background-color: rgba(0, 0, 0, 0.329);
        border-radius: 5px;
        min-height: 100%;
        height: auto;
        color: white;
    }

    .attributions {
        margin: 5px auto;
        font-size: 8px;
    }

</style>
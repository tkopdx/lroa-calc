<template>
    <div class="form-input-box">
        <p>{{explanationText}}</p>
        <div>
            <GameIcons :type="this.type"/>
            <input ref="input" @input="emitInputChange" type="number" :value="this.value" min=0 :max="computedMax">
        </div>
        <small>{{calculationText}}</small>
    </div>
</template>

<script>
const explanationText = {
    1: 'Number of maginifying glass research points (0-23)',
    2: 'Number of handbook research points (0-?)',
    3: 'Number of 11 point tiles (up to number of players)',
    4: 'Number of 6 point tiles (up to number of players)',
    5: 'Number of 2 point tiles (up to number of players)',
    6: 'Number of idols (up to 16)',
    7: 'Number of unused free action idol spaces (up to 4)',
    8: 'Number of guardians (up to 12)',
    9: 'Number of 3-point cards',
    10: 'Number of 2-point cards',
    11: 'Number of 1-point cards',
    12: 'Number of fear cards',
};

const calculationText = {
    1: 'no modification',
    2: 'no modification',
    3: '11 x number of tiles',
    4: '6 x number of tiles',
    5: '2 x number of tiles',
    6: '3 x number of idols',
    7: '1: 4, 2: 7, 3: 9, 4: 10',
    8: '5 x number of guardians',
    9: '3 x number of cards',
    10: '2 x number of cards',
    11: '1 x number of cards',
    12: '-1 x number of cards'
}

import GameIcons from './GameIcons.vue';

export default {
    name: "FormInput",
    components: {
        GameIcons
    },
    inject: ['numOfPlayers'],
    props: {
        type: Number,
        value: Number,
        firstRef: Number
    },
    mounted() {
        if (this.isFirstRef) this.$refs.input.focus();
    },
    updated() {
        if (this.isFirstRef) this.$refs.input.focus();
    },
    methods: {
        emitInputChange: function (e) {
            // console.log('input changed!', e.target.value, this.type);
            this.emitter.emit('handleCalcFormInputChange', {value: parseInt(e.target.value), type: this.type});
        }
    },
    computed: {
        explanationText: function () {
            return explanationText[this.type]
        },
        calculationText: function () {
            return calculationText[this.type]
        },
        computedMax: function () {
            // console.log(this.numOfPlayers.value);
            switch (this.type) {
                case 1:
                    return 23;
                case 2:
                    return 10;
                case 3:
                    return this.numOfPlayers.value;
                case 4:
                    return this.numOfPlayers.value * 2;
                case 5:
                    return this.numOfPlayers.value * 3;
                case 6:
                    return 16;
                case 7:
                    return 4;
                case 8:
                    return 12;
                case 9:
                    return 100;
                case 10:
                    return 100;
                case 11:
                    return 100;
                default:
                    return 100;       
            }
        },
        isFirstRef: function () {
            return this.firstRef === this.type;
        }
    }
}
</script>

<style>

    .form-input-box {
        display:flex;
        flex-direction: column;
        justify-content: space-around;
        align-items: center;
        background-color: white;
        color: black;
        padding: 5px;
        border-radius: 5px;
        box-shadow: 0 0 3px 1px black;
    }

    .form-input-box div {
        width: 80%;
        display: flex;
        align-items: stretch;
        justify-content: center;
        margin: 10px auto;
    }

    .form-input-box input {
        min-width: 100%;
        margin-left: 10px;
    }

</style>
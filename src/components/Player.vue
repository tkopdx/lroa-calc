<template>
    <li>
        <input class="player-name" type="text" :placeholder="name" @input="emitNameChange"/>
        <div class="player-data-items">
            <div class="player-data-item" v-for="item in Object.keys(this.data)" :key=item>
                <GameIcons :type="parseInt(item)"/>
                <PlayerDataItem :data="this.data[item]"/>
            </div>
        </div>
        <p class="total">
            Total: <span :class="this.showTotal ? 'grow' : 'total-span'">{{this.showTotal ? this.total : this.fakeTotal}}</span>
        </p>
    </li>
</template>

<script>
import images from '../functions/imagesPlayer';
import GameIcons from './GameIcons.vue';
import PlayerDataItem from './PlayerDataItem.vue';

export default {
    name: 'Player',
    data() {
        return {
            showTotal: false,
            fakeTotal: 0,
            fakeTotalInterval: null
        }
    },
    created() {
        this.emitter.on('handleTotalCalculated', this.handleTotalCalculated);
    },
    components: {
        GameIcons,
        PlayerDataItem
    },
    props: {
        name: String,
        data: Object,
        id: Number,
        total: Number
    },
    methods: {
        emitNameChange: function (e) {
            // console.log('name changed!', e.target.value, this.id);
            this.emitter.emit('handlePlayerNameChange', {value: e.target.value, id: this.id});
        },
        imgSrc: function (item) {
            // console.log(images[item]);
            return images[item];
        },
        increaseFakeTotal() {
            if (this.fakeTotal >= this.total - 1) {
                clearInterval(this.fakeTotalInterval);
                this.handleFakeTotalEnd();
            }
            this.fakeTotal++;
        },
        handleTotalCalculated: function () {
            if (this.showTotal || this.fakeTotalInterval) {
                clearInterval(this.fakeTotalInterval);
                this.showTotal = false;
                this.fakeTotal = 0;
            }
            this.fakeTotalInterval = setInterval(this.increaseFakeTotal, 10);
        },
        handleFakeTotalEnd() {
            // console.log(this.total);
            this.showTotal = true
        }
    }
}
</script>

<style>

    @keyframes grow {
        0% {
            transform: scale(1)
        }
        50% {
            transform: scale(1.5)
        }
        100% {
            transform: scale(1)
        }
    }

    li {
        display: flex;
        align-items: center;
        justify-content: space-evenly;
        box-shadow: 0 0 3px 1px black;
        background-color: white;
        border-radius: 5px;
        margin: 15px auto;
        width: 90%;
        padding: 5px;
        transition: all 0.1s;
        color: black;
    }

    li:hover {
        transform: scale(1.05);
    }

    .total {
       padding: 3px;
    }

    .player-data-items {
        overflow-x: auto;
        display: flex;
    }

    .player-data-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 3px;
    }

    .total-span {
        background: none;
    }

    .grow {
        display: inline-block;
        background: #2EA879;
        color: white;
        border-radius: 5px;
        padding: 2px;
        font-size: 1.5rem;
        transition: all 0.2s;
        animation: grow .3s 1;
        animation-timing-function: cubic-bezier(0.17, 0.84, 1, 0.47);
    }

</style>
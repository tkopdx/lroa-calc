<template>
    <div class="calc-outer">
        <div class="calc-mid">
  <div class="calc-form">
        <button class="close-but" @click="emitCloseCalcForm">x</button>
      <h2>
          Let's get calculating!
      </h2>
      <small ref="player" class="calc-player">calculating {{this.player.name}}</small>
      <FormPage
        :page="this.page"
        :nextPage="increasePage"
        :player="this.player"
      />
      <div class="buttons-box">
          <button v-if="isNotFirstPage" @click="decreasePage">back</button>
      <button v-if="!isNotFirstPage && !this.isFirstPlayer" @click="emitPreviousPlayer">previous player</button>
      <button v-if="isNotLastPage" @click="increasePage">next</button>
      <div v-else>
            <button v-if="this.isFinalPlayer" @click="emitCalculateTotal">calculate!</button>
            <button v-else @click="emitNextPlayer">next player</button>
        </div>
        </div>
  </div>
        </div>
  </div>
</template>

<script>
import FormPage from './FormPage.vue';

export default {
    name: 'CalcForm',
    created() {
        this.emitter.on('setCalcFormPage', this.setCalcFormPage)
    },
    data() {
        return {
            page: 1
        }
    },
    components: {
        FormPage   
    },
    props:{
        player: Object,
        isFinalPlayer: Boolean,
        isFirstPlayer: Boolean
    },
    computed: {
        isNotLastPage: function () {
            return this.page < 5;
        },
        isNotFirstPage: function () {
            return this.page > 1
        }
    },
    watch: {
    },
    methods: {
        increasePage: function () {
            if (this.page >= 5) return; 
            this.page++;
        },
        decreasePage: function () {
            if (this.page <= 1) return;
            this.page--;
        },
        emitCalculateTotal: function () {
            this.emitter.emit('handleCalculateTotal');
        },
        emitNextPlayer: function () {
            this.emitter.emit('nextPlayer');
        },
        emitPreviousPlayer: function () {
            this.emitter.emit('previousPlayer');
        },
        setCalcFormPage: function (info) {
            this.page = info.page;
        },
        emitCloseCalcForm: function () {
            this.emitter.emit('handleCloseCalcForm', false);
        }
    }
}
</script>

<style>

    .calc-outer {
        display: table;
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background-color:rgba(0, 0, 0, 0.589);
    }

    .calc-mid {
        display: table-cell;
        vertical-align: middle;
    }

    .calc-form {
        box-shadow: 0 0 2px 1px black;
        background: linear-gradient(#f9bf3b, #fcda8d);
        width: 80%;
        margin: auto;
        border-radius: 5px;
        position: relative;
    }

    .close-but {
        position: absolute;
        top: 2px;
        left: 2px;
        width: 20px;
        height: 20px;
        padding: 0;
        margin: auto;
        border-radius: 100px;
    }

    .calc-form h2 {
        padding-top: 25px;
    }

    .buttons-box {
        display: flex;
        margin: auto;
        justify-content: center;
        align-items: center;
        padding: 15px;
    }

</style>
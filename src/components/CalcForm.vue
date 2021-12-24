<template>
  <div class="calc-form">
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
        scrollToQuestion: function () {

        }
    }
}
</script>

<style>

    .calc-form {
        box-shadow: 0 0 2px 1px black;
        width: 95%;
        margin: auto;
    }

    .buttons-box {
        display: flex;
        margin: auto;
        justify-content: center;
        align-items: center;
        margin-top: 15px;
    }

</style>
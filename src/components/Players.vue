<template>
    <div>
        <h2>
            Players
        </h2>
        <ul>
            <Player v-for="player in players" :key="player.id" :name="player.name" :data="player.data" :id="player.id"/>
        </ul>
    </div>
  
</template>

<script>
import Player from './Player.vue';

export default {
    name: 'Players',
    props: {
        players: Array
    },
    components: {
        Player
    }
}
</script>

<style>

</style>
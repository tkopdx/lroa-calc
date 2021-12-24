<template>
  <img :src="imgSrc">
</template>

<script>

export default {
    name: 'GameIcons',
    props: {
        type: Number
    },
    computed: {
        imgSrc() {
            switch(this.type) {
                case 1:
                    return require('../assets/research.png');
                case 2:
                    return require('../assets/book.png');
                case 3:
                    return require('../assets/hieroglyph.png');
                case 4:
                    return require('../assets/hieroglyph.png');
                case 5:
                    return require('../assets/hieroglyph.png');
                case 6:
                    return require('../assets/statue.png');
                case 7:
                    return require('../assets/statue.png');
                case 8:
                    return require('../assets/dragon.png');
                case 9:
                    return require('../assets/playing-card.png');
                case 10:
                    return require('../assets/playing-card.png');
                case 11:
                    return require('../assets/playing-card.png');
                case 12:
                    return require('../assets/scary-eyes.png');
                default:
                    return 'error!';
            }
        }
    }
}
</script>

<style>

    img {
        height: 25px;
        width: auto;
        margin: 2px;
    }

</style>
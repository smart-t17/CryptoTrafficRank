<template>
    <div>
        <img :style="{width:compSize, height:compSize, margin:compMargin}" src="../../assets/spin.svg" alt="">
    </div>
</template>

<style scoped>
    img{
        display: block;
    }
</style>

<script>
export default {
    props:['size','margin'],
    computed:{
        compSize(){
            return (this.size ? this.size : 50) + 'px';
        },
        compMargin(){
            return (this.margin ? this.margin : 'auto');
        }
    }
}
</script>

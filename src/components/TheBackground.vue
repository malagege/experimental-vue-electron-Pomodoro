<template>
    <div class="bg" :style="{height: h + 'vh'}">
        {{ h }}
    </div>
</template>
<style scoped>
.bg{
    position: fixed;
    bottom: 0px;
    width: 100vw;
    height: 0px;
    z-index: -1;
    background: greenyellow;
    border: 1px solid red;
    transition: all 1s;
}
</style>
<script>
export default {
    props:['h'],
    data(){
        return {
        }
    }
}
</script>

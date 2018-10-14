<template>
    <div>
        {{mm}}:{{ss}}
    </div>
</template>

<script>
export default {
    props:['value'],
    computed:{
        mm(){
            let str = Math.floor(this.value/60).toString()
            str = str.padStart(2,"0")
            return str
        },
        ss(){
            let str = this.value % 60
            str = str.toString().padStart(2,"0")
            return str
        }
    }
}
</script>

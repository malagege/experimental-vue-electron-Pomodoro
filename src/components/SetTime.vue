<template>
    <div>
        <h2>設定</h2>
        <h3>番茄鐘時間</h3>
        <hr>
        <input  :value="mm" @change="input01Handler($event.target.value)">分
        <input  :value="ss" @change="input02Handler($event.target.value)">秒
        <hr>
        <h3>休息時間</h3>
        <hr>
        <input  :value="mm2" @change="input11Handler($event.target.value)">分
        <input  :value="ss2" @change="input12Handler($event.target.value)">秒
        <hr>
        <button @click="okHandler">確定</button>
        <button @click="cancelHandler">取消</button>
    </div>
</template>

<script>
export default {
    props: ['d_time','down_time'],
    data(){
        return {
            input01:0,
            input02:0,
            input11:0,
            input12:0,
        }
    },
    computed:{
        mm(){
            var mm = parseInt( this.d_time / 60 )
            // return mm <= 0 ? '00' : mm.toString().padStart(2,'0')
            return mm;
        },
        ss(){
            var ss =  this.d_time % 60
            // return ss <= 0 ? '00' : ss.toString().padStart(2,'0')
            return ss;
        },
        mm2(){
            var mm = parseInt( this.down_time / 60 )
            return mm;
        },
        ss2(){
            var ss =  this.down_time % 60
            return ss;
        }
    },
    methods:{
        okHandler(){
            this.$emit('ch_time', {
                d_time: parseInt(this.input01*60) + parseInt(this.input02),
                down_time :  parseInt(this.input11*60) + parseInt(this.input12),
            })
        },
        cancelHandler(){
            this.$emit('close_setting')
        },
        input01Handler(value){
            this.input01 = value;

        },
        input02Handler(value){
            this.input02 = value;
        },
        input11Handler(value){
            this.input11 = value;

        },
        input12Handler(value){
            this.input12 = value;
        },
    },
    mounted(){
        this.input01 = this.mm;
        this.input02 = this.ss;
        this.input11 = this.mm2;
        this.input12 = this.ss2;
    }
}
</script>

<style scoped>
h2{
    text-align: center;
}

</style>

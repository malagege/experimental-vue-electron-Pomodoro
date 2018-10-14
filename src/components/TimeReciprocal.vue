<template>
    
</template>


<script>
export default {
    props:['value'],
    data(){
        return {
            id: '',
        }
    },
    created(){
    },
    methods:{
        startTime(){
            if(this.id){
                this.id = setInterval(()=>{
                    this.value--;
                },1000)
            }
        },
        stopTime(){
            if(this.id){
                clearInterval(this.id);
            }
        },
        restTime(){
            this.stopTime();
            this.value = 1500;
        }
    }
}
</script>
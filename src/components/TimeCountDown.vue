<template>
    <div>
        <TimeReciprocal v-model="value"></TimeReciprocal>
        <TimeDownShow  v-model="value"></TimeDownShow>
    </div>
</template>

<script>
import TimeDownShow from './TimeDownShow.vue'
import TimeReciprocal from './TimeReciprocal.vue'

export default {
    props:['value'],

}
</script>

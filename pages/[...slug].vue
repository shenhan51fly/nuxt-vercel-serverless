<script setup lang="ts">
const route = useRoute()
const path = route.path
const emojis = ["🤔","❤️","😊","😎","😄","👍",]
const myRate = ref<number>(0)
const emojiWrapper = ref<HTMLElDivElement>(null)
const {data:rateData, pending} = await useFetch('/api/rate', {
    query:{
        uid:path
    },
    default(){
        return {r1:0,r2:0,r3:0,r4:0,r5:0}
    }
})
function onChange(): void{
    if(!emojiWrapper.value) return
    emojiWrapper.value.scrollTo({
        top: myRate.value * emojiWrapper.value.clientHeight,
        behavior: "smooth"
    })    
}
</script>
<template lang="pug">
    .emoji-wrapper.overflow-hidden.row-span-2.p-1(
        ref="emojiWrapper"
    )
        .emoji.flex.w-18.h-18.justify-center.items-center.snap-y.mb-2.text-7xl(
            v-for="item in emojis" :key="item"
        ) {{item}}
    .text-center.leading-8 your vote 
    .rating.flex-row-reverse.justify-center.items-center(
        @change="onChange"
    )
        input.mask.mask-star-2.w-10.h-10(
            v-for="item in 5"
            type="radio" 
            :key="item"
            name="rating"
            :class="{'active':6 - item === myRate}"
            :value="6 - item"
            v-model="myRate"

        )
    .col-span-2.text-center.pt-1 Ranks
</template>
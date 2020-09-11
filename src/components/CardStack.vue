<template>
  <section>
      <section 
      v-for="(card, index) in stack" 
      :key="index" 
      class="stack-wrapper"
      >

      <SingleCard 
      :cardInfo="card"
      :key="index" 
      @activeCard="activeCard"

        />
      </section>
  </section>
</template>

<script>

import SingleCard from '@/components/Card'

export default {
    name: 'cardStack',
    props:{
        stack: Array
    },
    components: {
        SingleCard
    },
    methods: {
        // Emit med vilket kort som har tryckts på för att visas som "ACTIVE CARD"
        activeCard(e) {

            this.$emit('activeCardView', e)
        }
    }
}
</script>

<style lang="scss" scoped>

    section{
        
        
        .stack-wrapper {
            height: 3rem;
            transition: height  1s ease;
            
            &:last-child{
                margin-bottom: 12rem;
            }
            &:hover {
                height: 10rem;
            }
            &:last-child:hover{
                height: 3rem;
            }
        }
    }

</style>
<template>
  <section>
  
  <Top 
  :topHeader="topInfo"
  />
  
  <SingleCard 
  :cardInfo="cardInfo" 
  />
  
  <CardForm 
  v-on:NewCard="cardView" 
  v-on:AddedCard="addCard"
  :cardInfo="cardInfo"
    />

  </section>
</template>

<script>

import Top from '@/components/Top'
import SingleCard from '@/components/Card'
import CardForm from '@/components/CardForm'

export default {
    name: 'AddCard',
      components: {
        Top,
        SingleCard,
        CardForm
      },
      methods: {
        cardView(e){
        this.cardInfo = e
        this.$emit('updatedView', this.cardInfo)

        },
        addCard(e){
          this.$emit('AddCard', e)

        }
      
      },
      data() {
        return {

          cardInfo: {
            cardHolder: '',
            cardNumber: '',
            valid: 
             {
              Month:'MM',
              Year: 'YY'
             },
            cvv: '',
            bank: '',
            logo: 'Bitcoin',
            theme: ''
          },

          topInfo: {
          header: 'ADD A NEW BANK CARD',
          text: 'NEW CARD'
      }
        }
      },
      
}
</script>

<style lang="scss" scoped>

  section {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>
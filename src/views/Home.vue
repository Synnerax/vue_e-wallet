<template>
<section id="home">
    <Top 
    :topHeader="topInfo" 
    />

    <section class="remove">

      <SingleCard 
      :cardInfo="cardView" 
      v-show="this.show"
      />
      <article class="buttons next" @click="removeCard">
        <h3 class="x">X</h3>
      </article>
  </section>
  <CardStack 
  :stack="stack" 
  @activeCardView="activeCard"
  />
  <router-link to="/addcard" class="add-card">
  <span>ADD A NEW CARD</span>
  </router-link>

  
</section>
</template>

<script>

import Top from '@/components/Top'
import SingleCard from '@/components/Card'
import CardStack from '@/components/CardStack'


export default {
  name: 'Home',
  props: {
    stack: Array
  },
  components: {
    Top,
    SingleCard,
    CardStack
  },
  data(){
    return {
     // gjorde hela cardView här för att inte få undefnied errors när inget finns
      cardView: {
            cardHolder: '',
            cardNumber: '',
            valid: 
             {
              Month:'',
              Year: ''
             },
            cvv: '',
            bank: '',
            logo: '',
            theme: ''
          },

      topInfo: {
        header: 'E-WALLET',
        text: 'ACTIVE CARD'
      },
      show: false
    }
  },
  methods: {
    // När du klickar på ett kort i cardStack, visa det kortet som det aktiva.
    activeCard(e) {
      this.cardView = e
      this.show = true
    },
    // När du klickar på (X) på det akriva kortet. 
    // så döljs singleCard komponent och kortets index skickas till App.vue
    removeCard() {
     this.show = false
      this.$emit('removeCard', this.cardView.index)
      
    }
  }
}
</script>

<style lang="scss" scoped>

section {
  display: flex;
  flex-direction: column;
  align-items: center;

  .add-card {
    
    width: 22rem;
    height: 5rem;
    
    margin-top: 3rem;
    border: 1px solid #000000;
    box-sizing: border-box;
    border-radius: 8px;

    font-family: PT Mono;
    font-weight: bold;
    font-size: 22px;
    line-height: 25px;

    display: flex;
    justify-content: center;
    align-items: center;
    text-decoration: none;

    color: #000000;
  }
  .buttons {
            display: none;
            position: absolute;
            background: linear-gradient(248.3deg, rgba(0, 0, 0, 0.16) 0%, rgba(0, 0, 0, 0) 100%), #FF002D;
            border: 2px solid rgba(255, 255, 255, 0.7);
            border-radius: 50%;
            width: 25px;
            height: 25px;
            right: 5px;
            top: 5px;

            .x {
              margin: 0;
              width: 100%;
              height: 100%;

              display: flex;
              justify-content: center;
              align-items: center;
            }
  }
    
}
  .remove {
    margin-bottom: 40px;;
    position: relative;
    &:hover{
    .buttons.next {
          cursor: pointer;
					display: block;
					animation: bounceInRight 200ms; 
				}
    }
  }




</style>
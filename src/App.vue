<template>
  <main id="app">
    
    <router-view 
    v-on:AddCard="addCard" 
    v-on:updatedView="updateView" 
    v-on:removeCard="remove" 
    :stack="cards"
    />
  </main>
</template>

<script>


export default {
  name: 'App',
  data(){
    return {
      cards: [],
      
    }
  },
  methods: {
    addCard(e){
    this.cards.push(e)
    localStorage.setItem('cards', JSON.stringify(this.cards))
    },
    updateView(e){
      this.cardView = []
      this.cardView.push(e)
    },
    remove(e){
      let localStack = JSON.parse(localStorage.getItem('cards'))
      localStack.splice(e, 1)

      localStorage.setItem('cards', JSON.stringify(localStack))
      
      this.cards.splice(e, 1)
      
      console.log(localStack, e)
    }
    
  },
  mounted() {
    if (localStorage.cards) {
      this.cards = JSON.parse(localStorage.cards);
    }
  },

}
</script>
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>

<template>
  <form action="/">
  <label for="card-number">
      CARD NUMBER
  </label>
  <input type="text" 
  v-model="cardNumber" 
  @keyup="addCard" 
  @keydown="isNumber" 
  id="card-number" 
  class="full"
  v-bind:class="{ 'missing-input': !validate.cardNumber}"
  >

  <label for="cardholder-name">
      CARDHOLDER NAME
  </label>
  <input type="text" 
  v-model="cardHolder" 
  @keyup="addCard" 
  @keydown="handleInputname" 
  id="cardholder-name" 
  class="full"
  v-bind:class="{ 'missing-input': !validate.cardHolder}"
  >

  <section class="half-wrapper">
      <section class="wrapper ">

        <label for="valid-date">
            VALID THRU
        </label>

        <section class="half" v-bind:class="{ 'missing-input': !validate.month || !validate.year}"> 

            <input type="text" 
            v-model="validDate.Month" 
            @keyup="addCard" 
            @keydown="isMonth" 
            id="valid-date" 
            class="date"
            >

            <span>/</span>
            
            <input type="text" 
            v-model="validDate.Year" 
            @keyup="addCard" 
            @keydown="isYear" 
            class="date"
            >
        </section>
      </section>
    
        <section class="wrapper">

            <label for="cvv">CVV</label>

            <input type="text" 
            v-model="cvv" 
            @keyup="addCard" 
            @keydown="isCVV"  
            id="cvv" 
            class="half"
            v-bind:class="{ 'missing-input': !validate.cvv}"
            >
        </section>
  </section>

  <label for="select-bank">VENDOR</label>
  <select name="Bank" 
  v-model="bank" 
  id="select-bank" 
  class="full" 
  v-bind:class="{ 'missing-input': !validate.bank}"
  >

      <option value="BITCOIN INC" @click="addCard">BITCOIN INC</option>
      <option value="NINJA BANK" @click="addCard">NINJA BANK</option>
      <option value="BLOCK CHAIN INC" @click="addCard">BLOCK CHAIN INC</option>
      <option value="EVIL CORP" @click="addCard">EVIL CORP</option>

  </select>

    <router-link to="/" v-if="inputFormValidated">

    <section class="add-card" 
    @click="addCardToStack"
    >
        <p>
            ADD CARD
        </p>
    </section>
    </router-link>

    <section class="add-card" 
    @click="checkInput" 
    v-else
    >
        <p>
            ADD CARD
        </p>
    </section>
        
  </form>
</template>

<script>
export default {
    name: 'cardForm',
    props:{
        cardInfo: Object
    },
    data() {
        return {
            
                cardNumber: '',
                cardHolder: '',
                validDate: {Month:'', Year: ''},
                cvv: '',
                bank: '',
                logo: 'Bitcoin',
                theme: '',
                //valid: false,
                validate: {
                    cardNumber: true,
                    cardHolder: true,
                    month: true,
                    year: true,
                    bank: true,
                    cvv: true
                    }
            
        }
    },
    methods: {
        // Förhindrar bokstäver i card number input
        // Förhindrar mer än 16 siffror 
        isNumber: function(e) {
            let charCode = (e.which) ? e.which : e.keyCode;
                
            if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
                e.preventDefault() 
            } else if(this.cardNumber.length >= 16 && e.keyCode !== 8 && e.keyCode !== 9){
                e.preventDefault()
            } 
        },
        // Förhindrar bokstäver i month input
        // Förhindrar mer än 2 siffror 
        isMonth: function(e) {
            let charCode = (e.which) ? e.which : e.keyCode;

            if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
            e.preventDefault()
        } else if (this.validDate.Month.length >= 2 && e.keyCode !== 8 && e.keyCode !== 9){
                e.preventDefault()
            }
        },
        // Förhindrar bokstäver i Year input
        // Förhindrar mer än 2 siffror 
        isYear: function(e) {
            let charCode = (e.which) ? e.which : e.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
            e.preventDefault()
        } else if (this.validDate.Year.length >= 2 && e.keyCode !== 8 && e.keyCode !== 9){
                e.preventDefault()
            }
        },
        // Förhindrar bokstäver i CVV input
        // Förhindrar mer än 3 siffror 
        isCVV: function(e) {
            let charCode = (e.which) ? e.which : e.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
            e.preventDefault()
        } else if (this.cvv.length >= 3 && e.keyCode !== 8 && e.keyCode !== 9){
                e.preventDefault()
            }
        },
        // Förhindrar siffror i Cardholder Name input
        handleInputname(e){
            
            let keyCode = (e.keyCode ? e.keyCode : e.which);
           
           if(keyCode > 47 && keyCode < 58) {
               e.preventDefault()
           }          
        },
        // Uppdaterar kortet med de inputs användaren har angett
        addCard(){
            
            let newCard = {cardNumber: this.cardNumber, cardHolder: this.cardHolder, valid: this.validDate, cvv: this.cvv, bank: this.bank, logo: this.logo, theme: ''}
         
            this.checkBank(newCard)
            this.$emit('NewCard', newCard)

        },
        // Skickar alla inputs till parent när man trycket på "ADD CARD"
        addCardToStack(){
        
            let card = {cardNumber: this.cardNumber, cardHolder: this.cardHolder, valid: this.validDate, cvv: this.cvv, bank: this.bank, logo: this.logo}
            
            this.checkBank(card)
            this.checkInput()
             
            if(this.checkInput){
                this.$emit('AddedCard', card)
            }
        },
        // Byter färg och logo för kortet beroende på vilken bank som är vald
        checkBank(card){
            
            if (this.bank == 'BITCOIN INC') {
                card.logo = 'Bitcoin'
                card.theme = 'orange'
            }else if (this.bank === 'NINJA BANK'){
                card.logo = 'Ninja'
                card.theme = 'black'
            }else if (this.bank == 'BLOCK CHAIN INC'){
                card.logo = 'Chain'
                card.theme = 'purple'
            }else if (this.bank == 'EVIL CORP'){
                card.logo = 'Evil'
                card.theme = 'red'
            }
        },
        // Byter mellan true och false på olika värden i this.validate
        // Är alla värden true så Byts valid från false till true
        
        checkInput() {
               
               if(this.cardNumber.length < 16 ){
                   this.validate.cardNumber = false
               } else {
                   this.validate.cardNumber = true
               }

               if(this.cardHolder == '') {
                   this.validate.cardHolder = false
               } else {
                   this.validate.cardHolder = true
               }
               
               if(this.validDate.Month.length < 2 || this.validDate.Month === 'MM'){
                   this.validate.month = false
               } else {
                   this.validate.month = true
               }

               if(this.validDate.Year.length < 2 || this.validDate.Year === 'YY'){
                   this.validate.year = false
               } else {
                   this.validate.year = true
               }

                if(this.cvv.length < 3) {
                   this.validate.cvv = false
               }else {
                   this.validate.cvv = true
               }

               if(this.bank === ''){
                   this.validate.bank = false
               } else {
                   this.validate.bank = true
               }

                // **flyttad till computed **
               /* if(this.cardNumber.length < 16 || this.cardHolder == '' || this.validDate.Month < 2 || this.validDate.Year < 2 || this.cvv < 2  || this.bank == ''){
                   this.valid = false
               } else {
                   this.valid = true
               }*/
            
        },
    },
    computed: {
        // När alla värden är true och kommer då skicka tillbaka true
        inputFormValidated: function(){
            if(this.cardNumber.length < 16 || this.cardHolder == '' || this.validDate.Month < 2 || this.validDate.Year < 2 || this.cvv < 2  || this.bank == ''){
                return false
            } else {
                return true
            }
        }
        
    }
}
</script>

<style lang="scss" scoped>
    form {
        margin-top: 3rem;
    
        label {
            
        font-family: PT Mono;
        font-style: normal;
        font-weight: normal;
        font-size: 12px;
        line-height: 13px;
        display: flex;

        color: rgba(0, 0, 0, 0.8);
        }

        input {
            margin: 0 0 1rem 0;
        }

        .full {
            
            width: 22rem;
            height: 3.5rem;
            
            border: 1px solid rgba(0, 0, 0, 0.8);
            box-sizing: border-box;
            border-radius: 8px;

            
        }
        .half {
            
            width: 10rem;
            height: 3.5rem;
            
            border: 1px solid rgba(0, 0, 0, 0.8);
            box-sizing: border-box;
            border-radius: 8px;

            
        }
        
        .half-wrapper {
            display: flex;
            justify-content: space-between;
        }
        .wrapper{
            width: 10rem;

            .date {
                margin: 0;
                width: 45%;
                height: 95%;
                border: 0;
            }
        }
        .add-card {

            margin-top: 2.5rem;
            width: 22rem;
            height: 3.5rem;
            background: #000000;
            border-radius: 8px;
            display: flex;
            justify-content: center;
            align-items: center;

            p {
                
                font-family: PT Mono;
                font-style: normal;
                font-weight: bold;
                font-size: 22px;
                line-height: 25px;
                

                color: #FFFFFF;
            }
        }
    .missing-input {
        border: 3px solid red;
    }
    }

</style>
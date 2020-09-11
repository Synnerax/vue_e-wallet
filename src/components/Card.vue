<template>
        <section>
        <article @click="activeCard" v-bind:class="cardInfo.theme">
            <img src="@/assets/blipNchip.svg" class="blip" alt="">
            
            
            <img  :src="bankIcon" class="bank-icon" alt="">
            
            <p class="card-number" :class="{'white-text' : textColor, 'black-text' : !textColor}">
                {{NumberSection.slice(0, 4)}} 
                {{NumberSection.slice(4, 8)}} 
                {{NumberSection.slice(8, 12)}} 
                {{NumberSection.slice(12, 16)}}
            </p>
                    
            
            
            <span class="ch-name" :class="{'white-text' : textColor, 'black-text' : !textColor}" >CARDHOLDER NAME</span>
            <span class="valid-date" :class="{'white-text' : textColor, 'black-text' : !textColor}">VALID THRU</span>

            <span class="cardholder" :class="{'white-text' : textColor, 'black-text' : !textColor}" >{{cardHolder}}</span>
            <span class="date" :class="{'white-text' : textColor, 'black-text' : !textColor}">
                {{month}}
                
                /
                {{year}}
                </span>
            
        
        </article>
        </section>
    
</template>

<script>

export default {
    name: 'singleCard',
    data(){
        return{
            cardEx: {
            cardHolder: 'FIRSTNAME LASTNAME',
            cardNumber: 'xxxxxxxxxxxxxxxx',
            valid: 
             {
              Month:'MM',
              Year: 'YY'
             },
            logo: 'Bitcoin'
          },
        }
    },
    props: {
        cardInfo: {
            cardHolder: String,
            cardNumber: String,
            valid: Object,
            bank: String,
            logo: String,
            theme: String,
            index: Number
        }
        
    },
    computed: {
        // Byter Färj på texten på alla kort förutom 'Bitcoin inc' eller om ingen bank är vald 
        textColor: function(){
            if (this.cardInfo.bank == '' || this.cardInfo.bank == 'BITCOIN INC'){
                return false
            } else {
                return true
            }
        },
        // Visar 'FIRSTNAME LASTNAME' på kortet om man inte skrivit in ett namn
        // lägger på .tpUpperCase() när man skriver in något
        cardHolder: function() {
            if(this.cardInfo.cardHolder == ''){
                return this.cardEx.cardHolder
            }else {
                return this.cardInfo.cardHolder.toUpperCase()
            }
        },
        // visar 'XXXX XXXX XXXX XXXX' om man inte skrivit in ett kortnummer
        NumberSection: function() {
            
            if(this.cardInfo.cardNumber == ''){
                return this.cardEx.cardNumber
            }else {
                return this.cardInfo.cardNumber
            }
        },
        // Visar MM för månad om ingen månad är vald
        month: function() {
            if(this.cardInfo.valid.Month == ''){
                return this.cardEx.valid.Month
            }else {
                return this.cardInfo.valid.Month
            }
        },
        //Visar YY för år om inget år är valt
        year: function() {
            if(this.cardInfo.valid.Year == ''){
                return this.cardEx.valid.Year
            }else {
                return this.cardInfo.valid.Year
            }
        },
        // visar bank logo för den bank som är vald, visar bitcoin logo om ingen bank är vald
        bankIcon: function() {
            let bank = this.cardInfo.logo
            if(bank == ''){
                    return require('@/assets/'+ this.cardEx.logo + '.svg')

                } else 
                return require('@/assets/' + bank +'.svg')
            }

    },
    methods: {
        // tar kortets :key value och skickar vidare det till sin parent 
        activeCard(){
            this.cardInfo.index = this.$vnode.key
            this.$emit('activeCard', this.cardInfo)
        },
        
        }
    
}
</script>

<style lang="scss" scoped>
    section {


        &:last-child{
            margin-bottom: 7rem;
        }
        article {
            background: linear-gradient(248.3deg, rgba(255, 255, 255, 0.24) 0%, rgba(255, 255, 255, 0) 100%), #D0D0D0;
            box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
            border-radius: 8px;
            width: 22rem;
            height: 15rem;
            display: flex;
            flex-wrap: wrap;

            .blip {
                margin: 15px 0 0 20px;;
            }

            .bank-icon {
                margin: 28px 25px 0 auto
            }

            .card-number {
                margin: 0;
                width: 22rem;
                height: 2rem;

                display: flex;
                align-items: center;
                justify-content: center;


                font-family: PT Mono;
                font-style: normal;
                font-weight: normal;
                font-size: 29px;
                line-height: 32px;
                letter-spacing: 4px;

                //color: #000000;
            }

            .ch-name, .valid-date {
                font-family: PT Mono;
                font-style: normal;
                font-weight: normal;
                font-size: 12px;
                line-height: 13px;
                height: 1rem;
                /* color: rgba(0, 0, 0, 0.8);*/
            }
            
            .ch-name, .cardholder {
                width: 13rem;
                margin-left: 16px;
                text-align: start;
            }

            .valid-date, .date {
                width: 6rem;
                
                text-align: end;

            }

        }
            .black {
                background: linear-gradient(248.3deg, rgba(255, 255, 255, 0.15) 0%, rgba(255, 255, 255, 0) 100%), #222222;
            }
            .orange {
                background: linear-gradient(248.04deg, rgba(255, 255, 255, 0.15) 0%, rgba(255, 255, 255, 0) 99.07%), #FFAE34;
            }
            .purple {
                background: linear-gradient(248.52deg, rgba(0, 0, 0, 0.15) 1.49%, rgba(0, 0, 0, 0) 100%), #8B58F9;
            }
            .red {
                background: linear-gradient(248.3deg, rgba(0, 0, 0, 0.16) 0%, rgba(0, 0, 0, 0) 100%), #F33355;
            }
            .white-text {
                color: #FFFFFF;
            }
            .black-text {
                color: #000000;
            }
    }
</style>
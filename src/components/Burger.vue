<template>
  
  <div class = "Burger">
            <h3>{{ burger.name }}</h3>
            <img v-bind:src="burger.url" style=width:170px >    
            <ul>
                <span v-if="burger.lactose==true"> <li>Contains <span id="Allergies">LACTOSE</span></li> </span>
                <span v-if="burger.gluten==true"> <li>Contains <span id="Allergies">GLUTEN</span></li> </span>
                <li>{{ burger.kcal }} kcal</li>
                Amount:
                <button class="Button" v-on:click= "negcount">
                -
                </button>
                {{ amountOrdered }}
                <button class="Button" v-on:click= "poscount">
                +
                </button>     
            </ul>
  </div>

</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },

data: function () {
  return {
    amountOrdered: 0,
  }
},
methods: {
  poscount(){
    this.amountOrdered= this.amountOrdered+1
    this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
                              );
  },
  negcount(){
    if ((this.amountOrdered>0)) {
      this.amountOrdered= this.amountOrdered-1
    }
  },
  addToOrder: function (event) {
    this.orderedBurger[event.name] = event.amount;
    },
}
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.Burger {
    background-color: rgb(109, 53, 53);
    color: #fff;
    border-radius: 5px;
    font-size: 100%;
    grid-template-columns: 300px 300px 300px;
    grid-column: indexOf(Hamburgers);
    grid-row: 2;
    padding-left: 75px;
    border: 1px solid #fcfbfb;

}

.c {
    grid-column: 3 ;
    padding-left: 75px;
    border: 1px solid #fcfbfb;

}

#Allergies{
    font-family: "arial";
    font-weight: bold;
    color:rgb(231, 231, 231);
    
}
</style>

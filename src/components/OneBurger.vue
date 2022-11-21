<template>
  <div>
      <h3> {{ burger.name }} </h3>
      <img :src=burger.img style="width: 200px">
      <ul>
        <section class="ingredients">
          <li> {{burger.kCal}} kCal </li>
          <li v-for="ingredient in burger.ingredients" v-bind:key="burger.name+ingredient"> 
            Contains <span id="ingred"> {{ingredient}} </span> 
          </li>   
        </section>
      </ul>
      <p> Amount: 
        <button id="removingBurger" type="removeBurger" v-on:click="removeBurger" style="width: 30px"> - </button>
        {{amountOrdered}}
        <button id="addingBurger" type="addBurger" v-on:click="addBurger" style="width: 30px"> + </button>
      </p>
  </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object,
    },
    data: function () { 
      return {
        amountOrdered: 0,
    }
  },
  methods: {
    addBurger: function () { 
      this.amountOrdered += 1;
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
      );
    },
    removeBurger: function () { 
      if (this.amountOrdered == 0) {
        return {}
      }
      this.amountOrdered -= 1;
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
      );
     }  
    },

  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

.ingredients {
   color: gray;

}

#ingred {
   font-weight: bold;
   color: white;
}
  
  </style>
  
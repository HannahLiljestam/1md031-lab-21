<template>

  <div>
  <div class="box" style ="float:left;" >
    <h3>{{burger.name}}</h3>
    <img v-bind:src=burger.img>
    <ul>
      <li>{{burger.kCal}}</li>
      <li>{{burger.lactose}}</li>
      <li>{{burger.gluten}}</li>
    </ul>

  </div>
    <div class="addBurgerButtons">
      Number of burgers: <br> {{ amountOrdered }} <br>
    <button v-on:click="minusBurgers" type="submit">
      –
    </button>

    <button v-on:click="plusBurgers" type="submit">
      +
    </button>

  </div>
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
    minusBurgers: function () {
      this.amountOrdered -= 1
      if (this.amountOrdered < 1) {
        this.amountOrdered = 0;
      }
    },
    plusBurgers: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    }
  }

}






</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

img{
  width: 360px; height: 260px;}


.wrapper {  /* wraps around the divs */
  display: grid;
  grid-gap: 2%;
  grid-template-columns: 30% 30% 30%;
  background-color: #444;
  color: #444;
}

body {     /* Font, capitalize, line height, color of text, font size and margins to the sides of the whole body */
  font-family: "Times New Roman", serif;
  text-transform: capitalize;
  line-height: normal;
  color: #2F4F4F;
  font-size: 16pt;
  margin: 50px; /*50px 120px 120px; */

}

#burgerinfo {  /* makes the burger part of the site have a black background and white text */
  background-color: #444;
  color: white;
  border: 20px solid #994d00;
}

button {  /* creates some space around the submit button */
  margin: 20px 20px 20px 0px;
  width: 50px;
  height: 50px;
  font-size: 30pt;

}

button:hover {  /* makes the submit button darker when hovered over, and also the cursor becomes a pointer */
  background-color: darkgray;
  cursor: pointer;
}
.addBurgerButtons {
  color:white;
  margin:auto;
  width:8em;
  font-size: 20pt;
}


</style>

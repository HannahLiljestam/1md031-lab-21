

<template>

  <header class="header-class">
    <h1>Welcome to <br> Hannah's Burgers!</h1>
  </header>
  <main>
    <section id="burgerinfo">
      <h1 class="title">BURGER SELECTION</h1>
      <h3 class="subtitle">Select one of our yummy burgers below to get started with your order.<br>All burgers available with beef, chicken or plantbeef.</h3>

      <div>
        <div class="wrapper">
          <Burger v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-on:orderedBurger="addToOrder($event)"/>
        </div>
      </div>



    </section>

    <section id="contact" style="clear:left">
      <h2 class="customer-info-title">CUSTOMER INFORMATION</h2>
      <h4 class="customer-info-sub-title">Fill in the following information to finish your order.</h4>


      <form>
        <p>
          <label for="name">Full Name</label><br>
          <input type="text" id="name" v-model="fn" required="required" placeholder="Full name">
        </p>

        <p>
          <label for="email">Email</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>

<!--        <p>
          <label for="Street name">Street Name</label><br>
          <input type="text" id="Street name" v-model="ln" placeholder="Street name">
        </p>

        <p>
          <label for="number">House Number</label><br>
          <input type="number" id="number" v-model="number" required="required" placeholder="House number">
        </p> -->

        <p>
          <label for="payment">Choose a method of payment</label>
          <select id="payment" v-model="rcp">
            <option selected>Credit card</option>
            <option>Klarna</option>
            <option>Swish</option>
          </select>
        </p>

        <p>Provide your gender:</p>
        <div>
          <input type="radio" id="Female" v-model="gender" value="Female"
                 checked>
          <label for="Female">Female</label>

          <input type="radio" id="Male" v-model="gender" value="Male">
          <label for="Male">Male</label>

          <input type="radio" id="Non-binary" v-model="gender" value="Non-binary">
          <label for="Non-binary">Non-binary</label>

          <input type="radio" id="Other" v-model="gender" value="Other">
          <label for="Other">Other</label>
        </div>


        <div>
          <h3 class="subtitle">Mark your preferred delivery location on the map below.</h3>
        </div>

        <div class="mapWrapper">
        <div id="map" v-on:click="setLocation">
          <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" >
            T
          </div>
        </div>
        </div>


      </form>
    </section>

    <button v-on:click="submitInfo" type="submit">
      <img src="">
      Place my order.
    </button>

  </main>
  <hr>
  <footer>
    <h6>&copy; Hannah's Burgers</h6>
  </footer>


</template>






<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

//Object Constructor Function

/*function MenuItem(pn, url, kCal, glu, lac) {
  this.productName = pn; // The *this* keyword refers to the object itself
  this.burgerURL = url;
  this.calories = kCal;
  this.gluten = glu;
  this.lactose = lac;
  //this.name = function() {
  //  return this.productName;
  //};
} */

// Array of burgers


/*
const burgers = [ new MenuItem('Hannahs tasty burger', 'https://cached-images.bonnier.news/bnl01/standard-article/466a6b03-1dc8-4c53-9b38-062066eb7843/ed7990bd-913c-4eca-a07c-db9d76aef545/16x9/0/original.jpg', '1000 calories', 'Contains gluten','Contains lactose'),
  new MenuItem('Hannahs yummy burger', 'https://www.edibleorlando.com/wp-content/uploads/Screenshot-2019-05-23-17.21.08.png', '1500 calories', 'Contains Gluten', 'Contains lactose'),
  new MenuItem('Hannahs delicious burger', 'https://www.burgerdudes.se/wp-content/uploads/2019/11/groundburger_timeout-620x380.jpg', '1200 calories', 'Contains Gluten', 'Contains lactose')  ];
*/



export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fn: "",
      em: "",
      /*                ln: "",
                number:"",*/
      gender: "",
      rcp: "",
      amountOrdered: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      },
      personalInformation:""


      //{name: "Hannah's tasty burger", kCal: 1000, img: "https://cached-images.bonnier.news/bnl01/standard-article/466a6b03-1dc8-4c53-9b38-062066eb7843/ed7990bd-913c-4eca-a07c-db9d76aef545/16x9/0/original.jpg"},
      //     {name: "Hannah's yummy burger", kCal: 1200, img: "https://www.edibleorlando.com/wp-content/uploads/Screenshot-2019-05-23-17.21.08.png"},
      //      {name: "Hannah's delicious burger", kCal: 1500, img: "https://www.burgerdudes.se/wp-content/uploads/2019/11/groundburger_timeout-620x380.jpg'"}]

      // [ {name: "small burger", kCal: 250},
      //         {name: "standard burger", kCal: 450},
      //         {name: "large burger", kCal: 850}
      //       ]
    }
  },
  methods: {
    submitInfo: function () {
      this.personalInformation = {
        name: this.fn,
        email: this.em,
        /*          this.ln,
        this.number,*/
        gender: this.gender,
        payment: this.rcp
      }
      console.log(
          this.personalInformation,
          this.amountOrdered,
          this.orderedBurgers,
      )
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: this.location,
        orderItems: this.orderedBurgers,
        personalInformation: this.personalInformation
      })

    },


    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },

    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },



/*    addOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;

      /!*var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top}; *!/

    },*/

    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };

      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y,
      };
    },
  },
};

</script>

<style>

@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

  #map {
    width: 1720px;
    height: 1078px;
    margin: 0;
    padding: 0;
    background: url("/img/polacks.jpg");
    background-repeat: no-repeat;
    position: relative;
    cursor: crosshair;
  }

#map div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:30px;
  height:20px;
  font-size: 12pt;
  text-align: center;
}

  body {     /* Font, capitalize, line height, color of text, font size and margins to the sides of the whole body */
    font-family: "Times New Roman", serif;
    text-transform: capitalize;
    line-height: normal;
    color: #2F4F4F;
    font-size: 16pt;
    margin: 50px; /*50px 120px 120px; */
  }

  .ingredients {
    color: #ff5500;
  }

  .allergies {           /* Writes allergies in bold font */
    font-weight: bold;
  }

  #burgerinfo {  /* makes the burger part of the site have a black background and white text */
    background-color: #444;
    color: white;
    border: 20px solid #994d00;
  }

  button {  /* creates some space around the submit button */
    margin: 20px 20px 20px 0px;
  }

  button:hover {  /* makes the submit button darker when hovered over, and also the cursor becomes a pointer */
    background-color: darkgray;
    cursor: pointer;
  }

  .wrapper {  /* wraps around the divs */
    display: grid;
    grid-gap: 2%;
    grid-template-columns: 30% 30% 30%;
    background-color: #444;
    color: #444;

  }

  .box{     /* wraps around each div a, b and c */
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 40px;
    font-size: 150%;
  }

  .title {     /* Connected to Burger Selection */
    text-align: center;
    padding-top: 50px;
    margin: 0;
    padding: 50px 0;
    color: white;
    text-transform: uppercase;
    font-size: 35pt;
  }
  .subtitle {    /* Connected to subtitle select burger */
    text-align: center;
    text-transform: uppercase;

  }

  .header-class {       /* the header Welcome to Hannah's burgers */
    background: url("https://wallpapersmug.com/download/2048x1152/48de0c/clouds-orange-sky.jpg");
    opacity: 1;
    background-position: center;
    font-size: 30pt;
    border: 20px solid #cc6600;
    text-align: center;
    padding-top: 50px;
    margin: 0;
    color: white;
  }


  #contact {
    border: 20px solid #cc6600;
    padding: 40px;
    background-color: #737373;
    color: white;
  }

  .customer-info-title {
    font-size: 35pt;
  }

  .customer-info-sub-title {
    text-transform: capitalize;
  }

.mapWrapper {
  overflow: scroll;
  font-size: 30pt;
  color: black;
}






</style>




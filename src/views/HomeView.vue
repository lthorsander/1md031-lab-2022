<template>
  <div> 
  <header>
        <img id="headerImg" src="https://www.mural-wallpaper.com/wp-content/uploads/2021/08/S-NA17.jpg">
        <h1>Welcome to BurgerOnline</h1>
  </header>
    <main>
        <section id="selectBurger">
            <h2> Select Burger</h2>
            <p> This is where you execute burger selection </p>
            <div class="wrapper">
                <Burger v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-bind:src="burger.img"
                  v-bind:key1="burger.ingredients"
                  v-on:orderedBurger="addToOrder($event)"
                />
            </div>
        </section>
        <section id="customerInfo">
            <h2> Customer Information</h2>
            <p> This is where you provide necessary information</p>
            <h3> Delivery Information:</h3>
            <section id="FullName">
              
                  <p>
                        <label for="fullname">Full name</label><br>
                        <input type="text" id="fullname" v-model="name" required="required" placeholder="First- and Last name">
                    </p>
                    <p> 
                        <label for="mail">E-mail</label><br>
                        <input type="text" id="mail" v-model="mail" required="required" placeholder="E-mail address">
                    </p>
                    <p> 
                        <label for="payment">Payment options </label><br>
                        <select id="payment" v-model="pay" required>
                            <option value="Credit card"> Credit card </option>
                            <option value="Swish"> Swish </option>
                            <option value="Apple pay"> Apple pay </option>
                            <option value="Cash"> Cash </option>

                        </select>
                    </p>
                    <p> 
                    <legend>Gender</legend>
                    <input type="radio" id="male" value="male" v-model="gender">
                    <label for="male">Male</label><br>
                    <input type="radio" id="female" value="female" v-model="gender">
                    <label for="female">Female</label><br>
                    <input type="radio" id="nonbinary" value="nonbinary" v-model="gender">
                    <label for="nonbinary">Non-binary</label><br>
                    <input type="radio" id="undisclosed" value="undisclosed" v-model="gender" checked>
                    <label for="undisclosed">Undisclosed</label>
                    </p>
                    <p>Please indicate point of delivery:</p>
                    <div id="mapscroller">
                      <div id="map" v-on:click="setLocation">  
                        <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
                          T
                        </div>
                      </div>
                    </div>

            </section>
        </section>
        <br> 
        <button id="submitOrder" type="submit" v-on:click="submit">
            <img src="https://cdn-icons-png.flaticon.com/512/5360/5360883.png" style="width: 10px">
            Place my order!
        </button>

    </main>
    <footer>
        <hr>
        &copy; 2018 Hypotethical Burgers Inc.
    </footer>
    
  
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/* function MenuItem (name, img, kCal, ingredients){
  this.name = name;
  this.img = img;
  this.kCal = kCal;
  this.ingredients = ingredients;
}*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      name:'',
      mail: '',
      street: '',
      house: '',
      pay: 'Credit card',
      gender: 'undisclosed',
      orderedBurgers: {},
      location: { x: 0,
            y: 0
          },
    }
  },

  methods: {

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    addOrder: function (event) {
      console.log(event)
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                y: event.currentTarget.getBoundingClientRect().top};

      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;         
    },

    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                y: event.currentTarget.getBoundingClientRect().top};

      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y; 
    },

    submit: function () {
      
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x ,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                form: {name: this.name, mail: this.mail, pay: this.pay, gender: this.gender}
                          }
             );
    }
  }
}
</script>

<style>
.wrapper{
  display: grid;
  grid-template-columns: 33% 33% 33%;
  background-color: black;
  padding: 10px;
}

#mapscroller {
  overflow: scroll;
  width: 1330px;
  height: 300px;
}

#map {
  position: relative;
  background: url("/Users/lovisathorsander/Documents/Trean/GSP/1md031-lab-2022/public/img/polacks.jpg");
  width: 1920px;
  height: 1078px;
  cursor: pointer;
  margin: 0px
}

  @import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

#selectBurger {
   background-color: black;
   color: white;
   margin: 5px 5px;
   padding: 5px 10px;
   border: dashed white;
}

button:hover {
   background-color: white;
   cursor: pointer;

}

#customerInfo {
   margin: 5px 5px;
   padding: 5px 10px;
   border: dashed black;
}

button {
   margin: 0px 10px 10px 10px;
   background-color: lightgray;
   color: black;
}

div {
   margin: 10px;
   padding: 0px;
}

header {
   margin: 0px 5px;
   height: 300px;
   overflow: hidden;
}

#headerImg {
   opacity: 0.85;
   width: 100%;
   height: auto;
}

h1 {
   color: white;
   position: absolute;
   padding: 40px 450px;
   margin-top: -740px;
   font-size: 200%;
}

  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    margin: 0px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>
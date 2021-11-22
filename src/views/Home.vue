<template>
   <body class="Wrapped">
    <section id ="Header" >
      <header>
         <h1>Welcome to Elsashamburgers</h1>
      </header>
    </section>

      <main>
        <section class="Hamburgers">
            <div id= "Selection">
            <h2>SELECT BURGER</h2>
            <nav> Choose the burger you like:</nav>
            </div>

            <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
           

        </section>
       
      

        <section class="Information">
            <h3>COSTUMER INFORMATION </h3>
            <nav> Provide necessery information  </nav>
            <h4>Delivery information:</h4>
            <p>
                <label for="fullname">Full name</label><br>
                <input type="text" id="fullname" v-model="fn" required="required" placeholder="First- and Lastname">
            </p>

            <p>
                <label for="emailadress">E-Mail</label><br>
                <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
            </p>
            <!--
            <p>
                <label for="streetname">Street</label><br>
                <input type="text" id="streetname" v-model="sn" required="required" placeholder="Steet name">
            </p>
            <p>
                <label for="housenumber">House</label><br>
                <input type="number" id="housenumber" v-model="hn" required="required" placeholder="House number">
            </p> -->
            
            <p>
                <label for="Payment">Payment options: </label>
                <select id="Payment" v-model="pmt">
                    <option>Credit card</option>
                    <option>Swish</option>
                    <option>Paypal</option>
                    <option>Klarna</option>
                </select>
             </p>

             <p>Gender:</p>

             <form>
                <input type="radio" id="male" v-model="Gender" value="male">
                <label for="male">Male</label><br>
                <input type="radio" id="female" v-model="Gender" value="female">
                <label for="female">Female</label><br>
                <input type="radio" id="cat" v-model="Gender" value="Cat" checked>
                <label for="cat">Other</label>
              </form> 

              <div class = "scrollmap">
                <div id="map" >
                 click here
                <div id="map" v-on:click="setLocation">
                  <div id="orderDot" v-bind:style="{ left: this.location.x + 'px', 
                                                        top: this.location.y + 'px' }">
                    T
                  </div>
                </div>
              </div>
              </div>

              <div id="Knapp">
                <button type="submit" class="Button" v-on:click= "placeOrder">
                    <img src=https://static5.depositphotos.com/1000270/486/i/600/depositphotos_4869272-stock-photo-cute-bengal-kitten-looks-pensively.jpg style=width:30px>
                    Place order 
                </button>
              </div>
        </section>
      </main>
      
      <div id="Finalword">
        <hr>
          <footer>
              <h3> &copy; Hypotetical Burgers Inc.</h3>
          </footer>
      </div>
   </body>
</template>

<script>

import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json';


const socket = io();



/*
function MenuItem(nm,url,kcal,gluten,lactose){
  this.name = nm;
  this.url = url;
  this.kcal = kcal;
  this.gluten = gluten;
  this.lactose = lactose;
}
*/

/*const burger1 = new MenuItem("Cheeseburger","https://www.mathiaszachau.com/wp-content/uploads/2016/05/saftiga-hamburgare-e1463035643226.jpg",400,true,true);
console.log(burger1);

const burger2 = new MenuItem("Halloumi","https://ingmar.app/blogg/wp-content/uploads/2020/11/Vegoburgare.jpg",300,true,false);
console.log(burger2);

const burger3 = new MenuItem("Greek","https://res.cloudinary.com/coopsverige/image/upload/w_1200,h_1200/v1485261961/184383.jpg",500,false,true);
console.log(burger3);*/

const burger1 = menu[0];
const burger2 = menu[1];
const burger3 = menu[2];

const Hamburgers = [burger1, burger2, burger3];

console.log(Hamburgers);



export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      orderedBurger: {},
      location: { x: 0,
            y: 0 },
      burgers: Hamburgers,
      fn: "",
      em: "",
      Gender: "Other",
      pmt: "Credit card",
      orderNumber: 0,
    }
  },
  methods: {

    placeOrder: function() {
     /* socket.emit("placeOrder",{ orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y},
                                orderItems: ["Beans", "Curry"]
                              },
                 );*/

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                personalInfo: { Name: this.fn,
                                                Email: this.em,
                                                Payment: this.pmt,
                                                Gender: this.Gender},
                                orderItems: this.orderedBurger
                              }
                 );  
      console.log(this.fn)
      console.log(this.em)
      console.log(this.pmt)
      console.log(this.Gender)
      console.log(this.orderedBurger)
        
    },

    addToOrder: function (event) {
    this.orderedBurger[event.name] = event.amount;
    }, 

    getOrderNumber: function () {
      /*return Math.floor(Math.random()*100000);*/
      this.orderNumber = this.orderNumber+1
      return this.orderNumber;
    },

    /*
   addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );  
                 
    },*/
      
    setLocation: function (event){    
     let offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                    
      this.location.x = event.clientX -10 - offset.x 
      this.location.y = event.clientY +10 - offset.y
     }
}
}

</script>

<style>

@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");

@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';




/* Så här kommenterar man */

body {
    font-family: "Monaco", monospace;
    font-size: 12pt;
    font-style: normal;
}

a, strong, em {
    color: #0ca3e9;
}

p.info,
#login p {
    width: 80%;
    margin: 1vw auto;
}

 
 #testId {

    text-transform: capitalize;
 }

  .Hamburgers {
    background-color: rgb(70, 17, 17);;
    color: white;

    display: grid;
    grid-gap: 20px;
    grid-template-columns: 300px 300px 300px;
    padding-left: 20px;
    padding-bottom: 20px;
    background-color: rgb(83, 36, 41);
    color: rgb(255, 255, 255);
    padding-left: 20 px;
    padding-bottom: 50 px;
    border: 5px dotted #fcfbfb;
 }


 .Button{
    background-color: white;
    transition-duration: 0.1s;
    font-family: "Monaco", monospace;
    text-transform: capitalize;
    
 }
 #Knapp{
    padding-top: 20px;
    padding-bottom: 20px;
 }

 .Button:hover {
    background-color: rgb(15, 175, 95);
    cursor: pointer;
 }

 .Information {
    background-color: rgb(0, 0, 0);;
    color: white;
    border: 5px dotted #fcfbfb;
    padding-left: 20px;

}

.Wrapped {
   background-color: black;
   padding-left: 20px;
   padding-right: 20px;
   grid-gap: 15px;
   grid-row-gap: 30px;
   display: grid;
}

#Header {
    background-image: url(https://static.thatsup.co/content/img/place/t/u/tugg-burgers-2.jpg);
    color: white;
    
    font-family: "Monaco", monospace;
    color: #fff;
    overflow:hidden;
    text-shadow: 4px 4px 4px #000000;
    font-size: 30pt;
    padding-top: 100px;
    text-align: center;
    text-transform: uppercase;
    width: 100%;
    height: 250px; 
}

#Selection {
    padding-left: 20px;
}

.Burger {
    background-color: rgb(109, 53, 53);
    color: #fff;
    border-radius: 5px;
    font-size: 100%;

}
.a {
    grid-column: 1  ;
    padding-left: 75px;
    border: 1px solid #fcfbfb;

}

.b {
    grid-column: 2 ;
    padding-left: 50px;
    border: 1px solid #fcfbfb;

}

.c {
    grid-column: 3 ;
    padding-left: 75px;
    border: 1px solid #fcfbfb;

}

.scrollmap{
  width: 600px;
  height: 400px;
  overflow:scroll;
  position:relative;
}

#Finalword {
    color: white;
}

#Allergies{
    font-family: "arial";
    font-weight: bold;
    color:rgb(231, 231, 231);
    
}




  #map {
    background: url("/img/polacks.jpg");
    width: 1920px;
    height: 1728px;
    background-color: red;

  }

  #orderDot{
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
  }
</style>

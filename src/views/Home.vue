<template>

  <section id="window">

    <section id="page">

      <header>
        <h1>Welcome to Bad Burger</h1>
      </header>

      <menu>
        <h2>Select Burger</h2>
        This is where you execute burger selection.<br><br>

        <div class="menugrid">
          <div>
            <Burger v-for="burger in burgers"
                    v-bind:burger="burger"
                    v-bind:key="burger.name"
                    v-on:orderedBurger="addToOrder($event)"/>
          </div>
        </div>
      </menu>

      <main>
        <h2>Customer Information</h2>
        This is where you provide necessary information.

        <form id="formWrapper">
          <p>
            <label for="fullname">Fullname</label><br>
            <input id="fullname" v-model="fullname" required="required" placeholder=" First- and Last name" type="text">
          </p>
          <p>
            <label for="email">E-mail</label><br>
            <input id="email" v-model="email" required="required" placeholder=" E-mail address" type="email">
          </p>
          <p>
            <label for="gender">Gender</label><br>
            <input id="genderNo" v-model="gender" type="radio" value="no"/>
            <label for="genderNo" checked="checked">Prefer not to say</label>
            <br />
            <input id="genderM" v-model="gender" type="radio" value="male"/>
            <label for="genderM">Male</label>
            <br />
            <input id="genderF" v-model="gender" type="radio" value="female"/>
            <label for="genderF">Female</label>
            <br />
          </p>
          <p>
            <label for="Payment">Payment options</label><br>
            <select id="Payment" v-model="payment" autocomplete="off">
              <option select="selected">Swish</option>
              <option>Credit Card</option>
              <option>Paypal</option>
            </select>
          </p>
          <div id="mapText">
            Indicate point of delivery on the map below:
          </div>
          <button type="submit"
                  id="orderBtn"
                  v-on:click="orderBtnListener">
            Order Now
          </button>
        </form>

        <br />
        <section id="mapWrapper">
          <div id="map" v-on:click="setLocation">
            <div id="mapDot"
                 v-if="location.x != 0 && location.y != 0"
                 v-bind:style="{
                      left: location.x + 'px',
                      top: location.y + 'px',
                }">
              T
            </div>
          </div>
        </section>


      </main>

      <footer>
        <small>&copy; 2021 Bad Burger Inc.</small>
      </footer>

    </section>

  </section>

</template>



<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*
function menuItem(name, kCal, ingredients, lactose, gluten) {
  this.name = name;
  this.kCal = kCal;
  this.ingredients = ingredients;
  this.lactose = lactose;
  this.gluten = gluten;
}
*/


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers : JSON.parse(JSON.stringify(menu)),
      fullname:'',
      email:'',
      gender:'',
      payment:'',
      orderedBurgers:{},
      location: { x: 0,
                  y: 0
      }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function () {
      let mapBlock = document.getElementById('mapWrapper');
      let offset = {
        x: mapBlock.offsetLeft,
        y: mapBlock.offsetTop
      };
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: {
                                  x: this.location.x - offset.x,
                                  y: this.location.y - offset.y,
                                  name: this.fullname,
                                  email: this.email,
                                  gender: this.gender,
                                  payment: this.payment,
                                  orderedBurgers: this.orderedBurgers
                                }
                              },
                 );
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    orderBtnListener: function() {
      this.addOrder();
      console.log([
        this.fullname,
        this.email,
        this.gender,
        this.payment,
        this.orderedBurgers,
        this.location
      ])
    },
    setLocation: function(event) {
      this.location.x = event.clientX - 10;
      this.location.y = event.clientY - 10;
    }
  }
}
</script>

<style>
  /*Page config*/

  #window {
    background-color: #ffffff;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 2000 1500'%3E%3Cdefs%3E%3Crect stroke='%23ffffff' stroke-width='0.5' width='1' height='1' id='s'/%3E%3Cpattern id='a' width='3' height='3' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cuse fill='%23fcfcfc' href='%23s' y='2'/%3E%3Cuse fill='%23fcfcfc' href='%23s' x='1' y='2'/%3E%3Cuse fill='%23fafafa' href='%23s' x='2' y='2'/%3E%3Cuse fill='%23fafafa' href='%23s'/%3E%3Cuse fill='%23f7f7f7' href='%23s' x='2'/%3E%3Cuse fill='%23f7f7f7' href='%23s' x='1' y='1'/%3E%3C/pattern%3E%3Cpattern id='b' width='7' height='11' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%23f5f5f5'%3E%3Cuse href='%23s'/%3E%3Cuse href='%23s' y='5' /%3E%3Cuse href='%23s' x='1' y='10'/%3E%3Cuse href='%23s' x='2' y='1'/%3E%3Cuse href='%23s' x='2' y='4'/%3E%3Cuse href='%23s' x='3' y='8'/%3E%3Cuse href='%23s' x='4' y='3'/%3E%3Cuse href='%23s' x='4' y='7'/%3E%3Cuse href='%23s' x='5' y='2'/%3E%3Cuse href='%23s' x='5' y='6'/%3E%3Cuse href='%23s' x='6' y='9'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='h' width='5' height='13' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%23f5f5f5'%3E%3Cuse href='%23s' y='5'/%3E%3Cuse href='%23s' y='8'/%3E%3Cuse href='%23s' x='1' y='1'/%3E%3Cuse href='%23s' x='1' y='9'/%3E%3Cuse href='%23s' x='1' y='12'/%3E%3Cuse href='%23s' x='2'/%3E%3Cuse href='%23s' x='2' y='4'/%3E%3Cuse href='%23s' x='3' y='2'/%3E%3Cuse href='%23s' x='3' y='6'/%3E%3Cuse href='%23s' x='3' y='11'/%3E%3Cuse href='%23s' x='4' y='3'/%3E%3Cuse href='%23s' x='4' y='7'/%3E%3Cuse href='%23s' x='4' y='10'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='c' width='17' height='13' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%23f2f2f2'%3E%3Cuse href='%23s' y='11'/%3E%3Cuse href='%23s' x='2' y='9'/%3E%3Cuse href='%23s' x='5' y='12'/%3E%3Cuse href='%23s' x='9' y='4'/%3E%3Cuse href='%23s' x='12' y='1'/%3E%3Cuse href='%23s' x='16' y='6'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='d' width='19' height='17' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%23ffffff'%3E%3Cuse href='%23s' y='9'/%3E%3Cuse href='%23s' x='16' y='5'/%3E%3Cuse href='%23s' x='14' y='2'/%3E%3Cuse href='%23s' x='11' y='11'/%3E%3Cuse href='%23s' x='6' y='14'/%3E%3C/g%3E%3Cg fill='%23efefef'%3E%3Cuse href='%23s' x='3' y='13'/%3E%3Cuse href='%23s' x='9' y='7'/%3E%3Cuse href='%23s' x='13' y='10'/%3E%3Cuse href='%23s' x='15' y='4'/%3E%3Cuse href='%23s' x='18' y='1'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='e' width='47' height='53' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%2356CEFF'%3E%3Cuse href='%23s' x='2' y='5'/%3E%3Cuse href='%23s' x='16' y='38'/%3E%3Cuse href='%23s' x='46' y='42'/%3E%3Cuse href='%23s' x='29' y='20'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='f' width='59' height='71' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%2356CEFF'%3E%3Cuse href='%23s' x='33' y='13'/%3E%3Cuse href='%23s' x='27' y='54'/%3E%3Cuse href='%23s' x='55' y='55'/%3E%3C/g%3E%3C/pattern%3E%3Cpattern id='g' width='139' height='97' patternUnits='userSpaceOnUse' patternTransform='scale(35.3) translate(-971.67 -728.75)'%3E%3Cg fill='%2356CEFF'%3E%3Cuse href='%23s' x='11' y='8'/%3E%3Cuse href='%23s' x='51' y='13'/%3E%3Cuse href='%23s' x='17' y='73'/%3E%3Cuse href='%23s' x='99' y='57'/%3E%3C/g%3E%3C/pattern%3E%3C/defs%3E%3Crect fill='url(%23a)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23b)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23h)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23c)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23d)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23e)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23f)' width='100%25' height='100%25'/%3E%3Crect fill='url(%23g)' width='100%25' height='100%25'/%3E%3C/svg%3E");
    background-attachment: fixed;
    background-size: cover;
    font-family: 'Raleway';
    height: 97vh;
  }

  #page {
    display: grid;
    width: 90%;
    max-width: 1350px;
    height: 80%;
    margin: auto;
    grid-template: "header header"
                   "menu  main"
                   "footer  footer";
    grid-template-rows: auto 1fr auto;
    grid-template-columns: 1fr 1fr;
    grid-row-gap: 5px;
    grid-column-gap: 10px;

  }

  #page > header {
    grid-area: header;
    border: 3px solid skyblue;
    border-radius: 20px;
    text-align: center;
  }

  #page > menu {
    background-color: #fffffe;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.06), 0 10px 20px rgba(0, 0, 0, 0.12);
    grid-area: menu;
    border-top-left-radius: 20px;
    border-bottom-left-radius: 20px;
    padding: 20px;
    padding-left: 30px;
  }

  #page > main {
    background-color: #fffffe;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.06), 0 10px 20px rgba(0, 0, 0, 0.12);
    grid-area: main;
    border-top-right-radius: 20px;
    border-bottom-right-radius: 20px;
    padding: 20px;
    padding-right: 30px;
    margin-top: 16px;
    margin-bottom: 16px;
  }

  #page > footer {
    grid-area: footer;
    text-align: center;
    color: darkgrey;
  }

  h1 {
    text-align: center;
    font-size: x-large;
    margin: 20px;
  }

  h2 {
    text-align: center;
    padding-bottom: 10px;
    margin: 10px;
  }

  h4 {
    text-align: center;
    margin: 5px;
  }

  li {
    font-size: small;
  }

  /*Contact config*/

  #formWrapper {
    display: grid;
    grid-template-rows: auto auto auto;
    grid-template-columns: 1fr 1fr;
  }

  #mapText {
    margin: auto;
  }

  #orderBtn {
    background: white;
    border: none;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.12);
    width: 50%;
    height: 40px;
    display: table;
    margin: 0 auto;
    border-radius: 15px;
  }

  #orderBtn:hover {
    background: ghostwhite;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.12);
  }

  /*Map config*/

  #mapWrapper {
    width: 40vw;
    height: 40vh;
    overflow: scroll;
    scrollbar-width: none;
    border-radius: 20px;
    box-shadow: inset 0 0 30px 10px rgba(0, 0, 0, 0.2);
  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
    cursor: crosshair;
  }

  #mapDot {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    overflow:auto;
  }
</style>

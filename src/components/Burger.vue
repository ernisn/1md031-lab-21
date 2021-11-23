<template>
<div id="menuGrid">

  <div id="oneBurger"
       v-on:click="amountBtnListener(1)">

    <div>
      <img v-if="burger.name === 'Super Bad Burger'"
        src='../../public/img/super.png'
        alt="Super Bad Burger img"
        width=140
      >
      <img  v-else-if="burger.name === 'Chicken Bad Burger'"
        src='../../public/img/chicken.png'
        alt="Chicken Bad Burger img"
        width=140
      >
      <img v-else
        v-on:click="amountBtnListener(1)"
        src='../../public/img/vegan.png'
        alt="Vegan Bad Burger img"
        width=140
      >
    </div>

    <div>
      <ul>
        <h4>{{ burger.name }}</h4>
        <li>Only {{ burger.kCal }} kCal</li>
        <li>Ingredients: {{ burger.ingredients }}</li>
        <li v-if=!burger.lactose>
          ⭐ ️No Lactose!
        </li>
        <li v-if=!burger.gluten>
          ⭐ Gluten Free!
        </li>
      </ul>
    </div>

  </div>

<!--  <div> </div>-->

  <div id="amountGp">
    <button class="amoutBtn"
            type="submit"
            v-on:click="amountBtnListener(1)">
      +
    </button>
    <p v-if="amountOrdered == 0">Add</p>
    <p v-if="amountOrdered != 0">{{ amountOrdered }}</p>
    <button class="amoutBtn"
            type="submit"
            v-if="amountOrdered > 0"
            v-on:click="amountBtnListener(-1)">
      -
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
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    },
    delBurger: function () {
      this.amountOrdered -= 1;
      this.$emit('orderedBurger', {
            name: this.burger.name,
            amount: this.amountOrdered
          }
      );
    },
    amountBtnListener: function (change) {
      if (change == 1) {
        this.addBurger();
      } else {
        this.delBurger();
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  /*Menu config*/

  #menuGrid {
    display: grid;
    grid-template-columns: 10fr 1fr;
    user-select: none;
  }

  #oneBurger {
    display: grid;
    grid-template-columns: 1fr 3fr;
    align-items: center;
    background-color: #fffffe;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.12);
    border-radius: 20px;
    margin-top: 10px;
    margin-bottom: 10px;
    padding: 10px;
  }

  #oneBurger:hover {
    transform: scale(1.02);
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.12);
  }

  #amountGp {
    display: grid;
    grid-template-rows: auto auto auto;
    height: 100px;
    width: 30px;
    margin: auto;
    padding-left: 20px;
    text-align: center;
  }

  .amoutBtn {
    background: white;
    border: none;
    height:30px;
    width: 30px;
    box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
    border-radius: 15px;
  }

  .amoutBtn:hover {
    background: ghostwhite;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  }

</style>

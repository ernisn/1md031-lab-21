<template>

  <div class="menugrid">

    <div>
      <img
        v-if="burger.name === 'Super Bad Burger'"
        src='../../public/img/super.png'
        alt="Super Bad Burger img"
        width=130
      >
      <img
        v-else-if="burger.name === 'Chicken Bad Burger'"
        src='../../public/img/chicken.png'
        alt="Chicken Bad Burger img"
        width=130
      >
      <img
        v-else
        src='../../public/img/vegan.png'
        alt="Vegan Bad Burger img"
        width=130
      >
      <button type="submit"
              v-on:click="amountBtnListener(1)">
        +1
      </button>
      Amout: {{ amountOrdered }}
      <button type="submit"
              v-if="amountOrdered > 0"
              v-on:click="amountBtnListener(-1)">
        -1
      </button>
    </div>

    <div>
      <ul>
        <h4>{{ burger.name }}</h4>
        <li>Only {{ burger.kCal }} kCal</li>
        <li>Ingredients: {{ burger.ingredients }}</li>
        <li>Lactose: {{ burger.lactose }}</li>
        <li>Gluten: {{ burger.gluten }}</li>
      </ul>
      <br>
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

  .menugrid {
    display: grid;
    grid-template-columns: 1fr 3fr;
    align-items: center;
  }

</style>

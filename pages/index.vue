<template>
  <b-container fluid class="bv-example-row">
    <b-col>
      <!-- hearder -->
      <br />
      <h2 align="center">Cool Drink</h2>
      <hr />
      <!-- product -->
      <div>
        <b-row align="center">
          <div v-for="list in list.data" :key="list.data">
            <b-row align-h="center">
              <b-col md="12">
                <div>
                  <b-card style="width: 18rem">
                    <template #header>
                      <h4>{{ list.name }}</h4>
                    </template>
                    <b-card-img
                      :src="list.image"
                      img-alt="Image"
                      height="150"
                    ></b-card-img
                    ><br /><br />
                    <p class="in" v-if="list.in_stock == true">In Stock</p>
                    <p class="out" v-if="list.in_stock == false">
                      Out Of Stock
                    </p>
                    <p class="price">Price : {{ list.price }} Bath</p>
                  </b-card>
                </div>
              </b-col>
            </b-row>
          </div>
        </b-row>
      </div>
      <hr />
      <!-- paid -->
      <div align="center">
        <b-row>
          <b-col>
            <div v-if="change == null">
              <h3 align="center">Total moneys: {{ total }} Baths</h3>
              <br />
            </div>
            <div>
              <b-button v-on:click="add(+10)" variant="outline-primary"
                >10 bath</b-button
              >
              <b-button v-on:click="add(+5)" variant="outline-primary"
                >5 bath</b-button
              >
              <b-button v-on:click="add(+2)" variant="outline-primary"
                >2 bath</b-button
              >
              <b-button v-on:click="add(+1)" variant="outline-primary"
                >1 bath</b-button
              >
            </div>
          </b-col>
        </b-row>
      </div>
      <hr />
      <!-- select -->
      <div align="center">
        <b-row align-h="center">
          <b-col sm="12">
            <b-button variant="primary" v-on:click="select(15, total, 0)"
              >Pepsi Max</b-button
            >
            <b-button disabled v-on:click="select(20, total, 1)"
              >Pepsi Diet</b-button
            >
            <b-button variant="primary" v-on:click="select(15, total, 2)"
              >Coke</b-button
            >
            <b-button disabled v-on:click="select(20, total, 3)"
              >Coke Vanilla (S)</b-button
            >
            <b-button variant="primary" v-on:click="select(25, total, 4)"
              >Coke Vanilla (L)</b-button
            >
            <b-button variant="primary" v-on:click="select(13, total, 5)"
              >Mountain Dew</b-button
            >
            <b-button variant="danger" v-on:click="clear">Cancle</b-button>
          </b-col>
        </b-row>
      </div>
      <hr />
      <!-- change & refune & less -->
      <div v-if="refune != 0">
        <b-alert show variant="danger" align="center">
          Refune Money : {{ refune }} Baths
        </b-alert>
      </div>

      <div v-if="change != null">
        <h4 align="center">You have received : {{ drink }} 1 ea</h4>
        <b-alert show variant="danger" align="center">
          Change Money : {{ change }} Baht
        </b-alert>
      </div>

      <div v-if="lm != 0">
        <p align="center">Not enough money!</p>
        <b-alert show variant="danger" align="center">
          Refune Money : {{ lm }} Baths
        </b-alert>
      </div>
    </b-col>
  </b-container>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    let list = await $axios.$get(
      'https://www.mocky.io/v2/5c77c5b330000051009d64c9'
    )
    return { list }
  },

  data() {
    return {
      total: 0,
      change: null,
      refune: 0,
      lm: 0,
      drink: null,
      coin: 0,
    }
  },

  methods: {
    //add Coins
    add(prices) {
      this.total = this.total + prices
    },

    //reset Coins
    clear() {
      this.refune = this.total
      setTimeout(function () {
        location.reload()
      }, 3000)
      this.total = 0
      this.change = null
    },

    //select Products
    select(price1, price2, i) {
      var product = [
        'Pepsi Max',
        'Pepsi Diet',
        'Coke',
        'Coke Vanilla (S)',
        'Coke Vanilla (L)',
        'Mountain Dew',
      ]
      this.drink = product[i]
      if (price1 > price2) {
        this.lm = this.total
        setTimeout(function () {
          location.reload()
        }, 3000)
        this.total = 0
        this.change = null
      } else {
        console.log('price : ' + price1)
        console.log('total : ' + price2)
        console.log('chang : ' + (price2 - price1))
        console.log('drink : ' + this.drink)
        this.total = 0
        this.change = price2 - price1
        setTimeout(function () {
          location.reload()
        }, 3000)
      }
    },
  },
}
</script>

<style scoped>
.out {
  font-size: 25px;
  background-color: red;
  color: white;
}

.in {
  font-size: 25px;
  background-color: green;
  color: white;
}

.price {
  font-size: 20px;
}
</style>

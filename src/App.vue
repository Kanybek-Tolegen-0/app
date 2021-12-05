<template>
  <div>
    <div class="product-list">
      <Product v-for="product in products" :key="product.name" :product="product"/>
    </div>

    <div class="hotbuttons">
      <input ref="title" class="product-value" type="text"/>
      <input ref="price" class="product-value" type="text"/>
      <button class="HotButton add" v-on:click = "add">Создать</button>
      <button class="HotButton delete" v-on:click = "remove">Удалить</button>
    </div>
  </div>
</template>

<script>
import Product from '../src/components/Product.vue';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return (
      {
        products: {},
      }
    );
  },
  components: {
    Product,
  },
  beforeMount() {
    axios.get('https://project-b156e-default-rtdb.firebaseio.com/products.json')
    .then((response) => {
      this.products = response.data;
      // const keys = Object.keys((this.products));
      for(let k in this.products) {
        this.products[k].id = k;
      }
  });

  },
  methods: {
    async add() {
      await axios.post('https://project-b156e-default-rtdb.firebaseio.com/products.json', {
        title: this.$refs.title.value,
        price: +this.$refs.price.value,
      }).then((response) => console.log(response))
      .catch((err) => console.log(err));

      await axios.get('https://project-b156e-default-rtdb.firebaseio.com/products.json')
      .then((response) => {
        this.products = response.data;
        for(let k in this.products) {
        this.products[k].id = k;
        }
      });
    },

    async remove() {
      let target = null;
      for( let k in this.products) {
        if(this.products[k].title === this.$refs.title.value && this.products[k].price === +this.$refs.price.value) {
          target = this.products[k].id;
          await axios.delete(`https://project-b156e-default-rtdb.firebaseio.com/products/${target}.json`);
        }
      }

      await axios.get('https://project-b156e-default-rtdb.firebaseio.com/products.json')
          .then((response) => {
          this.products = response.data;
          for(let k in this.products) {
          this.products[k].id = k;
        }
      });
    },
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: sans-serif;
  }

  .product-list {
    background-color: gray;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
  }

  .HotButton {
    width: 200px;
    height: 60px;
    font-size: 24px;
    background: none;
    border-radius: 8px;
    cursor: pointer;
  }

  .HotButton:hover {
    border: none;
    font-size: 28px;
    color: white;
    transition: 0.2s all linear;
  }

  .add:hover {
    background: green;
  }

  .delete:hover {
    background: red;
  }

  .hotbuttons {
    margin: 24px;
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .product-value {
    width: 320px;
    font-size: 32px;
    height: 60px;
    border-radius: 16px;
    outline: none;
    text-indent: 16px;
  }
</style>

<template>
  <div class="product-list">
		<!-- Для v-for не был указан ключ  -->
    <li class="card" v-for="product in products" :style="{width: cardsWidth + '%'}" :key="product.index">
      <p class="card-title">{{ product.title }}</p>
      <img class="card-image" :src="product.image" alt="">
      <p class="card-price">Цена: {{ product.price }} {{ currency }}</p>

      <div>
        <input type="number" ref="amount" :id="product.id" >
        <span>кг</span>
      </div>

      <button @click="addToCart(product)"> В корзину </button>
    </li>
  </div>
</template>

<script>
export default {
  props: {
    currency: String,
  },
  data() {
    return {
      products: [],
    };
  },
  computed: {
    cardsWidth() {
      let width = window.innerWidth;
			console.log(width)
      let count = 1;
			// переменная width принимает тип Number. в данном случае if (width > '840px') условие не будет работать так как переданные данные не являються строкой
      if (width > 840) {
        count = 3;
      } else if ((width > 420 && width < 840)) {
        count = 2;
      }
      return 100 / count;
    },
  },
  methods: {
    startPricesMonitoring() {
      setInterval(this.getList, 1000);
    },
    async getList() {
      let data = await this.$store.dispatch('getProductsList');

      this.products = data;
    },
    addToCart(product) {
      let amount = this.$refs.amount.find((input) => input.id === product.id).value;
			console.log(amount)
			// В данном случае должна быть проверка на введённое число так как человек может случайно не внести число или внести его меньше нуля
			if(amount != 0 && amount > 0 && amount != ''){
      let data = {
				amount,
        price: product.price,
        title: product.title,
      };
      this.$parent.cart.push(data);
			}
			else{
				alert('Вы ввели не верный вес')
			}
    },
  },
  created() {
    this.startPricesMonitoring();
  },
};
</script>

<style>
  .product-list {
    padding: 10px;
  }

  .card {
    display: inline-block;
    width: 100%;
    border: 1px solid #908888;
    border-radius: 5px;
    text-align: center;
    padding: 10px;
  }
  .card-image {
    width: 100%;
  }
  button {
    padding: 5px;
    margin: 5px;
  }

</style>

<template>
  <div class="cart-list">
    <!-- <div v-for="item in spisok_dlya_rendera">
      <p>{{ item }}</p> Данные из массива не выводятся в виде массива это не безопасно. Так же для вывода v-for используеться ключь :key по которому будет выводиться список  
    </div> -->
		<div v-for="item in spisok_dlya_rendera" :key="item.index">
      <p> Товар:{{ item.title }} Количество:{{item.amount}} Стоимость за кг:{{item.price}} </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    cart: Array,
  },
  computed: {
      spisok_dlya_rendera() {
				return (this.cart.length > 1)? this.cart.reverse(): this.cart;
        
      },
			
  },
};
</script>

<style>

</style>

<template>
  <main id="eshop">
    <ProductsList :products="productsList" :cart="cart" @add-product="handleAddToCart" @decrement-product="handleDecrementProduct" />
    <Cart :cart="cart" @remove-product="handleRemoveProduct" />
  </main>
</template>

<script setup>
import ProductsList from './components/ProductsList.vue';
import Cart from './components/Cart.vue';
import { ref } from 'vue';

const productsList = ref([]);
const cart = ref([]);

function loadProducts() {
  fetch('./json/products.json')
    .then(response => response.json())
    .then(data => {
      productsList.value = data;
    })
}

loadProducts();



function handleAddToCart(productName) {
  if (!isProductInCart(productName)) {
    addProductToCart(productName);
  } else {
    changeAmountBy(productName, 1);
  }
}

function handleDecrementProduct(productName) {
  if (!isProductInCart(productName)) {
    console.error('Product is not in cart!')
  } else {
    changeAmountBy(productName, -1);
  }
}

function handleRemoveProduct(productName) {
  if (!isProductInCart(productName)) {
    console.error('Product is not in cart!')
  } else {
    changeAmountTo(productName, 0);
  }
}

function isProductInCart(productName) {
  if (cart.value.find(item => item.name === productName)) return true;
  return false;
}

function addProductToCart(productName) {
  cart.value.push({
    name: productName,
    quantity: 1,
    pricePerUnit: productsList.value.find(item => item.name === productName).price,
    priceTotal: productsList.value.find(item => item.name === productName).price,
  });
}

function changeAmountBy(productName, amount) {
  const productIndex = cart.value.findIndex(item => item.name === productName);
  if (productIndex === -1) return console.error('Product not found when adding to cart!');

  cart.value[productIndex].quantity += amount;
  cart.value[productIndex].priceTotal = cart.value[productIndex].pricePerUnit * cart.value[productIndex].quantity;
}

function changeAmountTo(productName, amount) {
  const productIndex = cart.value.findIndex(item => item.name === productName);
  if (productIndex === -1) return console.error('Product not found when adding to cart!');

  cart.value[productIndex].quantity = amount;
  cart.value[productIndex].priceTotal = cart.value[productIndex].pricePerUnit * cart.value[productIndex].quantity;
}

</script>

<style lang="scss" scoped>
#eshop {
  display: grid;
  align-items: start;
  grid-template-columns: 2fr 1fr;
  gap: 2rem;

  padding: 1vw 3vw;
}
</style>

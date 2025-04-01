<script setup>
import { ref, onMounted, provide } from 'vue';
import ProductList from './components/ProductList.vue';
import ShoppingCart from './components/ShoppingCart.vue';

const cartItems = ref([]);

onMounted(() => {
  const savedCart = localStorage.getItem('cart');
  if (savedCart) {
    cartItems.value = JSON.parse(savedCart);
  }
});

const saveCart = () => {
  localStorage.setItem('cart', JSON.stringify(cartItems.value));
};

const addToCart = (product) => {
  const existingItem = cartItems.value.find(item => item.id === product.id);
  
  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cartItems.value.push({
      ...product,
      quantity: 1
    });
  }
  
  saveCart();
};

const updateQuantity = (productId, quantity) => {
  const item = cartItems.value.find(item => item.id === productId);
  if (item) {
    item.quantity = quantity;
    saveCart();
  }
};

const removeItem = (productId) => {
  cartItems.value = cartItems.value.filter(item => item.id !== productId);
  saveCart();
};

const clearCart = () => {
  cartItems.value = [];
  saveCart();
};

provide('addToCart', addToCart);
</script>

<template>
  <div class="container">
    <h1>Shopping Cart Demo</h1>
    <div class="layout">
      <ProductList />
      <ShoppingCart 
        :cartItems="cartItems"
        @updateQuantity="updateQuantity"
        @removeItem="removeItem"
        @clearCart="clearCart"
      />
    </div>
  </div>
</template>

<style lang="scss">
:root {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  line-height: 1.5;
  font-weight: 400;
  color: #213547;
  background-color: #ffffff;
}

body {
  margin: 0;
  min-width: 320px;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  
  h1 {
    text-align: center;
    margin-bottom: 40px;
  }
}

.layout {
  display: flex;
  gap: 30px;
  
  @media (max-width: 768px) {
    flex-direction: column;
  }
}

button {
  font-family: inherit;
  cursor: pointer;
  transition: background-color 0.2s;
  
  &:focus {
    outline: none;
    box-shadow: 0 0 0 2px rgba(66, 153, 225, 0.5);
  }
}
</style>
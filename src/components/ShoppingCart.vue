<script setup>
import { computed } from 'vue';

const props = defineProps({
  cartItems: {
    type: Array,
    required: true
  }
});

const emit = defineEmits(['updateQuantity', 'removeItem', 'clearCart']);

const total = computed(() => {
  return props.cartItems.reduce((sum, item) => {
    const price = item.discountedPrice || item.price;
    return sum + price * item.quantity;
  }, 0);
});

const updateQuantity = (item, change) => {
  const newQuantity = item.quantity + change;
  if (newQuantity > 0) {
    emit('updateQuantity', item.id, newQuantity);
  } else {
    emit('removeItem', item.id);
  }
};
</script>

<template>
  <div class="cart">
    <div class="cart-header">
      <h2>Shopping Cart</h2>
      <button 
        v-if="cartItems.length" 
        @click="$emit('clearCart')" 
        class="clear-cart"
      >
        Clear Cart
      </button>
    </div>
    
    <div v-if="cartItems.length === 0" class="empty-cart">
      Your cart is empty
    </div>
    
    <div v-else class="cart-items">
      <div v-for="item in cartItems" :key="item.id" class="cart-item">
        <img :src="item.image" :alt="item.name">
        <div class="item-details">
          <h3>{{ item.name }}</h3>
          <div class="price">
            ${{ ((item.discountedPrice || item.price) * item.quantity).toFixed(2) }}
          </div>
          <div class="quantity-controls">
            <button @click="updateQuantity(item, -1)">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="updateQuantity(item, 1)">+</button>
          </div>
          <button 
            @click="$emit('removeItem', item.id)" 
            class="remove-item"
          >
            Remove
          </button>
        </div>
      </div>
      
      <div class="cart-total">
        <strong>Total:</strong> ${{ total.toFixed(2) }}
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.cart {
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  min-width: 250px;
  max-width: 400px;
  margin: 0 auto;
}

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  
  h2 {
    margin: 0;
  }
}

.clear-cart {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    background-color: #c82333;
  }
}

.empty-cart {
  text-align: center;
  padding: 20px;
  color: #666;
}

.cart-item {
  display: flex;
  gap: 15px;
  padding: 15px 0;
  border-bottom: 1px solid #ddd;
  
  img {
    width: 80px;
    height: 80px;
    object-fit: cover;
    border-radius: 4px;
  }
}

.item-details {
  flex: 1;
  
  h3 {
    margin: 0 0 5px 0;
    font-size: 1.1em;
  }
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 10px 0;
  
  button {
    background-color: #6c757d;
    color: white;
    border: none;
    width: 25px;
    height: 25px;
    border-radius: 4px;
    cursor: pointer;
    
    &:hover {
      background-color: #5a6268;
    }
  }
}

.remove-item {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.9em;
  
  &:hover {
    background-color: #c82333;
  }
}

.cart-total {
  margin-top: 20px;
  text-align: right;
  font-size: 1.2em;
}

@media (max-width: 768px) {
  .cart {
    max-width: 100%;
    margin: 20px 10px;
  }
}
</style>
<script setup>
import { inject } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
const addToCart = inject("addToCart");

const products = [
  {
    id: 1,
    name: "Wireless Headphones",
    price: 199.99,
    discountedPrice: 159.99,
    image: "https://picsum.photos/200/200?random=1",
  },
  {
    id: 2,
    name: "Smartwatch",
    price: 299.99,
    discountedPrice: 249.99,
    image: "https://picsum.photos/200/200?random=2",
  },
  {
    id: 3,
    name: "Laptop Backpack",
    price: 79.99,
    discountedPrice: 59.99,
    image: "https://picsum.photos/200/200?random=3",
  },
  {
    id: 4,
    name: "Bluetooth Speaker",
    price: 149.99,
    discountedPrice: 119.99,
    image: "https://picsum.photos/200/200?random=4",
  },
  {
    id: 5,
    name: "Mechanical Keyboard",
    price: 129.99,
    discountedPrice: 99.99,
    image: "https://picsum.photos/200/200?random=5",
  },
  {
    id: 6,
    name: "Gaming Mouse",
    price: 79.99,
    discountedPrice: 64.99,
    image: "https://picsum.photos/200/200?random=6",
  },
  {
    id: 7,
    name: "External Hard Drive",
    price: 109.99,
    discountedPrice: 89.99,
    image: "https://picsum.photos/200/200?random=7",
  },
  {
    id: 8,
    name: "USB-C Docking Station",
    price: 89.99,
    discountedPrice: 69.99,
    image: "https://picsum.photos/200/200?random=8",
  },
  {
    id: 9,
    name: "Webcam HD 1080p",
    price: 59.99,
    discountedPrice: 49.99,
    image: "https://picsum.photos/200/200?random=9",
  },
];

const handleAddToCart = (product) => {
  addToCart(product);
  toast.success(`${product.name} added to cart!`);
};
</script>

<template>
  <div class="products">
    <h2>Products</h2>
    <div class="products-grid">
      <div v-for="product in products" :key="product.id" class="product-card">
        <img :src="product.image" :alt="`Image of ${product.name}`" />
        <h3>{{ product.name }}</h3>
        <div class="price-container">
          <span v-if="product.discountedPrice" class="original-price">
            ${{ product.price.toFixed(2) }}
          </span>
          <span class="discounted-price">
            ${{ (product.discountedPrice || product.price).toFixed(2) }}
          </span>
        </div>
        <button @click="handleAddToCart(product)" class="add-to-cart">
          Add to Cart
        </button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.products {
  padding: 20px;
  flex: 2;

  h2 {
    margin-bottom: 20px;
    text-align: center;
  }
}

.products-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
}

.product-card {
  flex: 1;
  min-width: 250px;
  background: #fff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;

  @media (max-width: 1200px) {
    min-width: calc(50% - 30px);
  }

  @media (max-width: 768px) {
    min-width: calc(50% - 20px);
  }

  @media (max-width: 480px) {
    min-width: 100%;
  }

  &:hover {
    transform: translateY(-5px);
  }

  img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 15px;
  }

  h3 {
    margin: 10px 0;
    font-size: 1.2em;
  }
}

.price-container {
  margin: 10px 0;
  display: flex;
  gap: 10px;
  justify-content: center;
  align-items: center;
}

.original-price {
  text-decoration: line-through;
  color: #999;
}

.discounted-price {
  color: #e53e3e;
  font-weight: bold;
}

.add-to-cart {
  width: 100%;
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;

  &:hover {
    background-color: #45a049;
  }
}
</style>

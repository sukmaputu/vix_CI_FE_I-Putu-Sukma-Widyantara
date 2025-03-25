<template>
  <div class="container">
    <div v-if="product" :class="productClass">
      <h1 v-if="product.category !== 'unavailable'">{{ product.title }}</h1>
      <img v-if="product.category !== 'unavailable'" :src="product.image" alt="Product Image">
      <p v-if="product.category !== 'unavailable'">Price: ${{ product.price }}</p>
      <p v-if="product.category !== 'unavailable'">Category: {{ product.category }}</p>
      <p v-if="product.category === 'unavailable'">Product not available.</p>
    </div>
    <button @click="nextProduct">Next Product</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      index: 1,
      product: null,
    };
  },
  methods: {
  async fetchProduct() {
    try {
      const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
      const data = await response.json();

      console.log("API Response:", data); 

      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = data;
      } else {
        this.product = { category: "unavailable" };
      }
    } catch (error) {
      console.error("Error fetching product:", error);
    }
  },
  nextProduct() {
    this.index = this.index >= 20 ? 1 : this.index + 1;
    this.fetchProduct();
  },
},

  mounted() {
    this.fetchProduct();
  },
  computed: {
    productClass() {
      if (this.product?.category === "men's clothing") return "page-men";
      if (this.product?.category === "women's clothing") return "page-women";
      return "page-unavailable";
    },
  },
};
</script>

<style>
:root {
  --men-bg: #87ceeb;
  --women-bg: #ffb6c1;
  --unavailable-bg: #d3d3d3;
}

.page-men {
  background-color: var(--men-bg);
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.page-women {
  background-color: var(--women-bg);
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.page-unavailable {
  background-color: var(--unavailable-bg);
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  font-size: 1.3rem;
  border: 1px solid blue;
}

button:hover {
  background-color: #0056b3;
}

img {
  max-width: 200px;
  height: auto;
  margin-top: 10px;
  border-radius: 10px;
}
</style>

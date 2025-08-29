<template>
  <div class="app">
    <h1>🛒 Vue Product Search</h1>

   
    <div class="search-box">
      <inputv-model="query" @input="searchProducts"placeholder="Search for a product..."/>
    </div>

    <div class="products-grid">
      <div class="product-card" v-for="p in products" :key="p.id">
        <img :src="p.images[0]" alt="Product Image" />
        <h3>{{ p.title }}</h3>
        <p>💰 ${{ p.price }}</p>
        <p>{{ p.category.name }}</p>
        <button class="buy-btn" @click="buyNow(p.title)">Buy Now</button>
      </div>
    </div>

    
    <p v-if="products.length === 0" class="no-results">
       No products found for "{{ query }}"
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: "",
      allProducts: [],  
      products: [],
    };
  },
  async mounted() {
    try {
      const res = await fetch("https://api.escuelajs.co/api/v1/products");
      const data = await res.json();
      this.allProducts = data;
      this.products = data;
    } catch (err) {
      console.error("Error fetching products:", err);
    }
  },
  methods: {
    searchProducts() {
      if (!this.query) {
        this.products = this.allProducts;
      } else {
        this.products = this.allProducts.filter((p) =>
          p.title.toLowerCase().includes(this.query.toLowerCase())
        );
      }
    },
    buyNow(productName) {
      alert(`🛒 You click Buy Now for "${productName}"`)
    },
  },
};

</script>

<style>

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 30px;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #f0f4f8, #e2ebf0);
  min-height: 100vh;
  width: 100%;
  box-sizing: border-box;
}

h1 {
  margin-bottom: 25px;
  color: #2c3e50;
  font-size: 2.3rem;
  text-align: center;
}


.search-box {
  margin-bottom: 30px;
  width: 100%;
  display: flex;
  justify-content: center;
}

input {
  padding: 12px 15px;
  width: 4000px;
  border: 2px solid #3498db;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
  transition: all 0.3s ease;
}

input:focus {
  border-color: #2980b9;
  box-shadow: 0 0 8px rgba(41, 128, 185, 0.5);
}


.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 25px;
  width: 100%;
  max-width: 1400px;
  padding: 10px;
  box-sizing: border-box;
}


.product-card {
  border-radius: 12px;
  padding: 15px;
  background: #fff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.product-card:hover {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
}


.product-card img {
  width: 100%;
  height: 260px;
  object-fit: cover;
  border-radius: 10px;
  margin-bottom: 15px;
}


.product-card h3 {
  font-size: 1.2rem;
  margin: 10px 0 5px;
  color: #2c3e50;
  min-height: 40px;
}

.product-card p {
  margin: 5px 0;
  font-size: 1rem;
  color: #555;
}

.product-card p:first-of-type {
  font-weight: bold;
  color: #27ae60;
}


.buy-btn {
  margin-top: 12px;
  padding: 10px;
  background: linear-gradient(135deg, #3498db, #2ecc71);
  color: #fff;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.buy-btn:hover {
  background: linear-gradient(135deg, #2ecc71, #3498db);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
  transform: scale(1.05);
}


.no-results {
  margin-top: 20px;
  font-size: 1.2rem;
  color: #e74c3c;
}
</style>

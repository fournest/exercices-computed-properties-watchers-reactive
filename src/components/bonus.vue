<script setup>
import { reactive, ref, computed, watch } from 'vue'

const productsData = reactive([
    {
        "name": "Ordinateur portable",
        "category": "Électronique",
        "price": 1200,
        "stock": 50
    },
    {
        "name": "Smartphone",
        "category": "Électronique",
        "price": 800,
        "stock": 150
    },
    {
        "name": "Livre de cuisine",
        "category": "Livres",
        "price": 25,
        "stock": 200
    },
    {
        "name": "Casque audio",
        "category": "Électronique",
        "price": 150,
        "stock": 75
    },
    {
        "name": "T-shirt en coton",
        "category": "Vêtements",
        "price": 30,
        "stock": 300
    }
]);


const searchQuery = ref('');
const selectedCategory = ref('Toutes');
const maxPrice = ref(Infinity);
const showOnlyInStock = ref(false);

const categories = computed(() => {
    const uniqueCategories = new Set();
    for (const product of productsData) {
        uniqueCategories.add(product.category);
    }
    return ['Toutes', ...Array.from(uniqueCategories)];
});

const filteredProducts = computed(() => {
    return productsData.filter(product => {
        const matchesSearchQuery = product.name.toLowerCase().includes(searchQuery.value.toLowerCase());
        const matchesCategory = selectedCategory.value === 'Toutes' || product.category === selectedCategory.value;
        const matchesMaxprice = product.price <= maxPrice.value;
        const matchesInStock = !showOnlyInStock.value || product.stock > 0;
        return matchesSearchQuery && matchesCategory && matchesMaxprice && matchesInStock;
    });
});


</script>

<template>
    <div>
        <h2>Catalogue de produits</h2>
        <div class="filters">
            <input v-model="searchQuery" type="text" placeholder="Rechercher un produit...">
            <select v-model="selectedCategory">
                <option value="">Toutes les catégories</option>
                <option v-for="category in categories" :key="category" :value="category">
                    {{ category }}
                </option>
            </select>
            <div>
                <label>Prix max : {{ maxPrice }}€</label>
                <input v-model.number="maxPrice" type="range" min="0" max="1000" step="10">
            </div>

            <label>
                <input v-model="showOnlyInStock" type="checkbox">
                Seulement les produits en stock
            </label>
        </div>

        <div class="results">
            <p>{{ filteredProducts.length }} produit(s) trouvé(s)</p>
            <div v-for="product in filteredProducts" :key="product.id" class="product-card">
                <h3>{{ product.name }}</h3>
                <p>Catégorie : {{ product.category }}</p>
                <p>Prix : {{ product.price }}€</p>
                <p>Stock : {{ product.stock }} {{ product.stock > 0 ? '✅' : '❌' }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
.filters {
    background: #f0f0f0;
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 5px;
}

.filters>* {
    margin: 10px 0;
    display: block;
}

.filters input,
.filters select {
    padding: 8px;
    margin: 5px 0;
    width: 200px;
}

.product-card {
    border: 1px solid #ddd;
    padding: 15px;
    margin: 10px 0;
    border-radius: 5px;
    background: white;
}

.results {
    margin-top: 20px;
}
</style>

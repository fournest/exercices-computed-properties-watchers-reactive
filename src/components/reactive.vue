<script setup>

import { reactive, computed } from 'vue'

const availableProducts = reactive(
    [
        { id: 1, name: "Yamaha YZF-R1", price: 18000 },
        { id: 2, name: "Harley-Davidson Street Glide Special", price: 28000 },
        { id: 3, name: "KTM 1290 Super Duke R", price: 20500 },
        { id: 4, name: "BMW R 1250 GS Adventure", price: 23000 }
    ]);

const cartItems = reactive([]);

const addToCart = (productToAdd) => {
    const existingItem = cartItems.find(item => item.id === productToAdd.id);
    if (existingItem) {
        existingItem.quantity++;
    } else {
        cartItems.push({ ...productToAdd, quantity: 1 });
    }
}

const removeFromCart = (productId) => {
    const existingItem = cartItems.find(item => item.id === productId);
    if (existingItem) {
        if (existingItem.quantity > 1) {
            existingItem.quantity--;
        } else {
            const index = cartItems.findIndex(item => item.id === productId);
            if (index !== -1) {
                cartItems.splice( index, 1 )
            }
        }
    }
};

const clearCart = () => {
    cartItems.splice( 0, cartItems.length );
}

const totalItems = computed(() => {
    return cartItems.reduce((sum, item) => sum + item.quantity,0);
});

const totalPrice = computed(() => {
return cartItems.reduce((sum, item) => sum + (item.price*item.quantity),0);
});


</script>

<template>
    <div>
        <h2>Panier d'achat</h2>
        <div class="products">
            <h3>Produits disponibles</h3>
            <div v-for="product in availableProducts" :key="product.id" class="product">
                <span>{{ product.name }} - {{ product.price }}€</span>
                <button @click="addToCart(product)">Ajouter au panier</button>
            </div>
        </div>
        <div class="cart">
            <h3>Votre panier</h3>
            <div v-if="cartItems.length === 0">Panier vide</div>
            <div v-else>
                <div v-for="item in cartItems" :key="item.id" class="cart-item">
                    <span>{{ item.name }}</span>
                    <span>{{ item.price }}€ x {{ item.quantity }}</span>
                    <button @click="removeFromCart(item.id)">-</button>
                    <button @click="addToCart(item)">+</button>
                </div>
                <div class="cart-summary">
                    <p><strong>Total articles : {{ totalItems }}</strong></p>
                    <p><strong>Total : {{ totalPrice }}€</strong></p>
                    <button @click="clearCart">Vider le panier</button>
                </div>
            </div>
        </div>
    </div>



</template>

<style scoped>
div {
  background-color: #ffffff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-width: 450px;
  box-sizing: border-box;
}
.products,
.cart {
    margin: 20px 0;
    padding: 15px;
    border: 1px solid #ccc;
}

.product,
.cart-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 10px 0;
    padding: 10px;
    background: #f5f5f5;
}

.cart-summary {
    margin-top: 15px;
    padding-top: 15px;
    border-top: 2px solid #333;
}

button {
    margin: 0 5px;
    padding: 5px 10px;
    cursor: pointer;
}
</style>
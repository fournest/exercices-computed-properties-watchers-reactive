<script setup>
import { ref, computed } from 'vue'

const prixHT = ref();
const tauxTVA = ref();

const prixTTC = computed(() => prixHT.value * (1 + tauxTVA.value));
const montantTVA = computed(() => prixTTC.value - prixHT.value);

</script>

<template>
    <div>
        <h2>Calculateur de prix TTC</h2>
        <label>
            Prix HT :
            <input v-model.number="prixHT" type="number" step="0.01">
        </label>
        <br>
        <label>
            TVA (%) :
            <input v-model.number="tauxTVA" type="number" step="0.1">
        </label>
        <br>
        <p><strong>Prix TTC : {{ prixTTC }}€</strong></p>
        <p>Montant TVA : {{ montantTVA }}€</p>
    </div>

</template>

<style scoped>
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient to right, #6a11cb, #2575fc;
  margin: 0;
}

div {
  background-color: #ffffff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-width: 450px;
  box-sizing: border-box;
}

h2 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 25px;
  font-size: 1.8em;
  font-weight: 600;
}

label {
  display: block;
  margin-bottom: 12px;
  color: #34495e;
  font-weight: 500;
  font-size: 1.05em;
}

input[type="number"] {
  width: calc(100% - 24px);
  padding: 12px;
  margin-top: 6px;
  border: 1px solid #c0c0c0;
  border-radius: 6px;
  box-sizing: border-box;
  font-size: 17px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input[type="number"]:focus {
  border-color: #2575fc;
  outline: none;
  box-shadow: 0 0 0 3px rgba(37, 117, 252, 0.2);
}

p {
  margin-top: 20px;
  padding: 10px 0;
  border-top: 1px solid #ebf0f5;
  color: #2c3e50;
  font-size: 1.2em;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

p:first-of-type {
  margin-top: 25px; /* Adjust margin for the first result paragraph */
}

p strong {
  color: #6a11cb;
  font-size: 1.3em;
}

</style>
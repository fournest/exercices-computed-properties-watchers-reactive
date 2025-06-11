<script setup>
import { ref, watch, computed } from 'vue'

const email = ref('');
const password = ref('');
const confirmPassword = ref ('');
const emailRegex = /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/;

const isEmailValid = computed(() => {
    return emailRegex.test(email.value);
});

const isPasswordValid = computed(() => {
return password.value.length >= 8;
});

const isPasswordMatch = computed(() => {
return password.value === confirmPassword.value && confirmPassword.value !== '';
});

const isFormValid = computed(() => {
return isEmailValid.value && isPasswordValid.value && isPasswordMatch.value;
});

</script>

<template>
    <div>
        <h2>Formulaire d'inscription</h2>
        <form>
            <div>
                <label>Email :</label>
                <input v-model="email" type="email" :class="{
                    error: !isEmailValid &&
                        email
                }">
                <span v-if="!isEmailValid && email" class="error-msg">Email invalide
                </span>
            </div>
            <div>
                <label>Mot de passe :</label>
                <input v-model="password" type="password" :class="{
                    error: !isPasswordValid && password
                }">
                <span v-if="!isPasswordValid && password" class="error-msg">
                    Le mot de passe doit contenir au moins 8 caractères
                </span>
            </div>
            <div>
                <label>Confirmer le mot de passe :</label>
                <input v-model="confirmPassword" type="password" :class="{
                    error:
                        !isPasswordMatch && confirmPassword
                }">
                <span v-if="!isPasswordMatch && confirmPassword" class="error-msg">
                    Les mots de passe ne correspondent pas
                </span>
            </div>
            <button type="submit" :disabled="!isFormValid">S'inscrire</button>
        </form>
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

h2 {
    font-size: 30px;
    font-weight: bolder;
    padding: 15px;

}
.error {
    border: 2px solid red;
}

.error-msg {
    color: red;
    font-size: 0.8em;
}

label {
    display: block;
    margin-top: 10px;
}

input {
    width: 100%;
    padding: 5px;
    margin: 5px 0;
}

button {
    margin-top: 15px;
    padding: 10px 20px;
}

button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
</style>
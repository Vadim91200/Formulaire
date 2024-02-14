<template>
  <form @submit.prevent="handleSubmit">
    <div>
      <label for="login">Login:</label>
      <input id="login" v-model="form.login" @blur="v$.login.$touch()" />
      <span v-if="v$.login.$error">Le login ne doit pas contenir d'espaces</span>
    </div>

    <div>
      <label for="email">Email:</label>
      <input id="email" type="email" v-model="form.email" @blur="v$.email.$touch()" />
      <span v-if="v$.email.$error">Email invalide</span>
    </div>

    <div>
      <label for="password">Mot de passe:</label>
      <input id="password" type="password" v-model="form.password" @blur="v$.password.$touch()" />
      <span v-if="v$.password.$error">Le mot de passe doit contenir au moins 8 caractères</span>
    </div>

    <div>
      <label for="confirmPassword">Confirmation du mot de passe:</label>
      <input id="confirmPassword" type="password" v-model="form.confirmPassword" @blur="v$.confirmPassword.$touch()" />
      <span v-if="v$.confirmPassword.$error || form.password !== form.confirmPassword">Les mots de passe ne correspondent pas</span>
    </div>

    <button type="submit">Inscription</button>
  </form>
</template>

<script setup>
import { reactive, ref } from 'vue';
import { useVuelidate } from '@vuelidate/core';
import { required, email, minLength } from '@vuelidate/validators';

const form = reactive({
  login: '',
  email: '',
  password: '',
  confirmPassword: ''
});

const rules = {
  login: { required, noSpaces: value => !value.includes(' ') },
  email: { required, email },
  password: { required, minLength: minLength(8) },
  confirmPassword: { required }
};

const v$ = useVuelidate(rules, form);

const handleSubmit = () => {
  v$.$validate();
  if (!v$.$error) {
    // Ici, faites l'appel API pour envoyer les données du formulaire
    console.log('Formulaire validé', form);
  } else {
    console.log('Erreurs dans le formulaire', v$);
  }
};
</script>

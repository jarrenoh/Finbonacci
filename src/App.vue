<template>
  <nav>
    <router-link to="/"> Login </router-link>
    <router-link to="/register"> Register </router-link>
    <router-link to="/home"> Home </router-link>
    <button @click="handleSignOut" v-if="isLoggedIn"> Sign Out </button>
  </nav>
  <router-view />
</template>

<script setup>
import {onMounted, ref} from "vue";
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth';
import { useRouter } from 'vue-router';
const isLoggedIn = ref(false);
const router = useRouter();

let auth;
onMounted(() => {
  auth = getAuth();
  onAuthStateChanged(auth, (user) => {
    if (user) {
      isLoggedIn.value = true;
    } else {
      isLoggedIn.value = false;
    }
  });
});


const handleSignOut = () => {
  signOut(auth).then(() => {
    router.push('/');
  });
}
</script>
<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: bisque;
}
</style>

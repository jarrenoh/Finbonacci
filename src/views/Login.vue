<template>
    <div class="container">
        <h1>Finbonacci</h1>
        <p>Take control of your Financial future with us - smart tracking, powerful insights and expert advice, all in one place!</p>
        <h2>Have An Account?</h2>
        <p><input type="text" placeholder="Email" v-model="email"></p>
        <p><input type="password" placeholder="Password" v-model="password"></p>
        <p v-if="errMsg" class="error">{{ errMsg }}</p>
        <p><button @click="register">Sign In</button></p>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { getAuth, signInWithEmailAndPassword } from 'firebase/auth';
import { useRouter } from 'vue-router';

const email = ref('');
const password = ref('');
const errMsg = ref('');
const router = useRouter();

const register = () => {
    signInWithEmailAndPassword(getAuth(), email.value, password.value)
        .then(() => {
            console.log("Logged In");
            router.push('/home');
        })
        .catch((error) => {
            console.log(error.code);
            switch (error.code) {
                case "auth/invalid-email":
                    errMsg.value = "Invalid Email";
                    break;
                case "auth/user-not-found":
                    errMsg.value = "No Account With That Email Found";
                    break;
                case "auth/wrong-password":
                    errMsg.value = "Wrong Password";
                    break;
                default:
                    errMsg.value = "Something Went Wrong";
                    break;
            }
        });
}
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: bisque;
}

h2, h1, p {
    text-align: center;
}

input {
    padding: 10px;
    width: 250px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin: 5px 0;
}

button {
    padding: 10px 20px;
    background-color: grey;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

.error {
    color: red;
}
</style>

<template>
    <div>
        <Navbar />
        <div class="container">
            <h1>Settings</h1>

            <!-- Change Username -->
            <div class="settings-option">
                <h3>Change Username</h3>
                <input v-model="newUsername" placeholder="New Username" />
                <button @click="changeUsername">Save</button>
                <p v-if="usernameError" class="error">{{ usernameError }}</p>
            </div>

            <!-- Change Email -->
            <div class="settings-option">
                <h3>Change Email</h3>
                <input v-model="newEmail" placeholder="New Email" />
                <button @click="changeEmail">Save</button>
                <p v-if="emailError" class="error">{{ emailError }}</p>
            </div>

            <!-- Change Password -->
            <div class="settings-option">
                <h3>Change Password</h3>
                <input v-model="newPassword" placeholder="New Password" type="password" />
                <button @click="changePassword">Save</button>
                <p v-if="passwordError" class="error">{{ passwordError }}</p>
            </div>

            <!-- Change Spending Target -->
            <div class="settings-option">
                <h3>Change Spending Target</h3>
                <input v-model="newSpendingTarget" placeholder="New Spending Target" type="number" />
                <button @click="changeSpendingTarget">Save</button>
                <p v-if="spendingTargetError" class="error">{{ spendingTargetError }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { getAuth, updateEmail, updatePassword } from 'firebase/auth';
import { doc, updateDoc, getDoc } from 'firebase/firestore';
import { db } from '../main.js'; // Ensure this import is correct
import Navbar from '../components/Navbar.vue';

const auth = getAuth();

// State for inputs
const newUsername = ref('');
const newEmail = ref('');
const newPassword = ref('');
const newSpendingTarget = ref('');

// State for error messages
const usernameError = ref('');
const emailError = ref('');
const passwordError = ref('');
const spendingTargetError = ref('');

// Change Username
const changeUsername = async () => {
    if (!newUsername.value) {
        usernameError.value = "Username cannot be empty.";
        return;
    }

    try {
        const user = auth.currentUser;
        if (!user) {
            usernameError.value = "User not authenticated.";
            return;
        }

        const userDocRef = doc(db, "users", user.uid);

        // Update the username in Firestore
        await updateDoc(userDocRef, {
            username: newUsername.value,
        });

        usernameError.value = ''; // Clear error
        alert("Username updated successfully!");
        console.log("Username updated:", newUsername.value);
    } catch (error) {
        usernameError.value = "Failed to update username.";
        console.error("Error updating username:", error);
    }
};

// Change Email
const changeEmail = async () => {
    if (!newEmail.value) {
        emailError.value = "Email cannot be empty.";
        return;
    }

    try {
        const user = auth.currentUser;
        if (!user) {
            emailError.value = "User not authenticated.";
            return;
        }

        // Update email in Firebase Authentication
        await updateEmail(user, newEmail.value);

        // Update email in Firestore
        const userDocRef = doc(db, "users", user.uid);
        await updateDoc(userDocRef, {
            email: newEmail.value,
        });

        emailError.value = ''; // Clear error
        alert("Email updated successfully!");
        console.log("Email updated:", newEmail.value);
    } catch (error) {
        emailError.value = "Failed to update email.";
        console.error("Error updating email:", error);
    }
};

// Change Password
const changePassword = async () => {
    if (!newPassword.value) {
        passwordError.value = "Password cannot be empty.";
        return;
    }

    try {
        const user = auth.currentUser;
        if (!user) {
            passwordError.value = "User not authenticated.";
            return;
        }

        // Update password in Firebase Authentication
        await updatePassword(user, newPassword.value);

        passwordError.value = ''; // Clear error
        alert("Password updated successfully!");
        console.log("Password updated.");
    } catch (error) {
        passwordError.value = "Failed to update password.";
        console.error("Error updating password:", error);
    }
};

// Change Spending Target
const changeSpendingTarget = async () => {
    if (!newSpendingTarget.value) {
        spendingTargetError.value = "Spending target cannot be empty.";
        return;
    }

    try {
        const user = auth.currentUser;
        if (!user) {
            spendingTargetError.value = "User not authenticated.";
            return;
        }

        const userDocRef = doc(db, "users", user.uid);

        // Update spending target in Firestore
        await updateDoc(userDocRef, {
            spendingTarget: parseFloat(newSpendingTarget.value),
        });

        spendingTargetError.value = ''; // Clear error
        alert("Spending target updated successfully!");
        console.log("Spending target updated:", newSpendingTarget.value);
    } catch (error) {
        spendingTargetError.value = "Failed to update spending target.";
        console.error("Error updating spending target:", error);
    }
};
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: bisque;
    text-align: center;
}

.settings-option {
    margin: 20px 0;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    width: 300px;
    background-color: white;
}

.settings-option h3 {
    margin-bottom: 10px;
}

.settings-option input {
    padding: 8px;
    width: 100%;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.settings-option button {
    padding: 8px 15px;
    background-color: grey;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.settings-option button:hover {
    background-color: #0056b3;
}

.error {
    color: red;
    font-size: 14px;
    margin-top: 5px;
}
</style>
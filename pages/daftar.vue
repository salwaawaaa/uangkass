<template>
    <div>
        <form @submit.prevent="masuk()">
            <input v-model="email" type="email"> <br>
            <input v-model="password" type="password"> <br>
            <button>kirim</button>
        </form>
    </div>
</template>

<script setup>
const client = useSupabaseAuthClient()

const email = ref()
const password = ref()

async function masuk() {
    await client.auth.signUp({
        email: email.value,
        password: password.value,
    })

    console.log("berhasil. cek email")
}
</script>

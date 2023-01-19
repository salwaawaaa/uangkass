<template>
    <div>
        <form @submit.prevent="masuk()">
            <br><input v-model="email" type="email"> <br/>
            <br><input v-model="password" type="password"> <br/>
            <br><button>masuk</button>
        </form>
    </div>
</template>

<script setup>
const client = useSupabaseAuthClient()

const email = ref()
const password = ref()

async function masuk() {
    await client.auth.signInWithPassword({
        email: email.value,
        password: password.value,
    })

    navigateTo("/admin")
}
</script>

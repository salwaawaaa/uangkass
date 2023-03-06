<template>
    <div class="col-md-4 m-auto p-3 mt-5">
      <div class="card">
        <div class="card-header bg-dark"><h3 class="text-center fw-bolder text-white">DAFTAR</h3></div>
        <div class="card-body">
        <form @submit.prevent="masuk()">
            <label for="email" class="text-dark fw-bold upper">Email</label>
             <input v-model="email" id="email" type="email" class="form-control"/> <br>
             <label for="password" class="text-dark fw-bold upper">Password</label>
            <input v-model="password" type="password" class="form-control"> <br>
            <button class="btn btn-primary me-3">masuk</button>
        </form>
        </div>
        </div>
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

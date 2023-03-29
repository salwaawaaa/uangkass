<template>
  <div>
    <div class="col-md-4 m-auto p-3 mt-5">
      <div class="card">
        <div class="card-header bg-dark"><h3 class="text-center fw-bolder text-white">LOGIN</h3></div>
        <div class="card-body">
          <div v-if="err" class="alert alert-danger">Email/Password Salah</div>
          <form @submit.prevent="masuk()">
            <label for="email" class="text-dark fw-bold upper mt-3">Email</label>
            <input v-model="email" id="email" type="email" class="form-control" />

            <label for="pass" class="text-dark fw-bold mt-3">Password</label><input v-model="password" id="pass" type="password" class="form-control" />

            <button class="btn btn-primary me-3 fw-semibold mt-3">masuk</button>
            <NuxtLink to="/daftar" class="btn btn-info  me-3 text-white mt-3 fw-semibold text-right">Sigin</NuxtLink>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const client = useSupabaseAuthClient();
const email = ref();
const password = ref();

async function masuk() {
  await client.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  navigateTo("/admin");
}
</script>

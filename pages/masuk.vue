<template>
  <div>
    <div class="col-md-4 m-auto p-3 mt-5">
      <div class="card">
        <div class="card-header bg-secondary"><h3 class="text-center fw-bolder text-white">LOGIN</h3></div>
        <div class="card-body">
          <div v-if="err" class="alert alert-danger">Email/Password Salah</div>
          <form @submit.prevent="masuk()">
        <br />
        <label for="email" class="text-dark fw-bold upper">Email</label>
        <input v-model="email" id="email" type="email" class="form-control" />
        <br />
        <br /><label for="pass" class="text-dark fw-bold">Password</label><input v-model="password" id="pass" type="password" class="form-control" />
        <br />
        <br /><button class="btn btn-primary me-3 fw-semibold">masuk</button>
        
          <NuxtLink to="/daftar" class="btn btn-info text-dark fw-semibold">SignUp</NuxtLink>
      </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const client = useSupabaseAuthClient();
const email  = ref();
const password = ref();

async function masuk() {
  await client.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });

  navigateTo("/admin");
}
</script>

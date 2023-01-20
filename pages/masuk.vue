<template>
  <div>
    <div class="col-md-4 m-auto card p-3 mt-5">
      <h3 class="text-center fw-bolder text-dark">LOGIN</h3>
      <form @submit.prevent="masuk()">
        <br />
        <label for="email" class="text-dark fw-bold upper">Email</label
        ><input v-model="email" id="email" type="email" class="form-control" />
        <br />
        <br /><label for="pass" class="text-dark fw-bold">Password</label
        ><input
          v-model="password"
          id="pass"
          type="password"
          class="form-control"
        />
        <br />
        <br /><button class="btn btn-primary mb-3">masuk</button>
      </form>
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

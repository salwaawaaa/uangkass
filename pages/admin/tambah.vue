<template>
  <div>
    <div class="col-md-4 m-auto card p-3 mt-5">
      <h1 class="text-center text-dark">Bayar Kas</h1>
      <form @submit.prevent="tambah()">
        <select v-model="nama" class="form-control">
          <option v-for="s in siswa" :key="s.id" :value="s.id">
            {{ s.nama }}
          </option></select
        ><br />
        <select v-model="minggu" class="form-control">
          <option v-for="s in weeks" :key="s.id" :value="s.id">
            {{ s.minggu }}
          </option></select
        ><br />
        <input v-model="nominal" placeholder="nominal" class="form-control" />
        <br />

        <button type="submit" class="btn btn-primary my-3">Tambah</button><br />
        <NuxtLink to="/" class="btn btn-danger">Kembali</NuxtLink>
      </form>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const nama = ref();
const weeks = ref();
const minggu = ref();
const nominal = ref();
const siswa = ref([]);

async function tambah() {
  await supabase.from("kas").insert({
    id_siswa: nama.value,
    id_week: minggu.value,
    nominal: nominal.value,
  });
  navigateTo("/admin");
}

async function dataSiswa() {
  const { data, error } = await supabase.from("siswa").select();
  siswa.value = data;
}
async function getWeek() {
  const { data, error } = await supabase.from("week").select("id,minggu");
  weeks.value = data;
  console.log(data);
}
onMounted(() => {
  dataSiswa();
  getWeek();
});
</script>

<template>
  <div>
    <div class="col-md-4 m-auto card p-3 mt-5">
      <h1 class="text-center text-dark">Bayar Kas</h1>
      <form @submit.prevent="tambah()">

        <label for="name" class="text-dark fw-bold upper">Nama</label>
        <select v-model="nama" class="form-control">
          <option v-for="s in siswa" :key="s.id" :value="s.id">
            {{ s.nama }}
          </option></select><br />
          
         <label for="week" class="text-dark fw-bold upper">Minggu</label>
        <select v-model="minggu" class="form-control">
          <option v-for="s in weeks" :key="s.id" :value="s.id">
            {{ s.minggu }}
          </option></select><br />

          <label for="nominal" class="text-dark fw-bold upper">Nominal Rp</label>
        <input v-model="nominal" id="nominal" type="nominal" class="form-control" />
          
        <button type="submit" class="btn btn-primary my-3">Tambah</button>
        <NuxtLink to="/admin" class="btn btn-danger m-3">Kembali</NuxtLink>
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

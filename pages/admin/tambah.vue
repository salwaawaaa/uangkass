<template>
  <navbar />
  <div>
    <h1>Bayar Kas</h1>
    <form @submit.prevent="tambah()">
      <select v-model="nama">
        <option v-for="s in siswa" :key="s.id" :value="s.id">{{ s.nama }}</option>
      </select>
      <input v-model="nominal" placeholder="nominal" /> <br />
      <button type="submit">Tambah</button>
      <NuxtLink to="/">Kembali</NuxtLink>
    </form>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const nama = ref();
const nominal = ref();
const siswa = ref([]);

async function tambah() {
  await supabase.from("kas").insert({
    id_siswa: nama.value,
    nominal: nominal.value,
  });
  navigateTo("/");
}

async function dataSiswa() {
  const { data, error } = await supabase.from("siswa").select();
  siswa.value = data;
}

onMounted(() => {
  dataSiswa();
});
</script>

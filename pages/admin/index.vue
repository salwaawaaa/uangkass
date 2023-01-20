<template>
  <div>
    <h1 class="text-dark fw-bolder">Selamat Datang Bendahara</h1>
    <NuxtLink to="/admin/tambah" class="btn btn-info text-white m-3"
      >Bayar</NuxtLink
    >

    <select>
      <option value="" disabled>Pilih Minggu</option>
      <option v-for="week in weeks" :key="week.id">{{ week.minggu }}</option>
    </select>
    <button class="btn btn-primary">Cari</button>

    <button @click="logout()" class="text-white btn btn-dark">Logout</button>
    <table class="table">
      <thead>
        <tr>
          <th>#</th>
          <th>nama</th>
          <th>minggu</th>
          <th>nominal</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(visitor, i) in datas" :key="visitor.id">
          <td>{{ i }}</td>
          <td>{{ visitor.id_siswa.nama }}</td>
          <td>{{ visitor.id_week.minggu }}</td>
          <td>{{ visitor.nominal }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
definePageMeta({
  middleware: "auth",
});
const supabase = useSupabaseClient();
const supa = useSupabaseAuthClient();
const datas = ref([]);
const weeks = ref();

async function getData() {
  const { data, error } = await supabase
    .from("kas")
    .select(`id, tanggal, nominal, id_siswa(nama) , id_week(minggu)`);
  datas.value = data;
  console.log(data);
}
async function getWeek() {
  const { data, error } = await supabase.from("week").select("id,minggu");
  weeks.value = data;
  console.log(data);
}
function logout() {
  const { error } = supa.auth.signOut();
  navigateTo("/");
}
onMounted(() => {
  getData();
  getWeek();
});
</script>

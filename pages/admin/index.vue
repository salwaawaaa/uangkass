<template>
  <div>
    <NuxtLink to="/admin/tambah" class="btn btn-info text-white m-3">Bayar</NuxtLink>

    <select v-model="minggu">
      <option v-for="week in weeks" :key="week.id" :value="week.minggu">{{ week.minggu }}</option>
    </select>

    <button @click="logout()" class="text-white btn btn-dark m-3">Logout</button>

    <div class="text-muted">ada {{ dataFilter.length }} data di {{ minggu }}</div>
    <table class="table">
      <thead>
        <tr class="text-white bg-dark">
          <th>#</th>
          <th>nama</th>
          <th>minggu</th>
          <th>nominal</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="dataFilter < 1">
          <td colspan="4">tidak ada data kas</td>
        </tr>

        <tr v-for="(visitor, i) in dataFilter" :key="visitor.id">
          <td>{{ i+1 }}.</td>
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
const weeks = ref([]);
const minggu = ref("minggu 1");
const router = useRouter()

console.log(router.currentRoute.value)

async function getData() {
  const { data, error } = await supabase.from("kas").select(`id, tanggal, nominal, id_siswa(nama) , id_week(id, minggu)`);
  datas.value = data;
}
async function getWeek() {
  const { data, error } = await supabase.from("week").select("id,minggu");
  weeks.value = data
}
function logout() {
  const { error } = supa.auth.signOut();
  navigateTo("/masuk");
}

const dataFilter = computed(() => {
  return datas.value.filter((i) => {
    return i.id_week.minggu.includes(minggu.value);
  });
});

onMounted(() => {
  getData();
  getWeek();
   //dataFilter()
});
</script>

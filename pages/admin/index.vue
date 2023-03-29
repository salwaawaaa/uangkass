<template>
  <div class="">
    <div class="container">
      <div class="card mt-5">
        <div class="card-header bg-dark">
          <h1 class="text-white fw-bolder">Selamat Datang Bendahara</h1>
        </div>
      
      
       <div class="card-body">
         <div class="col-md-4 py-2">
          <NuxtLink  to="/admin/tambah" class="btn btn-info text-white m-3 fw-semibold">Bayar</NuxtLink>

          <select v-model="minggu">
            <option v-for="week in weeks" :key="week.id" :value="week.minggu">
              {{ week.minggu }}
            </option>
          </select>

          <button
            @click="logout()"
            class="text-white btn btn-dark m-3 fw-semibold">Logout</button>
          </div>
          <div class="text-muted"> ada {{ dataFilter.length }} data di {{ minggu }}
          </div>
          <table class="table table-striped">
            <thead>
              <tr class="text-white bg-dark">
                <th>No</th>
                <th>nama</th>
                <th>minggu</th>
                <th>nominal</th>
                <th>aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-if="dataFilter < 1">
                <td colspan="4">tidak ada data kas</td>
              </tr>

              <tr v-for="(visitor, i) in dataFilter" :key="visitor.id">
                <td>{{ i + 1 }}.</td>
                <td>{{ visitor.id_siswa.nama }}</td>
                <td>{{ visitor.id_week.minggu }}</td>
                <td>{{ visitor.nominal }}</td>
                <td>{{ visitor.aksi }} <NuxtLink  to="/admin/tambah" class="btn btn-info text-white m-3 fw-semibold">edit</NuxtLink>
                <NuxtLink  to="/admin/hapus" class="btn btn-info text-white m-3 fw-semibold">hapus</NuxtLink></td>
              </tr>
            </tbody>
          </table>
          </div>
      </div>
    </div>
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
const router = useRouter();

console.log(router.currentRoute.value);

async function getData() {
  const { data, error } = await supabase
    .from("kas")
    .select(`id, tanggal, nominal, id_siswa(nama) , id_week(id, minggu)`);
  datas.value = data;
}
async function getWeek() {
  const { data, error } = await supabase.from("week").select("id,minggu");
  weeks.value = data;
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

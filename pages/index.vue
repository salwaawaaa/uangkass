<template>
  <div>
    <h1>Uang Kas</h1>

    <NuxtLink to="/masuk" class="btn btn-primary my-3">Masuk</NuxtLink>
    <table class="table">
      <thead>
        <tr>
          <th>#</th>
          <th>nama</th>
          <th>tanggal</th>
          <th>nominal</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="visitor in datas" :key="visitor.id">
          <td>{{ visitor.id }}</td>
          <td>{{ visitor.id_siswa.nama }}</td>
          <td>{{ visitor.tanggal }}</td>
          <td>{{ visitor.nominal }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const datas = ref([]);

async function getData() {
  const { data, error } = await supabase.from("kas").select(`
  id, tanggal, nominal, id_siswa(nama)
  `);

  datas.value = data;
}

onMounted(() => {
  getData();
});
</script>

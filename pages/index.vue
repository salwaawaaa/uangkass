<template>
    <div class="">
        <div class="container pt-5">
            <div class="card shadow">
                <div class="card-header bg-secondary">
                    <h1 class="text-white fw-bolder">Selamat Datang Bendahara</h1>
                </div>
    <div class="card-body">
      <NuxtLink to="/masuk" class="btn btn-primary my-3 fw-semibold">Masuk</NuxtLink>
      <table class="table mt-2 table-striped nowrap table-border">
        <thead>
          <tr class="text-white bg-secondary">
            <th>No</th>
            <th>nama</th>
            <th>tanggal</th>
            <th>nominal</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(visitor, i) in datas" :key="visitor.id">
            <td>{{ i + 1 }}</td>
            <td>{{ visitor.id_siswa.nama }}</td>
            <td>{{ visitor.tanggal }}</td>
            <td>{{ visitor.nominal }}</td>
          </tr>
        </tbody>
      </table>
      </div>
      </div>
    </div>
    </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const datas = ref([]);

async function getData() {
  const { data, error } = await supabase
    .from("kas")
    .select(
      `
  id, tanggal, nominal, id_siswa(nama)
  `
    )
    .order("id", { ascending: false });

  datas.value = data;
}

onMounted(() => {
  getData();
});
</script>

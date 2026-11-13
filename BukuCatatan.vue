<script setup>
import { ref, computed } from 'vue'

// 1. Variabel untuk menampung input form sementara
const judulBuku = ref('')
const kategoriBuku = ref('Fiksi')
const statusBaca = ref(false)

// 2. Array untuk menyimpan daftar buku yang sudah ditambahkan
const daftarBuku = ref([])

// 3. Fungsi untuk menambahkan buku ke dalam daftar
const tambahBuku = () => {
  if (judulBuku.value.trim() === '') {
    alert('Judul buku tidak boleh kosong!')
    return
  }

  // Masukkan data ke array daftarBuku
  daftarBuku.value.push({
    judul: judulBuku.value,
    kategori: kategoriBuku.value,
    sudahDibaca: statusBaca.value
  })

  // Kosongkan form kembali setelah disimpan
  judulBuku.value = ''
  kategoriBuku.value = 'Fiksi'
  statusBaca.value = false
}

// 4. Computed property untuk menghitung jumlah buku yang sudah selesai dibaca
const totalSelesaiDibaca = computed(() => {
  return daftarBuku.value.filter(buku => buku.sudahDibaca).length
})
</script>

<template>
  <div style="padding: 20px; font-family: Arial;">
    <h2>Formulir Catatan Buku Mandiri</h2>

    <!-- Form Input Buku -->
    <form @submit.prevent="tambahBuku" style="margin-bottom: 20px;">
      <div style="margin-bottom: 10px;">
        <label>Judul Buku: </label>
        <input type="text" v-model="judulBuku" placeholder="Masukkan judul buku..." style="padding: 5px; width: 250px;" />
      </div>

      <div style="margin-bottom: 10px;">
        <label>Kategori Buku: </label>
        <select v-model="kategoriBuku" style="padding: 5px;">
          <option value="Fiksi">Fiksi</option>
          <option value="Non-Fiksi">Non-Fiksi</option>
          <option value="Teknologi">Teknologi</option>
          <option value="Pelajaran">Pelajaran</option>
        </select>
      </div>

      <div style="margin-bottom: 10px;">
        <input type="checkbox" id="status" v-model="statusBaca" />
        <label for="status"> Sudah Selesai Dibaca</label>
      </div>

      <button type="submit" style="padding: 6px 15px; background-color: #42b883; color: white; border: none; cursor: pointer;">
        Tambah Buku
      </button>
    </form>

    <hr />

    <!-- Tampilan Daftar Buku dengan v-for -->
    <h3>Daftar Buku</h3>
    <p>Total Buku Selesai Dibaca: <strong>{{ totalSelesaiDibaca }}</strong> dari {{ daftarBuku.length }} buku</p>

    <ul>
      <li v-for="(buku, index) in daftarBuku" :key="index" style="margin-bottom: 8px;">
        <strong>{{ buku.judul }}</strong> (Kategori: {{ buku.kategori }}) — 
        <span :style="{ color: buku.sudahDibaca ? 'green' : 'red' }">
          {{ buku.sudahDibaca ? 'Sudah Selesai Dibaca' : 'Belum Selesai' }}
        </span>
      </li>
    </ul>
  </div>
</template>
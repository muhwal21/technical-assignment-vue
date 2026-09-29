<script setup>
import { computed, onMounted, ref } from 'vue'
import UserCard from './components/UserCard.vue'

const users = ref([])
const queryPencarian = ref('')
const arahUrutan = ref('asc')
const sedangMemuat = ref(false)
const pesanError = ref('')

async function muatPengguna() {
  sedangMemuat.value = true
  pesanError.value = ''

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`)
    }

    users.value = await response.json()
  } catch (error) {
    pesanError.value = 'Data pengguna gagal dimuat. Silakan coba lagi.'
    console.error(error)
  } finally {
    sedangMemuat.value = false
  }
}

// Computed dari fitur pencarian Session 5.
const penggunaTersaring = computed(() => {
  const kataKunci = queryPencarian.value.trim().toLowerCase()

  if (!kataKunci) {
    return users.value
  }

  return users.value.filter((user) => {
    const nama = user.name?.toLowerCase() ?? ''
    const username = user.username?.toLowerCase() ?? ''

    return nama.includes(kataKunci) || username.includes(kataKunci)
  })
})

// Chained computed: sumbernya penggunaTersaring, bukan users langsung.
const penggunaTerurut = computed(() => {
  const salinan = [...penggunaTersaring.value]

  return salinan.sort((a, b) => {
    const perbandingan = a.name.localeCompare(b.name)
    return arahUrutan.value === 'asc' ? perbandingan : -perbandingan
  })
})

function aturUrutan(arah) {
  arahUrutan.value = arah
}

onMounted(muatPengguna)
</script>

<template>
  <main class="page-shell">
    <section class="dashboard">
      <div class="topbar">
        <a class="brand" href="#" aria-label="Dashboard Vue home">
          <span class="brand__mark">WAD</span>
          <span class="brand__slash">/</span>
          <span>05</span>
        </a>

        <div class="topbar__meta">
          <span>VUE 3</span>
          <span>COMPOSITION API</span>
          <span class="live-dot">LIVE</span>
        </div>
      </div>

      <header class="hero">
        <div class="hero__main">
          <p class="eyebrow">TECHNICAL ASSIGNMENT / USER DIRECTORY</p>
          <h1>Dashboard Vue<br />Pertemuan 5.</h1>
          <p class="hero__description">
            Search it. Sort it. Open the details. Small interactions, local state,
            and chained computed — all in one clean dashboard.
          </p>
        </div>

        <aside class="hero__stat" aria-label="Jumlah pengguna tampil">
          <span class="hero__stat-label">VISIBLE USERS</span>
          <strong>{{ penggunaTerurut.length.toString().padStart(2, '0') }}</strong>
          <span class="hero__stat-caption">RESULTS / NOW</span>
        </aside>
      </header>

      <div class="ticker" aria-hidden="true">
        <span>LOCAL STATE ✦ CHAINED COMPUTED ✦ SEARCH ✦ SORT ✦ VUE 3 ✦</span>
        <span>LOCAL STATE ✦ CHAINED COMPUTED ✦ SEARCH ✦ SORT ✦ VUE 3 ✦</span>
      </div>

      <section class="workspace">
        <div class="section-heading">
          <div>
            <p class="section-kicker">01 / CONTROLS</p>
            <h2>Find your people.</h2>
          </div>
          <p class="section-copy">Cari berdasarkan nama atau username, lalu atur nama A-Z atau Z-A.</p>
        </div>

        <section class="toolbar" aria-label="Kontrol pengguna">
          <button class="primary-button" type="button" @click="muatPengguna">
            <span>Muat Pengguna</span>
            <span aria-hidden="true">↻</span>
          </button>

          <label class="search-field">
            <span class="search-field__label">SEARCH / NAME OR USERNAME</span>
            <span class="search-field__control">
              <span aria-hidden="true">⌕</span>
              <input
                v-model="queryPencarian"
                type="search"
                placeholder="Cari nama atau username..."
              />
            </span>
          </label>

          <div class="sort-control">
            <span class="sort-control__label">SORT / NAME</span>
            <div class="sort-buttons" aria-label="Urutan nama">
              <button
                type="button"
                class="sort-button"
                :class="{ 'sort-button--active': arahUrutan === 'asc' }"
                @click="aturUrutan('asc')"
              >
                A → Z
              </button>

              <button
                type="button"
                class="sort-button"
                :class="{ 'sort-button--active': arahUrutan === 'desc' }"
                @click="aturUrutan('desc')"
              >
                Z → A
              </button>
            </div>
          </div>
        </section>

        <div class="status-strip">
          <span class="status-strip__dot"></span>
          <p>
            Sort aktif: <strong>{{ arahUrutan === 'asc' ? 'A → Z' : 'Z → A' }}</strong>
            <span class="status-strip__divider">/</span>
            hasil pencarian diurutkan berdasarkan nama.
          </p>
        </div>

        <div class="section-heading section-heading--list">
          <div>
            <p class="section-kicker">02 / DIRECTORY</p>
            <h2>People, neatly sorted.</h2>
          </div>
          <span class="count-badge">{{ penggunaTerurut.length }} USERS</span>
        </div>

        <p v-if="sedangMemuat" class="state-message">
          <span class="state-message__icon">↻</span>
          Memuat data pengguna...
        </p>

        <div v-else-if="pesanError" class="state-message state-message--error">
          <div>
            <span class="state-message__icon">!</span>
            <p>{{ pesanError }}</p>
          </div>
          <button class="secondary-button" type="button" @click="muatPengguna">
            Coba Lagi
          </button>
        </div>

        <p v-else-if="penggunaTerurut.length === 0" class="state-message">
          <span class="state-message__icon">0</span>
          Tidak ada pengguna yang cocok dengan pencarian.
        </p>

        <section v-else class="user-list" aria-live="polite">
          <UserCard
            v-for="user in penggunaTerurut"
            :key="user.id"
            :name="user.name"
            :username="user.username"
            :email="user.email"
            :phone="user.phone"
            :company="user.company?.name"
            :city="user.address?.city"
          />
        </section>
      </section>

      <footer class="footer">
        <span>WAD / TECHNICAL ASSIGNMENT</span>
        <span>BUILT WITH VUE 3</span>
      </footer>
    </section>
  </main>
</template>

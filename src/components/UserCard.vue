<script setup>
import { ref } from 'vue'

const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  username: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  phone: {
    type: String,
    default: '-',
  },
  company: {
    type: String,
    default: '-',
  },
  city: {
    type: String,
    default: '-',
  },
})

// Problem 1: state ini sengaja lokal di setiap instance UserCard.
const detailTerbuka = ref(false)

function toggleDetail() {
  detailTerbuka.value = !detailTerbuka.value
}
</script>

<template>
  <article class="user-card" :class="{ 'user-card--open': detailTerbuka }">
    <div class="user-card__header">
      <div class="user-card__avatar" aria-hidden="true">
        <span>{{ props.name.charAt(0) }}</span>
      </div>

      <div class="user-card__content">
        <p class="user-card__eyebrow">USER PROFILE</p>
        <h3>{{ props.name }}</h3>

        <div class="user-card__meta">
          <span class="user-card__username">@{{ props.username }}</span>
          <a class="user-card__email" :href="`mailto:${props.email}`">
            {{ props.email }}
          </a>
        </div>
      </div>

      <button
        class="detail-button"
        type="button"
        :aria-expanded="detailTerbuka"
        @click="toggleDetail"
      >
        <span>{{ detailTerbuka ? 'Sembunyikan' : 'Lihat Detail' }}</span>
        <span class="detail-button__symbol" aria-hidden="true">
          {{ detailTerbuka ? '−' : '+' }}
        </span>
      </button>
    </div>

    <div v-if="detailTerbuka" class="user-card__detail">
      <div class="detail-item">
        <span class="detail-item__label">PHONE</span>
        <strong>{{ props.phone }}</strong>
      </div>
      <div class="detail-item">
        <span class="detail-item__label">COMPANY</span>
        <strong>{{ props.company }}</strong>
      </div>
      <div class="detail-item">
        <span class="detail-item__label">CITY</span>
        <strong>{{ props.city }}</strong>
      </div>
    </div>
  </article>
</template>

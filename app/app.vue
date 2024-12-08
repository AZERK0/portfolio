<script setup lang="ts">
useHead({
  meta: [
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  link: [
    { rel: 'icon', href: '/favicon.ico' }
  ],
  htmlAttrs: {
    lang: 'en'
  }
})

useSeoMeta({
  ogImage: 'https://landing-template.nuxt.dev/social-card.png',
  twitterImage: 'https://landing-template.nuxt.dev/social-card.png',
  twitterCard: 'summary_large_image'
})

const route = useRoute()
const router = useRouter()
const isContactPopupVisible = ref(false)

watch(isContactPopupVisible, (newValue) => {
  if (!newValue) {
    router.push({
      query: {
        ...route.query,
        contactPopup: undefined
      }
    })
  } else {
    router.push({
      query: {
        ...route.query,
        contactPopup: 'true'
      }
    })
  }
})

onMounted(() => {
  isContactPopupVisible.value = route.query.contactPopup === 'true'
})

watch(() => route.query.contactPopup, (newValue) => {
  isContactPopupVisible.value = newValue === 'true'
})
</script>

<template>
  <AppHeader />

  <UModal v-model="isContactPopupVisible">
    <div class="p-4">
      <UCard :ui="{ ring: '', divide: 'divide-y divide-gray-100 dark:divide-gray-800' }">
        <template #header>
          <div class="flex items-center justify-between">
            <h1 class="text-2xl font-semibold leading-6 text-gray-900 dark:text-white">
              Me contacter
            </h1>
            <UButton
              color="gray"
              variant="ghost"
              icon="i-heroicons-x-mark-20-solid"
              class="-my-1"
              @click="isContactPopupVisible = false"
            />
          </div>
        </template>

        <ContactMe />
      </UCard>
    </div>
  </UModal>

  <UMain>
    <NuxtPage />
  </UMain>

  <AppFooter />

  <UNotifications />
</template>

<script setup lang="ts">
const nuxtApp = useNuxtApp()
const { activeHeadings, updateHeadings } = useScrollspy()

const links = computed(() => [{
  label: 'Études',
  to: '#studies',
  icon: 'i-heroicons-cube-transparent',
  active: activeHeadings.value.includes('studies') && !activeHeadings.value.includes('pricing')
}, {
  label: 'Tarifs',
  to: '#pricing',
  icon: 'i-heroicons-credit-card',
  active: activeHeadings.value.includes('pricing') && !activeHeadings.value.includes('projects')
}, {
  label: 'Projets',
  to: '#projects',
  icon: 'i-heroicons-academic-cap',
  active: activeHeadings.value.includes('projects')
}])

nuxtApp.hooks.hookOnce('page:finish', () => {
  updateHeadings([
    document.querySelector('#studies'),
    document.querySelector('#pricing'),
    document.querySelector('#projects')
  ])
})
</script>

<template>
  <UHeader :links="links">
    <template #logo>
      Raphael Charpentier
    </template>

    <template #right>
      <UButton
        label="Me contacter"
        color="white"
        variant="ghost"
        trailing-icon="i-heroicons-arrow-right-20-solid"
        class="hidden lg:flex"
        to="?contactPopup=true"
      />
    </template>

    <template #panel>
      <UAsideLinks :links="links" />

      <UDivider class="my-6" />

      <UButton
        label="Me contacter"
        color="white"
        block
        class="mb-3"
        to="?contactPopup=true"
      />
    </template>
  </UHeader>
</template>

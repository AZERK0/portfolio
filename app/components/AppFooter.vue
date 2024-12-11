<script setup lang="ts">
const links = [
  {
    label: 'Sections',
    children: [{
      label: 'Études',
      to: '#studies'
    }, {
      label: 'Tarifs',
      to: '#pricing'
    }, {
      label: 'Projets',
      to: '#projects'
    }]
  },
  {
    label: 'Contact',
    children: [{
      label: 'raph.chrp@gmail.com',
      to: 'mailto:raph.chrp@gmail.com'
    }, {
      label: 'France, Lyon'
    }]
  }
]

const toast = useToast()

const loading = ref(false)

function onSubmit() {
  loading.value = true

  setTimeout(() => {
    toast.add({
      title: 'Subscribed!',
      description: 'You\'ve been subscribed to our newsletter.'
    })

    loading.value = false
  }, 1000)
}
</script>

<template>
  <UFooter>
    <template #top>
      <UFooterColumns :links="links">
        <template #right>
          <form @submit.prevent="onSubmit">
            <UButton
              label="Me contacter"
              variant="outline"
              color="white"
              block
              to="?contactPopup=true"
            />
          </form>
        </template>
      </UFooterColumns>
    </template>

    <template #left>
      <p class="text-gray-500 dark:text-gray-400 text-sm">
        Copyright © {{ new Date().getFullYear() }}. All rights reserved.
      </p>
    </template>

    <template #right>
      <UColorModeButton size="sm" />

      <UButton
        to="https://github.com/AZERK0"
        target="_blank"
        icon="i-simple-icons-github"
        aria-label="GitHub"
        color="gray"
        variant="ghost"
      />
    </template>
  </UFooter>
</template>

<template>
  <div class="flex flex-col gap-4">
    <div class="grid grid-cols-2 gap-2">
      <UButton
        :ui="{ rounded: 'rounded-md' }"
        icon="i-simple-icons-linkedin"
        size="lg"
        color="blue"
        variant="solid"
        label="LinkedIn"
        :trailing="false"
        to="https://www.linkedin.com/in/raphael-charpentier-38b51720b/"
        target="_blank"
      />
      <UButton
        :ui="{ rounded: 'rounded-md' }"
        icon="i-simple-icons-github"
        size="lg"
        color="gray"
        variant="solid"
        label="GitHub"
        :trailing="false"
        to="https://github.com/AZERK0"
        target="_blank"
      />
      <UButton
        block
        :ui="{ rounded: 'rounded-md' }"
        icon="i-simple-icons-malt"
        size="lg"
        color="red"
        variant="solid"
        label="Malt - Freelance"
        :trailing="false"
        class="col-span-2"
        to="https://www.malt.fr/profile/raphaelcharpentier"
        target="_blank"
      />
    </div>

    <UDivider
      label="Ou"
    />

    <UButton
      block
      :ui="{ rounded: 'rounded-md' }"
      icon="i-heroicons-envelope-solid"
      size="lg"
      variant="solid"
      label="raph.chrp@gmail.com"
      :trailing="false"
      to="mailto:raph.chrp@gmail.com"
    />
    <UButton
      block
      :ui="{ rounded: 'rounded-md' }"
      icon="i-heroicons-phone-solid"
      size="lg"
      color="white"
      variant="solid"
      label="(+33) 6 26 65 53 91"
      :trailing="false"
      to="tel:+33626655391"
    />
  </div>
</template>

<script setup lang="ts">
</script>

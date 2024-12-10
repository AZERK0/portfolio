<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())

useSeoMeta({
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})
</script>

<template>
  <div>
    <div class="hero" />
    <ULandingHero
      :title="page.hero.title"
      :description="page.hero.description"
      :links="page.hero.links"
      orientation="horizontal"
      :ui="{ title: 'text-5xl font-black tracking-tight text-white mix-blend-difference sm:text-7xl' }"
      class="content"
    >
      <template #headline>
        <UBadge
          v-if="page.hero.headline"
          variant="subtle"
          size="lg"
          class="relative rounded-full font-semibold"
        >
          <UIcon
            v-if="page.hero.headline.icon"
            :name="page.hero.headline.icon"
            class="mr-2 w-4 h-4 pointer-events-none"
          />

          {{ page.hero.headline.label }}
        </UBadge>
      </template>

      <div class="relative flex justify-end">
        <div
          class="portrait-background absolute w-4/5 h-full rounded-full mix-blend-difference bg-"
        />
        <NuxtImg
          src="/portrait.webp"
          format="webp"
          alt="Raphael Charpentier"
          class="w-4/5 h-auto border-2 border-gray-600 rounded-full drop-shadow-lg object-contain"
        />
      </div>

      <ULandingLogos
        :title="page.logos.title"
        align="center"
        :ui="{ images: 'mx-auto mt-10 flex flex-wrap items-center justify-center gap-8' }"
        class="sm:col-span-2"
      >
        <UTooltip
          v-for="icon in page.logos.icons"
          :key="icon"
          :text="icon.text"
        >
          <UIcon
            :name="icon.name"
            class="w-10 h-10 lg:w-12 lg:h-12 text-gray-900 dark:text-white"
          />
        </UTooltip>
      </ULandingLogos>
    </ULandingHero>

    <ULandingSection
      :title="page.studies.title"
      :description="page.studies.description"
      :headline="page.studies.headline"
    >
      <div
        id="studies"
        class="flex flex-col sm:flex-row gap-8 scroll-mt-[calc(var(--header-height)+140px+128px+38px)]"
      >
        <template
          v-for="(item, index) in page.studies.items"
          :key="index"
        >
          <UChip
            class="flex-1"
            color="green"
            position="top-left"
            :show="index === page.studies.items.length - 1"
            :ui="{ base: 'absolute rounded-full ring-1 ring-white dark:ring-gray-900 flex items-center justify-center text-white dark:text-gray-900 font-medium whitespace-nowrap animate-ping transform-none' }"
          >
            <ULandingCard
              v-bind="item"
              :ui="{ icon: { base: 'w-20 h-20 -my-7 flex-shrink-0 text-gray-900 dark:text-white' } }"
              class="h-full"
              orientation="horizontal"
            >
              <template #description>
                <span v-html="item.description" />
              </template>
              <NuxtImg
                v-if="item.image"
                :src="item.image"
                format="webp"
                class="w-full rounded-md"
              />
            </ULandingCard>
          </UChip>
          <UDivider
            v-if="index < page.studies.items.length - 1"
            icon="i-heroicons-arrow-right"
            class="w-20"
          />
        </template>
      </div>
    </ULandingSection>

    <ULandingSection
      :title="page.pricing.title"
      :description="page.pricing.description"
      :headline="page.pricing.headline"
      class="pricing-bg"
    >
      <div
        id="pricing"
        class="flex justify-center scroll-mt-[calc(var(--header-height)+140px+128px)]"
      >
        <UPricingCard
          v-bind="page.pricing.plan"
          class="md:w-1/2 lg:w-1/3"
        />
      </div>
    </ULandingSection>

    <ULandingSection
      :headline="page.projects.headline"
      :title="page.projects.title"
      :description="page.projects.description"
    >
      <UBlogList
        id="projects"
        orientation="horizontal"
        class="scroll-mt-[calc(var(--header-height)+140px+128px)]"
      >
        <UBlogPost
          v-for="(project, index) in page.projects.items"
          :key="index"
          v-bind="project"
        >
          <template #description>
            <span v-html="project.description" />
          </template>
        </UBlogPost>
      </UBlogList>
    </ULandingSection>

    <ULandingSection class="bg-primary-50 dark:bg-primary-400 dark:bg-opacity-10">
      <ULandingCTA
        v-bind="page.cta"
        :card="false"
      />
    </ULandingSection>

    <ULandingSection
      id="faq"
      :title="page.faq.title"
      :description="page.faq.description"
      class="scroll-mt-[var(--header-height)]"
    >
      <ULandingFAQ
        multiple
        :items="page.faq.items"
        :ui="{
          button: {
            label: 'font-semibold',
            trailingIcon: {
              base: 'w-6 h-6'
            }
          }
        }"
        class="max-w-4xl mx-auto"
      />
    </ULandingSection>
  </div>
</template>

<style scoped>
.hero {
  width: 100%;
  height: 100%;
  min-height: 100vh;
  position: relative;
  display: flex;
  place-content: center;
  place-items: center;
  --stripes: repeating-linear-gradient(
    100deg,
    black 0%,
    black 7%,
    transparent 10%,
    transparent 12%,
    black 16%
  );

  --rainbow: repeating-linear-gradient(
    100deg,
    #2b2eff 10%,
    #f3450b 15%,
    #2b2eff 20%,
    #0f0278 25%,
    #2b2eff 30%
  );
  background-image: var(--stripes), var(--rainbow);
  background-size: 300%, 200%;
  background-position: 50% 50%, 50% 50%;

  opacity: 0.7;
  filter: blur(10px);

  mask-image: radial-gradient(ellipse at 100% 0%, black 40%, transparent 70%);
  &::after {
    content: "";
    position: absolute;
    inset: 0;
    background-image: var(--stripes), var(--rainbow);
    background-size: 200%, 100%;
    animation: smoothBg 60s linear infinite;
    background-attachment: fixed;
    mix-blend-mode: difference;
  }
}

.light .hero {
  filter: invert(1);

  --stripes: repeating-linear-gradient(
    100deg,
    white 0%,
    white 7%,
    transparent 10%,
    transparent 12%,
    white 16%
  );
}

.content {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}

.portrait-background {
  background: #643500
}

.dark .portrait-background {
  background: #9296ff
}

@keyframes smoothBg {
  from {
    background-position: 50% 50%, 50% 50%;
  }
  to {
    background-position: 350% 50%, 350% 50%;
  }
}

.pricing-bg {
  background:
    linear-gradient(180deg, transparent 82%, rgba(9,9,11,1) 100%),
    radial-gradient(circle at 50% 90%, #f3450b 10%, rgba(43, 46, 255, 0.5) 30%, transparent 40%);
}
</style>

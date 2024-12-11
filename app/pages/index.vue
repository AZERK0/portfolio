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
          class="absolute w-4/5 h-full rounded-full"
          style="backdrop-filter: invert(0.05) grayscale(0.5)"
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

    <ULandingSection class="relative overflow-hidden bg-primary-50 dark:bg-primary-400 dark:bg-opacity-10">
      <ULandingCTA
        v-bind="page.cta"
        :card="false"
      />
      <div class="absolute top-1/2 left-1/2 h-4/5 transform -translate-x-1/2 -translate-y-1/2 rounded-full bg-orange-100 blur-sm mix-blend-difference pointer-events-none spotlight" />
    </ULandingSection>
  </div>
</template>

<style>
/* Variables globales */
:root {
  --stripes-dark: repeating-linear-gradient(
    100deg,
    black 0%,
    black 7%,
    transparent 10%,
    transparent 12%,
    black 16%
  );
  --stripes-light: repeating-linear-gradient(
    100deg,
    white 0%,
    white 7%,
    transparent 10%,
    transparent 12%,
    white 16%
  );
  --rainbow-dark: repeating-linear-gradient(
    100deg,
    #2b2eff 10%,
    #f3450b 15%,
    #2b2eff 20%,
    #0f0278 25%,
    #2b2eff 30%
  );
  --rainbow-light: repeating-linear-gradient(
    100deg,
    #545300 10%,
    #0880a8 15%,
    #545300 20%,
    #a6b05d 25%,
    #545300 30%
  );
}

/* Animation keyframes */
@keyframes smoothBg {
  from {
    background-position: 50% 50%, 50% 50%;
  }
  to {
    background-position: 350% 50%, 350% 50%;
  }
}

@keyframes spotlight-animation {
  0% {
    top: 50%;
    left: 50%;
    scale: 1;
    filter: blur(8px);
  }
  10% {
    filter: blur(24px);
  }
  20% {
    top: 5%;
    left: 35%;
    scale: 0.6;
  }
  50% {
    top: 75%;
    left: 65%;
    scale: 0.8;
    filter: blur(16px);
  }
  70% {
    top: 60%;
    left: 30%;
    scale: 0.6;
    filter: blur(24px);
  }
  100% {
    top: 50%;
    left: 50%;
    scale: 1;
    filter: blur(8px);
  }
}

/* Section Hero */
.hero {
  width: 100%;
  height: 100%;
  min-height: 100vh;
  position: relative;
  display: flex;
  place-content: center;
  place-items: center;
  background-image: var(--stripes-dark), var(--rainbow-dark);
  background-size: 300%, 200%;
  background-position: 50% 50%, 50% 50%;
  opacity: 0.7;
  filter: blur(10px) opacity(50%) saturate(200%);;
  mask-image: radial-gradient(ellipse at 100% 0%, black 40%, transparent 70%);
}

.hero::after {
  content: "";
  position: absolute;
  inset: 0;
  background-image: var(--stripes-dark), var(--rainbow-dark);
  background-size: 200%, 100%;
  background-attachment: fixed;
  animation: smoothBg 60s linear infinite;
  mix-blend-mode: difference;
}

.light .hero {
  filter: blur(10px) invert(100%);
  background-image: var(--stripes-dark), var(--rainbow-light);
}

.light .hero::after {
  background-image: var(--stripes-dark), var(--rainbow-light);
}

/* Section Content */
.content {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}

/* Section Pricing Background */
.pricing-bg {
  background: linear-gradient(180deg, transparent 82%, rgb(9, 9, 11) 100%),
  radial-gradient(
    circle at 50% 90%,
    #f3450b 10%,
    rgba(43, 46, 255, 0.5) 30%,
    transparent 40%
  );
}

.light .pricing-bg {
  background: linear-gradient(180deg, transparent 82%, rgb(250, 250, 250) 100%),
  radial-gradient(
    circle at 50% 90%,
    #f3450b 10%,
    rgba(43, 46, 255, 0.5) 30%,
    transparent 40%
  );
}

/* Section Spotlight */
.spotlight {
  animation: spotlight-animation 6.5s ease-in-out infinite;
  aspect-ratio: 1 / 1;
}
</style>

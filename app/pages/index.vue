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
      <div class="flex flex-col sm:flex-row gap-8 scroll-mt-[calc(var(--header-height)+140px+128px+96px)]">
        <template
          v-for="(item, index) in page.studies.items"
          :key="index"
        >
          <ULandingCard
            v-bind="item"
            class="flex-1"
          >
            <template #description>
              <span v-html="item.description" />
            </template>
          </ULandingCard>
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
    >
      <UPricingGrid
        id="pricing"
        compact
        class="scroll-mt-[calc(var(--header-height)+140px+128px+96px)]"
      >
        <UPricingCard
          v-for="(plan, index) in page.pricing.plans"
          :key="index"
          v-bind="plan"
        />
      </UPricingGrid>
    </ULandingSection>

    <ULandingSection
      :headline="page.testimonials.headline"
      :title="page.testimonials.title"
      :description="page.testimonials.description"
    >
      <UPageColumns
        id="testimonials"
        class="xl:columns-4 scroll-mt-[calc(var(--header-height)+140px+128px+96px)]"
      >
        <div
          v-for="(testimonial, index) in page.testimonials.items"
          :key="index"
          class="break-inside-avoid"
        >
          <ULandingTestimonial v-bind="testimonial" />
        </div>
      </UPageColumns>
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
</style>

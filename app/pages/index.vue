<script setup lang="ts">
const { data: page } = await useAsyncData('index', () => queryContent('/').findOne())

useSeoMeta({
  title: page.value.title,
  ogTitle: page.value.title,
  description: page.value.description,
  ogDescription: page.value.description
})

const projectCategoies = [
  {
    id: 'all',
    label: 'Toutes catégories'
  },
  {
    id: 'perso',
    label: 'Personnel'
  },
  {
    id: 'freelance',
    label: 'Freelance'
  },
  {
    id: 'epitech',
    label: 'Epitech'
  }
]

const projectCategory = ref(projectCategoies[0])

function filteredProjects() {
  return page.value.projects.items.filter(
    project => projectCategory.value.id === 'all' || projectCategory.value.id === project.category
  )
}

const openSlideOver = ref(false)
const slideOverProject = ref(null)
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

      <div class="relative flex justify-center lg:justify-end">
        <NuxtImg
          src="/portrait.webp"
          format="webp"
          alt="Raphael Charpentier"
          class="md:w-3/4 lg:w-11/12 lg:-mt-32 lg:-mb-44 portrait"
        />
      </div>

      <ULandingLogos
        :title="page.logos.title"
        align="center"
        :ui="{ images: 'mx-auto mt-10 flex flex-wrap items-center justify-center gap-8' }"
        class="lg:col-span-2"
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
              :ui="{ icon: { base: 'w-20 h-20 -my-7 flex-shrink-0 text-gray-900 dark:text-white grayscale' } }"
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
            class="w-20 max-sm:hidden"
          />
          <UDivider
            v-if="index < page.studies.items.length - 1"
            icon="i-heroicons-arrow-down"
            class="h-20 sm:hidden"
            orientation="vertical"
          />
        </template>
      </div>
    </ULandingSection>

    <ULandingSection
      :headline="page.projects.headline"
      :title="page.projects.title"
      :description="page.projects.description"
    >
      <div
        id="projects"
        class="flex flex-col gap-6 scroll-mt-[calc(var(--header-height)+140px+128px)]"
      >
        <UFormGroup
          size="xl"
        >
          <USelectMenu
            v-model="projectCategory"
            :options="projectCategoies"
            class="w-60"
          />
        </UFormGroup>

        <UBlogList orientation="horizontal">
          <UBlogPost
            v-for="(project, index) in filteredProjects()"
            :key="index"
            v-bind="project"
            class="cursor-pointer"
            :ui="{ title: 'text-gray-900 dark:text-white text-xl font-semibold text-wrap group-hover:text-gray-600 dark:group-hover:text-gray-300 transition-colors duration-200' }"
            @click="openSlideOver = true; slideOverProject = project"
          >
            <template #description>
              <i>
                Voir le projet
              </i>
            </template>
          </UBlogPost>
        </UBlogList>

        <USlideover
          v-model="openSlideOver"
          :ui="{ width: 'w-screen max-w-xl' }"
        >
          <div class="p-4 flex-1 overflow-auto">
            <UCard
              class="flex flex-col flex-1"
              :ui="{ body: { base: 'flex-1' }, ring: '', divide: 'divide-y divide-gray-100 dark:divide-gray-800' }"
            >
              <template #header>
                <div class="flex items-start justify-between">
                  <div class="flex flex-col gap-4">
                    <h1 class="text-2xl font-bold">
                      {{ slideOverProject.title }}
                    </h1>

                    <time class="text-sm text-gray-500 dark:text-gray-400 font-medium pointer-events-none">
                      {{ slideOverProject.date }}
                    </time>

                    <UBadge
                      v-if="slideOverProject.badge"
                      v-bind="slideOverProject.badge"
                      class="w-fit"
                      variant="soft"
                      size="md"
                    />
                  </div>

                  <UButton
                    color="gray"
                    variant="ghost"
                    icon="i-heroicons-x-mark-20-solid"
                    @click="openSlideOver = false"
                  />
                </div>
              </template>

              <div class="flex flex-col gap-8 flex-1">
                <NuxtImg
                  v-if="!slideOverProject.otherImages"
                  :src="slideOverProject.image"
                  format="webp"
                  class="w-full rounded-lg"
                />

                <UCarousel
                  v-if="slideOverProject.otherImages"
                  v-slot="{ item }"
                  :items="[
                    ...(slideOverProject.video ? [slideOverProject.video] : []),
                    slideOverProject.image,
                    ...slideOverProject.otherImages
                  ]"
                  :ui="{ item: 'basis-full' }"
                  :prev-button="{ color: 'gray' }"
                  :next-button="{ color: 'gray' }"
                  class="rounded-lg overflow-hidden"
                  arrows
                >
                  <NuxtImg
                    v-if="item.includes('webp')"
                    :src="item"
                    class="w-full"
                    draggable="false"
                    format="webp"
                  />
                  <video
                    v-else
                    controls
                    class="w-full rounded-lg"
                    autoplay
                  >
                    <source
                      :src="slideOverProject.video"
                      type="video/mp4"
                    >
                  </video>
                </UCarousel>

                <UButton
                  v-if="slideOverProject.button"
                  v-bind="slideOverProject.button"
                  block
                />

                <UAlert
                  v-if="slideOverProject.alert"
                  v-bind="slideOverProject.alert"
                />

                <strong>Description</strong>

                <span v-html="slideOverProject.description" />
              </div>
            </UCard>
          </div>
        </USlideover>
      </div>
    </ULandingSection>

    <ULandingSection
      :title="page.experience.title"
      :description="page.experience.description"
      :headline="page.experience.headline"
    >
      <div
        id="experiences"
        class="flex flex-col items-center gap-8 scroll-mt-[calc(var(--header-height)+140px+128px)]"
      >
        <ULandingCard
          v-for="(item, index) in page.experience.items"
          :key="index"
          v-bind="item"
          :ui="{ icon: { base: 'w-24 h-24 -my-7 flex-shrink-0 text-gray-900 dark:text-white grayscale' } }"
          class="h-full w-full max-w-xl"
          orientation="horizontal"
        >
          <template #description>
            <span v-html="item.description" />
          </template>
        </ULandingCard>
      </div>
    </ULandingSection>

    <ULandingSection
      :title="page.about.title"
      :headline="page.about.headline"
      :links="page.about.links"
      class="about-bg"
    >
      <div
        id="about"
        class="flex flex-col items-center gap-4 scroll-mt-[calc(var(--header-height)+140px+128px)]"
      >
        <div class="grid grid-cols-3 divide-x divide-primary-500 dark:divide-primary-400 bg-primary-500 dark:bg-primary-400 bg-opacity-5 dark:bg-opacity-5 p-2 mb-8 rounded-lg backdrop-blur-sm ring-1 ring-primary-500 dark:ring-primary-400 dark:ring-opacity-50">
          <div class="px-4">
            <strong>Nom</strong>
            <p>{{ page.about.info.name }}</p>
          </div>
          <div class="px-4">
            <strong>Âge</strong>
            <p>{{ new Date().getFullYear() - new Date(page.about.info.birthdate).getFullYear() }} ans</p>
          </div>
          <div class="px-4">
            <strong>Localisation</strong>
            <p>{{ page.about.info.location }}</p>
          </div>
        </div>

        <p
          class="text-center"
          v-html="page.about.content"
        />
        <i
          class="text-center text-2xl font-bold mt-8"
          v-html="page.about.quote"
        />
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

    <ULandingSection class="relative overflow-hidden bg-indigo-100 dark:bg-indigo-400 dark:bg-opacity-10">
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
    filter: blur(24px);
  }
  10% {
    filter: blur(8px);
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
    filter: blur(8px);
  }
  100% {
    top: 50%;
    left: 50%;
    scale: 1;
    filter: blur(24px);
  }
}

/* Section Hero */
.hero {
  width: 100%;
  height: 100%;
  min-height: 100vh;
  position: absolute;
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

.portrait {
  mask-image: linear-gradient(black 90%, transparent 100%);
}

/* Section About Background */
.dark .about-bg {
  background-color: #0a091a;
  background-image:
    linear-gradient(180deg, transparent 82%, rgb(9, 9, 11) 100%),
    url("https://www.transparenttextures.com/patterns/cartographer.png");
}

.light .about-bg {
  position: relative;
  background-color: #e8efff;
  background-image: linear-gradient(180deg, transparent 82%, rgb(250, 250, 250) 100%);
}

.light .about-bg::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: linear-gradient(180deg, transparent 82%, rgb(250, 250, 250) 100%),
    url("https://www.transparenttextures.com/patterns/cartographer.png");
  opacity: 0.35;
  z-index: 1;
  pointer-events: none;
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

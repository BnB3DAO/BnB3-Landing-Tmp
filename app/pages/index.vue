<script setup lang="ts">
import { useMotion } from "@vueuse/motion";

const { data: page } = await useAsyncData("index", () =>
  queryContent("/").findOne()
);

definePageMeta({
  colorMode: "dark",
});
const wordArr = [
  "Build",
  "Bump",
  "Bullet",
  "Bolster",
  "Brew",
  "Beyond",
  "Belief",
];

const customersRef = ref();
const ctaRef = ref();

onMounted(() => {
  const { variant } = useMotion(customersRef, {
    initial: {
      x: 0,
      opacity: 1,
    },
    enter: {
      x: -20,
      opacity: 1,
      transition: {
        type: "spring",
        duration: 2000,
        onComplete: () => {
          variant.value = "levitate";
        },
      },
    },
    levitate: {
      x: 20,
      opacity: 1,
      transition: {
        duration: 2000,
        repeat: Infinity,
        repeatType: "mirror",
      },
    },
  });
});

useSeoMeta({
  title: page.title,
  ogTitle: page.title,
  description: page.description,
  ogDescription: page.description,
});
</script>

<template>
  <div>
    <ULandingHero :description="page.hero.description" :links="page.hero.links">
      <template #headline>
        <UBadge
          v-if="page.hero.headline"
          variant="subtle"
          size="lg"
          class="relative rounded-full font-semibold"
        >
          <NuxtLink
            :to="page.hero.headline.to"
            target="_blank"
            class="focus:outline-none"
            tabindex="-1"
          >
            <span class="absolute inset-0" aria-hidden="true" />
          </NuxtLink>

          {{ page.hero.headline.label }}

          <UIcon
            v-if="page.hero.headline.icon"
            :name="page.hero.headline.icon"
            class="ml-1 w-4 h-4 pointer-events-none"
          />
        </UBadge>
        <div class="flex justify-center pt-12">
          <div
            class="text-5xl sm:text-7xl font-bold text-primary-300 lg:flex items-center space-x-4"
          >
            <div>Web3 Enthusiasts</div>
            <VueWriter :array="wordArr" class="text-primary-700" />
          </div>
        </div>
      </template>
      <img
        v-motion-pop-visible
        v-if="page.landingImg"
        :src="page.landingImg"
        class="h-full w-full object-cover object-center rounded-xl z-999"
      />
      <ImagePlaceholder v-else />

      <div class="pt-2">
        <div class="text-center font-black text-lg">{{ page.logos.title }}</div>
        <ULandingLogos align="center">
          <div
            v-for="(icon, index) in page.logos.icons"
            v-motion
            :delay="400 * index"
            :initial="{
              opacity: 0,
              y: -90,
            }"
            :visible="{
              opacity: 1,
              y: 0,
              transition: {
                repeatType: 'loop',
                repeatDelay: 5000,
              },
            }"
          >
            <UIcon
              :key="icon"
              :name="icon"
              class="w-10 h-10 lg:w-16 lg:h-16 flex-shrink-0 text-gray-900 dark:text-white"
            />
          </div>
        </ULandingLogos>
      </div>
    </ULandingHero>

    <ULandingSection
      v-motion-pop-visible
      :title="page.features.title"
      :description="page.features.description"
      :headline="page.features.headline"
    >
      <UPageGrid
        id="features"
        class="scroll-mt-[calc(var(--header-height)+140px+128px+96px)]"
      >
        <ULandingCard
          v-for="(item, index) in page.features.items"
          :key="index"
          v-bind="item"
          v-motion
          :delay="200 * index"
          :initial="{
            opacity: 0,
            y: index < 3 ? -30 : 30,
          }"
          :visible="{
            opacity: 1,
            y: 0,
            transition: {
              repeatType: 'loop',
            },
          }"
        />
      </UPageGrid>
    </ULandingSection>

    <ULandingSection
      v-if="false"
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
          v-motion
          :delay="400 * index"
          :initial="{
            opacity: 0,
            x: index < 3 ? -30 : 30,
          }"
          :visible="{
            opacity: 1,
            x: 0,
            transition: {
              repeatType: 'loop',
              stiffness: 100,
              repeatDelay: 5000,
            },
          }"
        >
          <ULandingTestimonial v-bind="testimonial" />
        </div>
      </UPageColumns>
    </ULandingSection>

    <ULandingSection
      class="bg-primary-50 dark:bg-primary-400 dark:bg-opacity-10"
    >
      <ULandingCTA v-bind="page.cta" :card="false" />
    </ULandingSection>

    <ULandingSection
      v-if="false"
      v-motion-slide-visible-top
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
              base: 'w-6 h-6',
            },
          },
        }"
        class="max-w-4xl mx-auto"
      />
    </ULandingSection>
  </div>
</template>

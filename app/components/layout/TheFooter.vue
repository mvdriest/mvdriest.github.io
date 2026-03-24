<script setup lang="ts">
const route = useRoute()

const isContactPage = computed(() => route.path === '/contact')
</script>

<template>
  <footer
    :class="[
      'border-solid border-dark-500 pb-24 text-white',
      isContactPage ? 'bg-gray-200' : 'bg-dark-600',
    ]"
  >
    <LayoutTheContainer>
      <div :class="['rounded-3xl p-12', isContactPage ? 'bg-dark-600' : 'bg-[#2E2E2E]']">

        <nuxt-img
              src="/images/other/mvdriest.svg"
              alt="logo"
              class="w-24"
            />
        <div class="my-12 gap-12 flex flex-col md:flex-row md:justify-between">
          <div class="flex flex-col gap-12 md:flex-row md:gap-20">
            <div class="flex flex-col">
              <h1 class="max-w-sm text-3xl leading-8 font-semibold uppercase">
                Ik ben jouw allround creative.
              </h1>
            </div>
            <div class="pages">
              <h2 class="mb-6 text-xl text-white/70 font-semibold font-family-helvetica -tracking-[1px] uppercase">
                Menu
              </h2>
              <div class="flex flex-col gap-3">
                <NuxtLink
                  to="#top"
                  class="font-family-helvetica text-[2.125em] leading-none font-semibold uppercase tracking-[-0.04em] transition-colors hover:text-primary-600 pointer no-underline"
                >
                  Home
                </NuxtLink>
                <NuxtLink
                  to="#mijnwerk"
                  class="font-family-helvetica text-[2.125em] leading-none font-semibold uppercase tracking-[-0.04em] transition-colors hover:text-primary-600 pointer no-underline"
                >
                  Mijn werk
                </NuxtLink>
                <NuxtLink
                  to="#overmij"
                  class="font-family-helvetica text-[2.125em] leading-none font-semibold uppercase tracking-[-0.04em] transition-colors hover:text-primary-600 pointer no-underline"
                >
                  Over mij
                </NuxtLink>
                <NuxtLink
                  to="#services"
                  class="font-family-helvetica text-[2.125em] leading-none font-semibold uppercase tracking-[-0.04em] transition-colors hover:text-primary-600 pointer no-underline"
                >
                  Services
                </NuxtLink>
                <NuxtLink
                  to="/contact"
                  class="font-family-helvetica text-[2.125em] leading-none font-semibold uppercase tracking-[-0.04em] transition-colors hover:text-primary-600 pointer no-underline"
                >
                  Contact
                </NuxtLink>
              </div>
            </div>
          </div>
          <div>
            <h2 class="mb-6 text-xl text-white/70 font-semibold font-family-helvetica -tracking-[1px] uppercase">
                Email
              </h2>
              <CopyEmailButton email="info@mvdriest.nl" />
          </div>
          <!-- <SocialsList color="yellow" direction="column" /> -->
        </div>
        <div
          class="flex gap-4 flex-col md:flex-row border-t-2 border-solid border-dark-500 pt-5 justify-between items-center"
        >
          <p class="font-family-helvetica text-base text-center text-white/70 leading-none font-semibold uppercase tracking-[-0.04em]">© {{ new Date().getFullYear() }} Martijn van Driest</p>
          <div class="flex gap-4 items-center justify-center flex-col sm:flex-row">
            <a class="font-family-helvetica text-base text-white/70 leading-none font-semibold uppercase tracking-[-0.04em] hover:text-primary-600" href="">Linkedin</a>
            <a class="font-family-helvetica text-base text-white/70 leading-none font-semibold uppercase tracking-[-0.04em] hover:text-primary-600" href="">Instagram</a>
          </div>
        </div>
      </div>
    </LayoutTheContainer>
  </footer>
</template>

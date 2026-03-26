<script lang="ts" setup>
type ContactStatus = {
  type: 'none' | 'pending' | 'success' | 'fail';
  message: String;
};
const contactStatus = ref<ContactStatus>({
  type: 'none',
  message: ''
})

const sendForm = async (e: Event) => {
  const formData = new FormData(e.target as HTMLFormElement)

  contactStatus.value = {
    type: 'pending',
    message: 'Bericht wordt verstuurd...'
  }

  try {
    await $fetch('https://formspree.io/f/myyqynak', {
      method: 'POST',
      body: formData,
      headers: {
        Accept: 'application/json'
      }
    })

    contactStatus.value = {
      type: 'success',
      message: 'Het versturen is gelukt!'
    }
  } catch (err: any) {
    console.error(
      'Er is iets fout gegaan bij het versturen van het formulier',
      err
    )
    if (err?.data?.errors) {
      contactStatus.value = {
        type: 'fail',
        message: err?.data?.errors.map((e: any) => e?.message).join(', ')
      }
      return
    }
    contactStatus.value = {
      type: 'fail',
      message: 'Er is iets fout gegaan. Probeer het later opnieuw!'
    }
  } finally {
    (e.target as HTMLFormElement)?.reset()
  }
}

const submitButtonLabel = computed(() => {
  if (contactStatus.value.type === 'pending') {
    return 'Versturen...'
  }

  if (contactStatus.value.type === 'success') {
    return 'Verzonden'
  }

  return 'Versturen'
})
</script>

<template>
  <section class="pb-16 md:pb-36 bg-gray-200">
    <div class="w-full h-[20rem] md:h-[50lvh] relative">
      <div 
        class="absolute inset-0 bg-cover"
        :style="{
          backgroundImage: 'url(/images/other/camerafotoik.jpg)',
          backgroundPosition: '100% 30%',
          backgroundSize: '130%',
          transform: 'scaleX(-1)',
          filter: 'grayscale(100%) brightness(1) contrast(1.1)'
        }"
      />
      <div class="absolute inset-0 bg-black/45 md:bg-black/15" />
      <div class="absolute inset-0 bg-linear-to-t from-black/65 via-black/20 to-transparent md:from-black/30 md:via-transparent md:to-transparent" />
      <LayoutTheContainer class="relative z-10 flex items-end justify-start h-full">
          <h1 ref="title" class="hero-title uppercase text-4xl max-w-4xl leading-tight md:text-7xl 2xl:text-8xl font-bold text-left text-primary-600 md:leading-18 2xl:leading-[5.8rem] bottom-8 md:bottom-10 md:-tracking-[4px] absolute [text-shadow:0_3px_12px_rgba(0,0,0,0.65)] md:text-shadow-none">
            Welk <br/>
            merk mag ik <br/>
            pimpen?  
          </h1>
      </LayoutTheContainer>
    </div>
    <LayoutTheContainer>
      <div class="grid md:grid-cols-4 gap-8 md:gap-20 mt-10 md:mt-20">
        <div class="flex items-start justify-top flex-col gap-8">
          <div>
            <p class="text-xl text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase opacity-70 pb-2">Email</p>
            <CopyEmailButton email="info@mvdriest.nl" />
          </div>
          <div class="flex items-start justify-top flex-col">
            <p class="text-xl text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase opacity-70 pb-2">Email</p>
            <p class="text-xl text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase pb-2">Kvk: 85431478</p>
            <p class="text-xl text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase">BTW: NL004093393B39</p>
          </div>
        </div>
        <div class="col-span-3 min-w-0">
          <div class="bg-white p-6 md:p-[40px] rounded-lg">
            <h1
              class="text-black max-w-3xl my-0 text-[2.0rem] leading-[50px] font-semibold mb-8"
            >
              Laat een bericht achter
            </h1>
            <p
              v-if="
                contactStatus.type === 'success' || contactStatus.type === 'fail'
              "
              :class="
                contactStatus.type === 'success'
                  ? 'text-green-500'
                  : 'text-red-500'
              "
            >
              {{ contactStatus.message }}
            </p>
            <form class="flex flex-col space-y-5" @submit.prevent="sendForm">
              <div class="flex flex-col space-y-1">
                <label class="text-[18px] text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase">
                  Uw volledige naam:
                </label>
                <input
                  type="name"
                  name="name"
                  class="w-full border placeholder:text-slate-300 bg-gray-100 rounded-md p-2 font-semibold font-family-helvetica -tracking-[1px] text-[18px]"
                  placeholder="John Doe"
                >
              </div>
              <div class="flex flex-col space-y-1">
                <label class="text-[18px] text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase"> Uw email: </label>
                <input
                  type="email"
                  name="email"
                  class="w-full border placeholder:text-slate-300 bg-gray-100 rounded-md p-2 font-semibold font-family-helvetica -tracking-[1px] text-[18px]"
                  placeholder="email@email.com"
                >
              </div>
              <div class="flex flex-col space-y-1">
                <label class="text-[18px] text-dark-800 font-semibold font-family-helvetica -tracking-[1px] uppercase"> Uw bericht: </label>
                <textarea
                  rows="5"
                  name="message"
                  class="w-full border placeholder:text-slate-300 bg-gray-100 rounded-md p-2 font-semibold font-family-helvetica -tracking-[1px] text-[18px]"
                  placeholder="Vertel wat over het project"
                />
              </div>
              <button
                type="submit"
                class="group relative flex min-h-16 w-full items-center justify-center overflow-hidden rounded-[0.85rem] border-0 bg-dark-600 px-6 py-6 pr-[5.2rem] text-white transition-opacity duration-200 disabled:cursor-wait focus-visible:outline-2 focus-visible:outline-offset-4 focus-visible:outline-dark-600 cursor-pointer"
                :data-submit-state="contactStatus.type"
                :disabled="contactStatus.type === 'pending'"
              >
                <span class="flex h-[1.2em] flex-col items-center justify-center overflow-hidden leading-none" aria-hidden="true">
                  <span class="flex flex-col items-center transition-transform duration-[450ms] ease-[cubic-bezier(0.65,0,0,1)] group-hover:-translate-y-[1.2em] group-focus-visible:-translate-y-[1.2em] group-data-[submit-state=success]:-translate-y-[2.4em]">
                    <span class="flex h-[1.2em] items-center whitespace-nowrap font-family-helvetica text-[18px] font-semibold uppercase -tracking-[1px] leading-none">{{ submitButtonLabel }}</span>
                    <span class="flex h-[1.2em] items-center whitespace-nowrap font-family-helvetica text-[18px] font-semibold uppercase -tracking-[1px] leading-none">{{ submitButtonLabel }}</span>
                    <span class="flex h-[1.2em] items-center whitespace-nowrap font-family-helvetica text-[18px] font-semibold uppercase -tracking-[1px] leading-none">{{ submitButtonLabel }}</span>
                  </span>
                </span>

                <span class="sr-only">{{ submitButtonLabel }}</span>

                <span class="absolute right-2 top-1/2 flex size-[3.35rem] -translate-y-1/2 items-center justify-center overflow-hidden rounded-[0.95rem] text-dark-600" aria-hidden="true">
                  <span class="absolute inset-0 bg-primary-600 transition-transform duration-[525ms] ease-[cubic-bezier(0.625,0.05,0,1)] group-hover:rotate-90 group-focus-visible:rotate-90 group-data-[submit-state=success]:rotate-90" />
                  <span class="absolute inset-0 z-1 overflow-hidden rotate-[-45deg]">
                    <span class="absolute inset-0">
                      <svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 10 8" fill="none" class="absolute left-1/2 top-1/2 block size-[1.45rem] p-[0.08em] transition-transform duration-[525ms] ease-[cubic-bezier(0.625,0.05,0,1)] [transform:translate(-50%,-50%)_translateX(0%)] group-hover:[transform:translate(-50%,-50%)_translateX(170%)] group-focus-visible:[transform:translate(-50%,-50%)_translateX(170%)] group-data-[submit-state=success]:[transform:translate(-50%,-50%)_translateX(170%)]"><path d="M4.45231 0.385986H6.02531L9.30131 3.99999L6.02531 7.61399H4.45231L7.40331 4.58499H0.695312V3.42799H7.41631L4.45231 0.385986Z" fill="currentColor" /></svg>
                      <svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 10 8" fill="none" class="absolute left-1/2 top-1/2 block size-[1.45rem] p-[0.08em] transition-transform duration-[525ms] ease-[cubic-bezier(0.625,0.05,0,1)] [transform:translate(-50%,-50%)_translateX(-190%)] group-hover:[transform:translate(-50%,-50%)_translateX(0%)] group-focus-visible:[transform:translate(-50%,-50%)_translateX(0%)] group-data-[submit-state=success]:[transform:translate(-50%,-50%)_translateX(0%)]"><path d="M4.45231 0.385986H6.02531L9.30131 3.99999L6.02531 7.61399H4.45231L7.40331 4.58499H0.695312V3.42799H7.41631L4.45231 0.385986Z" fill="currentColor" /></svg>
                    </span>
                  </span>
                </span>
              </button>
            </form>
          </div>
        </div>
      </div>
      
    </LayoutTheContainer>
  </section>
</template>

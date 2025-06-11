<script setup>
import Navbar from './components/Navbar.vue'
import AboutMe from './components/AboutMe.vue'
import Skills from './components/Skills.vue'
import Projects from './components/Projects.vue'
import Footer from './components/Footer.vue'
import Back2Top from './components/Back2Top.vue'
import ShareButton from './components/ShareButton.vue'

import { useI18n } from 'vue-i18n'
import { watch } from 'vue'

const { t, locale } = useI18n()

function updateMetadata() {
  document.title = t('meta.title')

  const description = document.querySelector('meta[name="description"]')
  if (description) description.setAttribute('content', t('meta.description'))

  const ogTitle = document.querySelector('meta[property="og:title"]')
  if (ogTitle) ogTitle.setAttribute('content', t('meta.title'))

  const ogDescription = document.querySelector('meta[property="og:description"]')
  if (ogDescription) ogDescription.setAttribute('content', t('meta.description'))

  const twitterTitle = document.querySelector('meta[name="twitter:title"]')
  if (twitterTitle) twitterTitle.setAttribute('content', t('meta.title'))

  const twitterDescription = document.querySelector('meta[name="twitter:description"]')
  if (twitterDescription) twitterDescription.setAttribute('content', t('meta.description'))

  document.documentElement.lang = locale.value
}

updateMetadata()
watch(locale, updateMetadata)
</script>

<template>
  <div data-bs-spy="scroll" data-bs-target="#contentNavbar">
    <Navbar />
    <Back2Top />
    <ShareButton />
    <AboutMe />
    <Skills />
    <Projects />
    <Footer />
  </div>
</template>

<style scoped></style>
<script setup lang="ts">
import { ref, watch } from 'vue'
import { useI18n } from 'vue-i18n'

const { locale, fallbackLocale, messages } = useI18n()
const currentLocale = ref(locale.value)

const changeLanguage = () => {
  locale.value = currentLocale.value
  localStorage.setItem('user-locale', currentLocale.value)
}

// Initialize from localStorage if available, otherwise from browser
const savedLocale = localStorage.getItem('user-locale')
const supportedLocales = Object.keys(messages.value)

if (savedLocale) {
  currentLocale.value = savedLocale
  locale.value = savedLocale
} else {
  const browserLocale = navigator.language.split('-')[0]
  if (supportedLocales.includes(browserLocale)) {
    currentLocale.value = browserLocale
    locale.value = browserLocale
  } else {
    currentLocale.value = fallbackLocale.value as string
    locale.value = fallbackLocale.value as string
  }
}
</script>

<template>
  <div class="language-switcher">
    <select v-model="currentLocale" @change="changeLanguage" class="bg-transparent appearance-none focus:outline-none text-sm">
      <option value="pt">{{ $t('common.portuguese') }}</option>
      <option value="en">{{ $t('common.english') }}</option>
      <option value="es">{{ $t('common.spanish') }}</option>
    </select>
  </div>
</template>


<style scoped>
.language-switcher {
  display: inline-block;
}
</style> 
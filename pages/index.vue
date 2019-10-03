<template>
  <div class="container">
    <div v-for="(theme, index) in themedays" :key="index" class="p-4">
      <div class="max-w-sm rounded overflow-hidden shadow-lg scale">
        <img class="h-48 w-full object-cover" :src="theme.img" alt="Sunset in the mountains" />
        <div class="px-6 py-4">
          <div class="font-bold text-xl mb-2">{{ theme.title[locale] }}</div>
          {{ formatDate(theme.date) }} kvar!
        </div>
      </div>
    </div>
    <footer class="fixed absolut bottom-0 right-0 p-4">Data from https://temadagar.se</footer>
  </div>
</template>

<script>
import formatDistanceStrict from 'date-fns/formatDistanceStrict'
import parse from 'date-fns/parse'
import { enGB, eo, ru, sv } from 'date-fns/locale'
import calender from '~/static/kalender.json'

export default {
  components: {},
  data() {
    return {
      themedays: calender.themedays
    }
  },

  computed: {
    locale() {
      return 'sv'
    },

    locales() {
      return { enGB, eo, ru, sv }
    }
  },

  methods: {
    formatDate(toDate) {
      if (!toDate) {
        return ''
      }

      const date = new Date()
      return formatDistanceStrict(
        new Date(date.getFullYear(), date.getMonth(), date.getDate()),
        parse(toDate, 'yyyyMMdd', date, { weekStartsOn: 1 }),
        { locale: this.locales[this.locale] }
      )
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue',
    Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.scale {
  transition-property: all;
  transition-duration: 200ms;
}
.scale:hover {
  transform: scale(1.1);
}
</style>

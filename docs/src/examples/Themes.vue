<template>
  <div class="full-width row justify-center q-pa-md q-gutter-sm">
    <div class="full-width row justify-center items-center q-pa-md q-gutter-sm">

      <q-select
        v-model="selectedTheme"
        :options="themes"
        outlined
        dense
        style="min-width: 250px;"
      />

      <q-checkbox v-model="applyAll" label="Apply to Whole Page" />
    </div>

    <div ref="componentRef" class="full-width bg-surface1">
      <main class="full-width row justify-evenly items-start q-gutter-sm full-width col-11" :class="{ 'bg-surface1': applyAll !== true }">
        <section class="q-mb-lg">
          <div class="row justify-center text-samples">
            <h1 class="text-text1">
              Surface
            </h1>
          </div>
          <div class="surface-samples text-text1">
            <q-card class="bg-surface1 laminate-shadow">1</q-card>
            <q-card class="bg-surface2 laminate-shadow">2</q-card>
            <q-card class="bg-surface3 laminate-shadow">3</q-card>
            <q-card class="bg-surface4 laminate-shadow">4</q-card>
          </div>
        </section>

        <section>
          <div class="text-samples row justify-evenly items-center">
            <h1 class="text-brand1 q-ma-none">
              <span class="swatch bg-brand1 laminate-shadow"></span>
              Brand 1
            </h1>
            <p class="text-brand1">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
          </div>

          <div class="text-samples row justify-evenly items-center">
            <h1 class="text-brand2 q-ma-none">
              <span class="swatch bg-brand2 laminate-shadow"></span>
              Brand 2
            </h1>
            <p class="text-brand2">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
          </div>

          <div class="text-samples row justify-evenly items-center">
            <h1 class="text-text1 q-ma-none">
              <span class="swatch bg-text1 laminate-shadow"></span>
              Text Color 1
            </h1>
            <p class="text-text1">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
          </div>

          <div class="text-samples row justify-evenly items-center">
            <h1 class="text-text2 q-ma-none">
              <span class="swatch bg-text2 laminate-shadow"></span>
              Text Color 2
            </h1>
            <p class="text-text2">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
          </div>
        </section>

      </main>
    </div>

  </div>
</template>

<script>
import { defineComponent, onMounted, onUnmounted, ref, watch, nextTick } from 'vue'
import { useQuasar } from 'quasar'
import useLaminate from 'vue-laminate/src/index.js'
import 'vue-laminate/src/index.scss'
import 'vue-laminate/src/quasar-shim.scss'
import 'vue-laminate/src/themes/all-themes.scss'
import 'vue-laminate/src/aside.scss'
import 'vue-laminate/src/header-footer.scss'

const themes = [
  'default',
  'light',
  'dark',
  'dark-alt',
  'dim-dark',
  'amber',
  'amber-dark',
  'blue',
  'blue-dark',
  'blue-grey',
  'blue-grey-dark',
  'brown',
  'brown-dark',
  'cyan',
  'cyan-dark',
  'indigo',
  'indigo-dark',
  'light-green',
  'light-green-dark',
  'green',
  'green-dark',
  'lime',
  'lime-dark',
  'grey',
  'grey-dark',
  'orange',
  'orange-dark',
  'deep-orange',
  'deep-orange-dark',
  'pink',
  'pink-dark',
  'purple',
  'purple-dark',
  'deep-purple',
  'deep-purple-dark',
  'red',
  'red-dark',
  'teal',
  'teal-dark',
  'yellow',
  'yellow-dark',
  'admin1'
]

export default defineComponent({
  name: 'Themes',

  setup () {
    const componentRef = ref(null),
      applyAll = ref(false),
      $q = useQuasar(),
      selectedTheme = ref('default')

    const { laminate } = useLaminate({ useCache: true, cacheName: 'selected-theme' })

    watch(selectedTheme, val => {
      laminate.setTheme(val)
    })

    watch(applyAll, val => {
      if (val) {
        laminate.setElement(document.body)
        nextTick(() => {
          $q.dark.set(laminate.themeName.value.indexOf('dark') > -1)
        })
      }
      else {
        laminate.setElement(componentRef.value)
        $q.dark.set('auto')
      }
    })

    watch (laminate.themeName, val => {
      if (applyAll.value === true) {
        nextTick(() => {
          $q.dark.set(val.indexOf('dark') > -1)
        })
      }
      else {
        $q.dark.set('auto')
      }
    })

    onMounted(() => {
      laminate.onMounted()
      laminate.setElement(componentRef.value)
    })

    onUnmounted(() => {
      laminate.setElement(null)
      $q.dark.set('auto')
    })

    return {
      componentRef,
      applyAll,
      selectedTheme,
      themes
    }

  }
})
</script>

<style lang="scss">
.surface-samples {
  display: grid;
  --size: 16ch;
  grid-template-columns: var(--size) var(--size);
  grid-auto-rows: var(--size);
  gap: 2ch;

  & > * {
    border-radius: 1rem;
    display: grid;
    place-content: center;
    font-size: 3rem;
    font-weight: 200;
  }
}

.text-samples {
  display: grid;
  gap: 1.5ch;

  & > h1 {
    font-size: 2.5rem;
    display: inline-flex;
    align-items: center;
    gap: 1ch;
    line-height: 2.5rem;
    font-weight: 100;
  }

  & > p {
    max-inline-size: 35ch;
    font-size: 1.25rem;
    line-height: 1.5;
  }
}

.swatch {
  display: inline-block;
  flex-shrink: 0;
  inline-size: 1.5ch;
  block-size: 1.5ch;
  border-radius: 50%;
}
</style>
<template>
  <div class="q-pa-md">
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

    <q-layout ref="componentRef" view="hHh Lpr lff" container style="height: 300px" class="shadow-2 rounded-borders bg-surface1">
      <q-header elevated class="bg-black">
        <q-toolbar>
          <q-btn flat @click="drawer = !drawer" round dense icon="menu" />
          <q-toolbar-title>Header</q-toolbar-title>
        </q-toolbar>
      </q-header>

      <q-drawer
        v-model="drawer"
        show-if-above
        :width="200"
        :breakpoint="500"
        bordered
      >
        <q-scroll-area class="fit">
          <q-list>

            <template v-for="(menuItem, index) in menuList" :key="index">
              <q-item clickable :active="menuItem.label === 'Outbox'" v-ripple>
                <q-item-section avatar>
                  <q-icon :name="menuItem.icon" />
                </q-item-section>
                <q-item-section>
                  {{ menuItem.label }}
                </q-item-section>
              </q-item>
              <q-separator :key="'sep' + index"  v-if="menuItem.separator" />
            </template>

          </q-list>
        </q-scroll-area>
      </q-drawer>

      <q-page-container>
        <q-page padding>
          <p v-for="n in 15" :key="n">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Fugit nihil praesentium molestias a adipisci, dolore vitae odit, quidem consequatur optio voluptates asperiores pariatur eos numquam rerum delectus commodi perferendis voluptate?
          </p>
        </q-page>
      </q-page-container>
    </q-layout>
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
// import 'vue-laminate/src/section.scss'

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

const menuList = [
  {
    icon: 'inbox',
    label: 'Inbox',
    separator: true
  },
  {
    icon: 'send',
    label: 'Outbox',
    separator: false
  },
  {
    icon: 'delete',
    label: 'Trash',
    separator: false
  },
  {
    icon: 'error',
    label: 'Spam',
    separator: true
  },
  {
    icon: 'settings',
    label: 'Settings',
    separator: false
  },
  {
    icon: 'feedback',
    label: 'Send Feedback',
    separator: false
  },
  {
    icon: 'help',
    iconColor: 'primary',
    label: 'Help',
    separator: false
  }
]

export default {
  setup () {
    const componentRef = ref(null),
      applyAll = ref(false),
      $q = useQuasar(),
      selectedTheme = ref('default')

    const { laminate } = useLaminate({ useCache: true, cacheName: 'layout-theme' })

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
        laminate.setElement(componentRef.value.$el || componentRef.value)
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
      laminate.setElement(componentRef.value.$el || componentRef.value)
    })

    onUnmounted(() => {
      laminate.setElement(null)
      $q.dark.set('auto')
    })

    return {
      componentRef,
      applyAll,
      themes,
      selectedTheme,
      drawer: ref(false),
      menuList
    }
  }
}
</script>
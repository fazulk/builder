<script lang="ts" setup>
import { Content, fetchOneEntry, isPreviewing } from '@builder.io/sdk-vue/vue3'
import { onMounted, ref } from 'vue'
import '@builder.io/sdk-vue/vue3/css'
import HelloWorldComponent from './components/HelloWorld.vue'

// Register your Builder components
const REGISTERED_COMPONENTS = [
  {
    component: HelloWorldComponent,
    name: 'Hello World',
    canHaveChildren: false,
    inputs: [
      {
        name: 'some-message',
        type: 'string',
        defaultValue: 'replace with something good',
      },
    ],
  },
]

const canShowContent = ref(false)
const content = ref<any>(null)
const apiKey = import.meta.env.VITE_PUB_API

onMounted(() => {
  fetchOneEntry({
    model: 'page',
    apiKey: import.meta.env.VITE_PUB_API,
    userAttributes: {
      urlPath: window.location.pathname,
    },
  }).then((res) => {
    content.value = res
    canShowContent.value = content.value || isPreviewing()
  })
})
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125">
  </header>
  <div>
    <div v-if="canShowContent">
      <Content
        model="page"
        :content="content"
        :api-key="apiKey"
        :custom-components="REGISTERED_COMPONENTS"
      />
    </div>
    <div v-else>
      Content not Found
    </div>
  </div>
</template>

<style>
#app {
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}
</style>

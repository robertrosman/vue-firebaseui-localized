<script setup>
  import { onMounted, onUnmounted, ref, defineProps, defineEmits } from 'vue'
  const container = ref(null)
  const error = ref()
  let script
  let app
  
  const emit = defineEmits(['error', 'loaded'])
  
  const props = defineProps({
    auth: Object,
    firebase: Object,
    config: Object,
    version: String,
    lang: String,
    rtl: Boolean
  })
  
  const eject = (err) => {
    if (err) {
      error.value = err
      emit('error', err)
    }
    script.remove()
  }
  
  const onScriptLoaded = async () => {
    if (app) await app.delete()
    container.value.innerHTML = ''
    const firebaseUI =
      window.firebaseui.auth.AuthUI.getInstance() ||
      new window.firebaseui.auth.AuthUI(props.auth)
    firebaseUI.start(`#firebaseui-auth-container`, props.config)
    app = window.firebase.app('[DEFAULT]-firebaseui-temp')
    emit('loaded')
  }

  onMounted(() => {
    window.firebase = props.firebase
    script = document.createElement('script')
    script.src = `https://www.gstatic.com/firebasejs/ui/${props.version}/firebase-ui-auth__${props.lang}.js`
    script.async = true
    script.addEventListener('error', eject)
    script.addEventListener('load', onScriptLoaded)
    document.body.appendChild(script)
  })

  onUnmounted(eject)
  
</script>

<template>
  <div id="firebaseui-auth-container" ref="container">
    <slot v-if="error" name="error">Couldn't load firebase ui</slot>
    <slot v-else name="loading">Loading...</slot>
  </div>
  <link
    type="text/css"
    rel="stylesheet"
    :href="`https://www.gstatic.com/firebasejs/ui/${version}/firebase-ui-auth${
      props.rtl ? '-rtl' : ''
    }.css`"
  />
</template>
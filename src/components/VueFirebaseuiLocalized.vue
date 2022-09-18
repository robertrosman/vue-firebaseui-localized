<script setup>
import { onMounted, onUnmounted, ref, defineProps, defineEmits } from "vue";
import LoadingContainer from "./LoadingContainer.vue";
const container = ref(null);
const error = ref();
let script;
let app;

const emit = defineEmits(["error", "loaded"]);

const props = defineProps({

  /** 
   * Your configuration options for Firebase UI, see referece https://github.com/firebase/firebaseui-web#configuration
   */
  config: {
    Object,
    required: true
  },

  /** 
   * The firebase ui version you are using. NOTE: Must be the same version as your package.json version
   */
  version: {
    String,
    required: true
  },

  /**
   * The language you want to use, for supported languages and their codes, look here: https://github.com/firebase/firebaseui-web/blob/master/LANGUAGES.md
   */
  lang: {
    String,
    required: true
  },

  /** 
   * Pass in firebase that you have imported from firebase/compat/app
   */
  firebase: {
    type: Object,
    required: true
  },

  /** 
   * Pass in your auth object that you get from getAuth(app)
   */
  auth: {
    type: Object,
    required: true
  },

  /** 
   * Set to true if you're using a right-to-left language
   */
  rtl: {
    Boolean,
    required: false
  },
});

const eject = (err) => {
  if (err) {
    error.value = err;
    emit("error", err);
  }
  script.remove();
};

const onScriptLoaded = async () => {
  if (app) await app.delete();
  container.value.innerHTML = "";
  const firebaseUI =
    window.firebaseui.auth.AuthUI.getInstance() ||
    new window.firebaseui.auth.AuthUI(props.auth);
  firebaseUI.start(`#firebaseui-auth-container`, props.config);
  app = window.firebase.app("[DEFAULT]-firebaseui-temp");
  emit("loaded");
};

onMounted(() => {
  window.firebase = props.firebase;
  script = document.createElement("script");
  script.src = `https://www.gstatic.com/firebasejs/ui/${props.version}/firebase-ui-auth__${props.lang}.js`;
  script.async = true;
  script.addEventListener("error", eject);
  script.addEventListener("load", onScriptLoaded);
  document.body.appendChild(script);
});

onUnmounted(eject);
</script>

<template>
  <div id="firebaseui-auth-container" ref="container">
    <slot v-if="error" name="error">Couldn't load firebase ui</slot>
    <slot v-else name="loading">
      <LoadingContainer :button-amount="config.signInOptions.length" />
    </slot>
  </div>
  <link
    type="text/css"
    rel="stylesheet"
    :href="`https://www.gstatic.com/firebasejs/ui/${version}/firebase-ui-auth${
      props.rtl ? '-rtl' : ''
    }.css`"
  />
</template>

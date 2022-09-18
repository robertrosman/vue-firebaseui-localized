<script setup>
import VueFirebaseuiLocalized from "./components/VueFirebaseuiLocalized.vue";
import firebase from "firebase/compat/app";
import * as firebaseui from "firebaseui";
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

const app = initializeApp({
  apiKey: "AIzaSyAfYQebgYopm3nX8gjkpsG0RD4EZLeoIoQ",
});
const auth = getAuth(app);

const config = {
  signInSuccessUrl: "/",
  signInOptions: [
    firebase.auth.EmailAuthProvider.PROVIDER_ID,
    firebase.auth.GoogleAuthProvider.PROVIDER_ID,
  ],
};
</script>

<template>
  <VueFirebaseuiLocalized
    version="6.0.1"
    lang="sv"
    :config="config"
    :firebase="firebase"
    :auth="auth"
  />
</template>

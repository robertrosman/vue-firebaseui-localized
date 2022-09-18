# vue-firebaseui-localized

Vue wrapper over firebaseui with simpler localization. Conceptual port of [react-firebaseui-localized](https://github.com/greg-schrammel/react-firebaseui-localized) to vue.

Instead of building all languages etc, the javascript bundles are fetched from the official cdn.


Installation
------------

`npm i vue-firebaseui-localized firebase firebaseui`

 or

`yarn add vue-firebaseui-localized firebase firebaseui`


Usage
-----

Put this code where you want Firebase UI to be located:

```
<script setup>
import VueFirebaseuiLocalized from "vue-firebaseui-localized";
import firebase from "firebase/compat/app";
import * as firebaseui from "firebaseui";
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

const app = initializeApp({
  apiKey: "your-unique-api-key-here",
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
```

Props
-----

| prop     | required | notes                                                                                                                                                 |
|----------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| config   | true     | Your configuration options for Firebase UI, see referece https://github.com/firebase/firebaseui-web#configuration                                     |
| version  | true     | The firebase ui version you are using. NOTE: Must be the same version as your package.json version                                                    |
| lang     | true     | The language you want to use, for supported languages and their codes, look here: https://github.com/firebase/firebaseui-web/blob/master/LANGUAGES.md |
| firebase | true     | Pass in firebase that you have imported from firebase/compat/app                                                                                      |
| auth     | true     | Pass in your auth object that you get from getAuth(app)                                                                                               |
| rtl      | false    | Set to true if you're using a right-to-left language                                                                                                  |

Slots
-----

| slot    | notes                                                            |
|---------|------------------------------------------------------------------|
| loading | Message or content to show while loading the javascript from cdn |
| error   | Message or content to show if loading Firebase UI fails          |

Events
------

| event  | arguments | notes                                                      |
|--------|-----------|------------------------------------------------------------|
| loaded |           | Firebase UI is loaded                                      |
| error  | Error     | Couldn't load Firebase UI, the error is passed as argument |
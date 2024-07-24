<script setup>
import {OidcClient} from '@axa-fr/oidc-client'
import { ref } from 'vue';

const configuration = {
    client_id: 'xxxxx',
    redirect_uri: window.location.origin + '/oidc/callback',
    silent_redirect_uri: window.location.origin + '/oidc/silent-callback',
    scope: 'openid',
    authority: 'xxxx',
    refresh_time_before_tokens_expiration_in_second: 40,
    service_worker_relative_url: '/OidcServiceWorker.js',
    service_worker_only: true,
  };

  const href = window.location.href;


const token = ref(null)

  const vanillaOidc = OidcClient.getOrCreate(() => fetch)(configuration);


  vanillaOidc.tryKeepExistingSessionAsync().then(() => {
    token.value = vanillaOidc.tokens;
  });



  function logout() {
    vanillaOidc.logoutAsync();
  }
  function login() {
    vanillaOidc.loginAsync('/');
  }
  if (href.includes(configuration.redirect_uri)) {
    // @ts-ignore
    vanillaOidc.loginCallbackAsync()
  }
</script>

<template>
  <main>
    <button @click="login">login</button>
    <button @click="logout">logout</button>

    <pre>
      {{ token }}
    </pre>
  </main>
</template>

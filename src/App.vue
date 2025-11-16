<script setup lang="ts">

import { SignedIn, SignedOut, SignInButton, UserButton, useClerk } from '@clerk/vue'
const clerk = useClerk()

async function copyToken() {
  try {
    // Obtener la sesión activa
    const session = clerk.value?.session
    console.log('Sesión:', session)
    if (!session) throw new Error('No hay sesión activa')
    
    // Obtener el token JWT
    const token = await session.getToken()
    console.log('Token:', token)
    
    await navigator.clipboard.writeText(token)
  } catch (error) {
    console.error('Error:', error)
  }
}
</script>

<template>
  <header>
    <SignedOut>
      <SignInButton />
    </SignedOut>
    <SignedIn>
      <UserButton />
    </SignedIn>
  </header>
  
  <button @click="copyToken">Copiar token para Postman</button>
</template>
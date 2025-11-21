<script setup lang="ts">
import { watch } from 'vue'
import { SignedIn, SignedOut, SignInButton, UserButton, useClerk, useUser, useAuth } from '@clerk/vue'

const clerk = useClerk()

// --- LÓGICA DE AUTO-ACTIVACIÓN DE ORGANIZACIÓN ---
// 1. Usamos useUser en lugar de useOrganizationList
const { user, isLoaded: isUserLoaded } = useUser()
const { orgId, isLoaded: isAuthLoaded } = useAuth()

// 2. Observamos cambios.
watch([isUserLoaded, isAuthLoaded, user, orgId], () => {
  // Verificamos que todo haya cargado y que tengamos un usuario logueado
  if (isUserLoaded.value && isAuthLoaded.value && user.value) {
    
    // Si actualmente NO hay una organización seleccionada (orgId es undefined)...
    if (!orgId.value) {
      // 3. Accedemos a las membresías desde el objeto user
      const memberships = user.value.organizationMemberships
      
      if (memberships && memberships.length > 0) {
        const firstOrgId = memberships[0].organization.id
        console.log('⚠️ No hay Org activa. Auto-activando:', firstOrgId)
        
        // Usamos la instancia de clerk para activar la organización
        clerk.value?.setActive({ organization: firstOrgId })
      }
    }
  }
})

// --- TU FUNCIÓN DE COPIAR TOKEN ---
async function copyToken() {
  try {
    const session = clerk.value?.session
    console.log('Sesión:', session)
    
    if (!session) throw new Error('No hay sesión activa')
    
    // Al obtener el token AHORA, ya debería venir con la org_id y la metadata
    const token = await session.getToken() ?? 'No hay token'
    console.log('Token:', token)
    
    await navigator.clipboard.writeText(token)
    alert('Token copiado al portapapeles')
  } catch (error) {
    console.error('Error:', error)
  }
}
</script>

<template>
  <header>
    <SignedOut>
      <!-- Si no estás logueado, muestra botón de login -->
      <SignInButton />
    </SignedOut>
    
    <SignedIn>
      <!-- Si estás logueado, muestra tu avatar -->
      <div style="display: flex; gap: 10px; align-items: center;">
        <UserButton />
      </div>
    </SignedIn>
  </header>
  
  <br>
  <SignedIn>
    <button @click="copyToken">Copiar token para Postman</button>
  </SignedIn>
</template>
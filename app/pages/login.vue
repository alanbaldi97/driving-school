<template>
  <div class="space-y-6">
    <UCard>
      <template #header>
        <h2 class="text-xl font-semibold text-gray-900 dark:text-white">
          Iniciar Sesión
        </h2>
      </template>
      
      <UForm :state="state" class="space-y-4" @submit="onSubmit">
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" type="email" placeholder="tu@email.com" />
        </UFormGroup>

        <UFormGroup label="Contraseña" name="password">
          <UInput v-model="state.password" type="password" placeholder="••••••••" />
        </UFormGroup>

        <UButton type="submit" color="primary" block>
          Iniciar Sesión
        </UButton>
      </UForm>
    </UCard>
  </div>
</template>

<script setup lang="ts">
// Usar el layout de autenticación en lugar del default
definePageMeta({
  layout: 'auth'
})

const state = reactive({
  email: '',
  password: ''
})

const onSubmit = () => {
  console.log('Login:', state)
  // Aquí iría la lógica de autenticación
}
</script>

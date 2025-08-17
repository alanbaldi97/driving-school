<template>
  <div class="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 dark:from-gray-900 dark:to-gray-800">
    <div class="flex min-h-screen items-center justify-center px-4 py-12 sm:px-6 lg:px-8">
      <div class="w-full max-w-md space-y-8">
        <!-- Logo -->
        <div class="text-center">
          <UIcon name="i-heroicons-home-modern" class="mx-auto h-12 w-12 text-primary" />
          <h2 class="mt-6 text-3xl font-bold tracking-tight text-gray-900 dark:text-white">
            Dashboard
          </h2>
        </div>
        
        <!-- Contenido de la página de autenticación -->
        <NuxtPage />
      </div>
    </div>
  </div>
</template>

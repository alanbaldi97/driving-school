<template>
  <div class="flex flex-col h-full">
    <!-- Logo del sidebar -->
    <div class="flex items-center justify-between p-4 border-b border-slate-200/60 dark:border-slate-700/60">
      <div class="flex items-center space-x-3">
        <div class="w-6 h-6 rounded-lg bg-gradient-to-br from-blue-500 to-blue-600 flex items-center justify-center">
          <UIcon name="i-heroicons-home-modern" class="w-4 h-4 text-white" />
        </div>
        <span class="text-lg font-semibold bg-gradient-to-r from-slate-900 to-slate-700 dark:from-white dark:to-slate-300 bg-clip-text text-transparent">
          Dashboard
        </span>
      </div>
      <UButton
        icon="i-heroicons-x-mark"
        variant="ghost"
        size="sm"
        class="lg:hidden hover:bg-slate-100 dark:hover:bg-slate-800 text-slate-600 dark:text-slate-400"
        @click="$emit('close')"
      />
    </div>

    <!-- Navegación principal -->
    <nav class="flex-1 p-4 space-y-3 overflow-y-auto">
      <div class="space-y-2">
        <h3 class="px-2 text-xs font-medium text-slate-500 dark:text-slate-400 uppercase tracking-wide">
          Principal
        </h3>
        
        <UButton
          to="/"
          variant="ghost"
          class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
          :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300 border-r-2 border-cyan-500': $route.path === '/' }"
        >
          <template #leading>
            <UIcon name="i-heroicons-home" class="w-4 h-4" />
          </template>
          {{ $t('nav.dashboard') }}
        </UButton>

        <UButton
          to="/analytics"
          variant="ghost"
          class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
          :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300 border-r-2 border-cyan-500': $route.path === '/analytics' }"
        >
          <template #leading>
            <UIcon name="i-heroicons-chart-bar" class="w-4 h-4" />
          </template>
          {{ $t('nav.analytics') }}
        </UButton>

        <!-- Menú con Submenús: Proyectos -->
        <div class="space-y-1">
          <UButton
            variant="ghost"
            class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
            :class="{ 
              'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': projectsOpen || $route.path.startsWith('/projects')
            }"
            @click="toggleProjects"
          >
            <template #leading>
              <UIcon name="i-heroicons-folder" class="w-4 h-4" />
            </template>
            {{ $t('nav.projects') }}
            <template #trailing>
              <UIcon 
                :name="projectsOpen ? 'i-heroicons-chevron-down' : 'i-heroicons-chevron-right'" 
                class="w-4 h-4 transition-transform duration-200"
              />
            </template>
          </UButton>
          
          <!-- Submenús de Proyectos -->
          <Transition
            enter-active-class="transition-all duration-300 ease-out"
            enter-from-class="opacity-0 transform -translate-y-2 scale-95"
            enter-to-class="opacity-100 transform translate-y-0 scale-100"
            leave-active-class="transition-all duration-200 ease-in"
            leave-from-class="opacity-100 transform translate-y-0 scale-100"
            leave-to-class="opacity-0 transform -translate-y-2 scale-95"
          >
            <div v-if="projectsOpen" class="ml-4 space-y-1 border-l-2 border-slate-200 dark:border-slate-700 pl-2">
              <UButton
                to="/projects/active"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/projects/active' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-play-circle" class="w-3 h-3" />
                </template>
                Proyectos Activos
              </UButton>
              
              <UButton
                to="/projects/completed"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/projects/completed' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-check-circle" class="w-3 h-3" />
                </template>
                Completados
              </UButton>
              
              <UButton
                to="/projects/archived"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/projects/archived' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-archive-box" class="w-3 h-3" />
                </template>
                Archivados
              </UButton>
            </div>
          </Transition>
        </div>

        <!-- Menú con Submenús: Equipo -->
        <div class="space-y-1">
          <UButton
            variant="ghost"
            class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
            :class="{ 
              'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': teamOpen || $route.path.startsWith('/team')
            }"
            @click="toggleTeam"
          >
            <template #leading>
              <UIcon name="i-heroicons-users" class="w-4 h-4" />
            </template>
            {{ $t('nav.team') }}
            <template #trailing>
              <UIcon 
                :name="teamOpen ? 'i-heroicons-chevron-down' : 'i-heroicons-chevron-right'" 
                class="w-4 h-4 transition-transform duration-200"
              />
            </template>
          </UButton>
          
          <!-- Submenús de Equipo -->
          <Transition
            enter-active-class="transition-all duration-300 ease-out"
            enter-from-class="opacity-0 transform -translate-y-2 scale-95"
            enter-to-class="opacity-100 transform translate-y-0 scale-100"
            leave-active-class="transition-all duration-200 ease-in"
            leave-from-class="opacity-100 transform translate-y-0 scale-100"
            leave-to-class="opacity-0 transform -translate-y-2 scale-95"
          >
            <div v-if="teamOpen" class="ml-4 space-y-1 border-l-2 border-slate-200 dark:border-slate-700 pl-2">
              <UButton
                to="/team/members"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/team/members' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-user-group" class="w-3 h-3" />
                </template>
                Miembros
              </UButton>
              
              <UButton
                to="/team/roles"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/team/roles' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-identification" class="w-3 h-3" />
                </template>
                Roles y Permisos
              </UButton>
              
              <UButton
                to="/team/departments"
                variant="ghost"
                size="sm"
                class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-md transition-all duration-200"
                :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/team/departments' }"
              >
                <template #leading>
                  <UIcon name="i-heroicons-building-office" class="w-3 h-3" />
                </template>
                Departamentos
              </UButton>
            </div>
          </Transition>
        </div>
      </div>

      <div class="pt-4 space-y-2">
        <h3 class="px-2 text-xs font-medium text-slate-500 dark:text-slate-400 uppercase tracking-wide">
          Herramientas
        </h3>
        
        <UButton
          to="/settings"
          variant="ghost"
          class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
          :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/settings' }"
        >
          <template #leading>
            <UIcon name="i-heroicons-cog-8-tooth" class="w-4 h-4" />
          </template>
          {{ $t('nav.settings') }}
        </UButton>

        <UButton
          to="/help"
          variant="ghost"
          class="w-full justify-start hover:bg-cyan-50 dark:hover:bg-cyan-900/20 hover:text-cyan-700 dark:hover:text-cyan-300 rounded-lg transition-all duration-200"
          :class="{ 'bg-cyan-50 dark:bg-cyan-900/20 text-cyan-700 dark:text-cyan-300': $route.path === '/help' }"
        >
          <template #leading>
            <UIcon name="i-heroicons-question-mark-circle" class="w-4 h-4" />
          </template>
          {{ $t('nav.help') }}
        </UButton>
      </div>
    </nav>

    <!-- Información del usuario -->
    <div class="p-4 border-t border-slate-200/60 dark:border-slate-700/60">
      <div class="flex items-center space-x-3 p-2 rounded-lg bg-slate-50/50 dark:bg-slate-800/30">
        <UAvatar
          src="https://avatars.githubusercontent.com/u/904724?v=4"
          alt="Usuario"
          size="sm"
          class="ring-2 ring-blue-200 dark:ring-blue-800"
        />
        <div class="flex-1 min-w-0">
          <p class="text-sm font-medium text-slate-900 dark:text-white truncate">
            John Doe
          </p>
          <p class="text-xs text-slate-500 dark:text-slate-400 truncate">
            john@example.com
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineEmits(['close'])

// Estados para controlar los submenús
const projectsOpen = ref(false)
const teamOpen = ref(false)

// Funciones para toggle de submenús
const toggleProjects = () => {
  projectsOpen.value = !projectsOpen.value
  // Opcionalmente cerrar otros submenús
  if (projectsOpen.value) {
    teamOpen.value = false
  }
}

const toggleTeam = () => {
  teamOpen.value = !teamOpen.value
  // Opcionalmente cerrar otros submenús
  if (teamOpen.value) {
    projectsOpen.value = false
  }
}
</script>

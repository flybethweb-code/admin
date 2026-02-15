<template>
  <div class="min-h-screen bg-gray-50 flex font-sans text-brand-gray">
    <!-- Sidebar -->
    <aside class="w-72 bg-brand-blue text-white flex-shrink-0 flex flex-col shadow-2xl z-40 relative overflow-hidden">
      <!-- Decorator Background -->
      <div class="absolute -right-24 -top-24 h-64 w-64 rounded-full bg-white/5 blur-3xl" />
      <div class="absolute -left-24 -bottom-24 h-64 w-64 rounded-full bg-brand-green/10 blur-3xl" />

      <div class="p-8 pb-10 flex items-center space-x-3 relative z-10">
        <div class="h-10 w-10 bg-white rounded-xl flex items-center justify-center shadow-lg transform rotate-3 hover:rotate-0 transition-premium cursor-pointer">
          <PaperAirplaneIcon class="h-6 w-6 text-brand-blue" />
        </div>
        <div class="flex flex-col">
          <span class="text-2xl font-serif font-black tracking-tighter text-white">FLYBETH</span>
          <span class="text-[9px] font-black uppercase tracking-[0.3em] text-brand-green/80 -mt-1">Super Portal</span>
        </div>
      </div>
      
      <nav class="mt-4 flex-grow px-4 space-y-2 relative z-10">
        <div class="px-4 mb-4">
           <p class="text-[10px] font-black text-white/30 uppercase tracking-[0.2em]">Main Menu</p>
        </div>
        <NuxtLink 
          v-for="item in navigation" 
          :key="item.name" 
          :to="item.href"
          class="flex items-center px-5 py-3.5 text-sm font-bold rounded-2xl transition-premium group relative"
          :class="[route.path.startsWith(item.href) && (item.href !== '/dashboard' || route.path === '/dashboard') ? 'bg-white text-brand-blue shadow-xl shadow-black/10' : 'text-white/60 hover:text-white hover:bg-white/5']"
        >
          <component :is="item.icon" class="h-5 w-5 mr-4 flex-shrink-0 transition-premium group-hover:scale-110" />
          {{ item.name }}
          <div v-if="route.path.startsWith(item.href) && (item.href !== '/dashboard' || route.path === '/dashboard')" class="absolute right-4 h-1.5 w-1.5 rounded-full bg-brand-green" />
        </NuxtLink>
      </nav>

      <div class="p-6 relative z-10">
        <UiBaseCard padding class="!bg-white/5 !border-white/10 !rounded-3xl hover:!bg-white/10 transition-premium cursor-pointer">
          <div class="flex items-center space-x-4">
            <div class="h-10 w-10 rounded-xl bg-brand-green flex items-center justify-center text-white font-black shadow-lg shadow-brand-green/20">SA</div>
            <div class="min-w-0 flex-1">
              <p class="text-xs font-black text-white truncate">Super Admin</p>
              <p class="text-[10px] text-white/50 font-medium truncate uppercase tracking-widest mt-0.5">Administrator</p>
            </div>
          </div>
        </UiBaseCard>
        
        <button 
          @click="logout" 
          class="mt-6 flex items-center justify-center space-x-3 w-full py-4 text-xs font-black text-white/40 hover:text-white transition-premium uppercase tracking-widest border border-white/5 rounded-2xl hover:bg-red-500/20 hover:border-red-500/30"
        >
          <ArrowLeftOnRectangleIcon class="h-4 w-4" />
          <span>Secure Sign Out</span>
        </button>
      </div>
    </aside>

    <!-- Main Content -->
    <div class="flex-grow flex flex-col min-w-0 h-screen overflow-hidden">
      <!-- Top header -->
      <header class="bg-white/80 backdrop-blur-md border-b border-gray-100 h-20 flex items-center justify-between px-10 sticky top-0 z-30">
        <div class="flex items-center space-x-4">
           <h2 class="text-2xl font-serif font-black text-brand-blue tracking-tight">{{ currentPageTitle }}</h2>
        </div>
        
        <div class="flex items-center space-x-10">
          <div class="hidden lg:flex items-center bg-gray-50 border border-transparent focus-within:border-brand-blue/20 focus-within:bg-white rounded-2xl px-5 py-2.5 transition-premium group min-w-[320px]">
            <MagnifyingGlassIcon class="h-4 w-4 text-brand-gray/40 mr-3 group-focus-within:text-brand-blue" />
            <input type="text" placeholder="Search for bookings, agents, or invoices..." class="bg-transparent border-none text-sm font-medium focus:outline-none w-full placeholder:text-brand-gray/30 text-brand-blue">
          </div>
          
          <div class="flex items-center space-x-3">
            <button class="h-11 w-11 flex items-center justify-center rounded-xl text-brand-gray/50 hover:bg-gray-50 hover:text-brand-blue transition-premium relative bg-white border border-gray-50 shadow-sm">
              <BellIcon class="h-5 w-5" />
              <span class="absolute top-3 right-3 h-2 w-2 bg-brand-green ring-4 ring-white rounded-full"></span>
            </button>
            <button class="h-11 w-11 flex items-center justify-center rounded-xl text-brand-gray/50 hover:bg-gray-50 hover:text-brand-blue transition-premium bg-white border border-gray-50 shadow-sm">
              <Cog6ToothIcon class="h-5 w-5" />
            </button>
          </div>
        </div>
      </header>

      <!-- Page content -->
      <main class="flex-1 overflow-y-auto p-10 scroll-smooth">
        <slot />
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  Squares2X2Icon as HomeIcon, 
  UsersIcon, 
  BuildingOfficeIcon as TenantIcon,
  TicketIcon, 
  BanknotesIcon as RevenueIcon,
  ShieldCheckIcon as RolesIcon,
  Cog6ToothIcon,
  ArrowLeftOnRectangleIcon,
  BellIcon,
  MagnifyingGlassIcon,
  PaperAirplaneIcon
} from '@heroicons/vue/24/outline'

const route = useRoute()

const navigation = [
  { name: 'Overview', href: '/dashboard', icon: HomeIcon },
  { name: 'Agents (Tenants)', href: '/tenants', icon: TenantIcon },
  { name: 'Users', href: '/users', icon: UsersIcon },
  { name: 'All Bookings', href: '/bookings', icon: TicketIcon },
  { name: 'Financials', href: '/revenue', icon: RevenueIcon },
  { name: 'Roles & Access', href: '/roles', icon: RolesIcon },
  { name: 'Settings', href: '/settings', icon: Cog6ToothIcon },
]

const currentPageTitle = computed(() => {
  const item = navigation.find(n => route.path.startsWith(n.href))
  return item ? item.name : 'Super Admin'
})

const logout = () => {
  navigateTo('/')
}
</script>

<style>
/* Custom Scrollbar for better UX */
::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-track {
  @apply bg-transparent;
}
::-webkit-scrollbar-thumb {
  @apply bg-gray-200 rounded-full hover:bg-brand-blue/20 transition-premium;
}
</style>

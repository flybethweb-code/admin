<template>
  <div class="space-y-10 pb-12">
    <div class="flex flex-col md:flex-row md:items-end justify-between gap-6">
       <div class="space-y-1">
         <h1 class="text-4xl font-serif font-black text-brand-blue tracking-tight">Agent Management</h1>
         <p class="text-brand-gray/60 font-medium">Review, approve, and oversee all travel agency tenants on the Flybeth platform</p>
       </div>
       <UiBaseButton variant="primary" size="lg" @click="showAddAgent = true">
         <PlusIcon class="h-5 w-5 mr-2" />
         Vetting New Agent
       </UiBaseButton>
    </div>

    <!-- Filters & Search -->
    <UiBaseCard padding class="!py-4">
      <div class="flex flex-col lg:flex-row gap-8 items-center">
        <div class="w-full lg:flex-grow max-w-2xl">
          <UiBaseInput 
            v-model="searchQuery" 
            placeholder="Search by legal name, owner email, or agency slug..."
            :icon="MagnifyingGlassIcon"
          />
        </div>
        <div class="flex items-center space-x-6">
          <div class="flex flex-col space-y-1">
            <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-widest ml-1">Membership Status</span>
            <select class="bg-white border border-gray-100 rounded-xl py-2.5 px-6 text-sm font-bold text-brand-blue focus:ring-4 focus:ring-brand-blue/5 shadow-sm outline-none transition-premium hover:border-brand-blue/30 cursor-pointer min-w-[200px]">
              <option>All Partners</option>
              <option>Fully Verified</option>
              <option>Pending Verification</option>
              <option>Suspended Access</option>
            </select>
          </div>
          <div class="flex flex-col space-y-1">
            <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-widest ml-1">Sorted By</span>
            <select class="bg-white border border-gray-100 rounded-xl py-2.5 px-6 text-sm font-bold text-brand-blue focus:ring-4 focus:ring-brand-blue/5 shadow-sm outline-none transition-premium hover:border-brand-blue/30 cursor-pointer min-w-[180px]">
              <option>Highest Volume</option>
              <option>Recently Joined</option>
              <option>Revenue Share</option>
            </select>
          </div>
        </div>
      </div>
    </UiBaseCard>

    <!-- Tenants Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-8">
      <UiBaseCard 
        v-for="agent in agents" 
        :key="agent.id" 
        padding 
        class="group relative hover:-translate-y-2 transition-premium"
      >
         <div class="flex items-start justify-between mb-8">
           <div class="h-16 w-16 rounded-3xl bg-brand-blue/[0.03] border border-brand-blue/5 flex items-center justify-center font-black text-2xl text-brand-blue shadow-inner group-hover:bg-brand-blue group-hover:text-white group-hover:shadow-2xl group-hover:shadow-brand-blue/20 transition-premium uppercase transform group-hover:rotate-6">
             {{ agent.name.split(' ').map(n => n[0]).join('').substring(0, 2) }}
           </div>
           <span class="px-4 py-1.5 rounded-full text-[10px] font-black uppercase tracking-widest inline-block shadow-sm" :class="statusClass(agent.status)">
             {{ agent.status }}
           </span>
         </div>
         
         <div class="mb-8">
           <h3 class="text-xl font-serif font-black text-brand-blue group-hover:text-brand-blue transition-premium">{{ agent.name }}</h3>
           <p class="text-xs font-bold text-brand-gray/40 mt-1 uppercase tracking-widest">{{ agent.email }}</p>
         </div>
         
         <div class="grid grid-cols-2 gap-4 pt-8 border-t border-gray-50 mb-8">
           <div class="flex flex-col">
             <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-[0.15em]">Total Yield</span>
             <span class="text-xl font-black text-brand-blue tracking-tighter mt-1 hover:text-brand-green transition-premium cursor-pointer">${{ agent.revenue }}</span>
           </div>
           <div class="flex flex-col">
             <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-[0.15em]">Total Volume</span>
             <span class="text-xl font-black text-brand-blue tracking-tighter mt-1">{{ agent.bookings }}</span>
           </div>
         </div>

         <div class="flex items-center space-x-3">
           <UiBaseButton variant="secondary" size="md" class="flex-1 !rounded-2xl !py-4 font-black">
             Partner View
           </UiBaseButton>
           <UiBaseButton variant="outline" size="md" class="flex-none !rounded-2xl !py-4 !px-4 hover:!bg-brand-blue hover:!text-white group/gear">
             <Cog6ToothIcon class="h-5 w-5 transition-premium group-hover/gear:rotate-90" />
           </UiBaseButton>
         </div>

         <div class="mt-6 flex justify-center border-t border-gray-50 pt-4 opacity-50 hover:opacity-100 transition-premium">
            <button v-if="agent.status === 'Pending'" class="text-[11px] font-black text-brand-green uppercase tracking-[0.2em] flex items-center hover:scale-105 transition-premium">
              <CheckBadgeIcon class="h-4 w-4 mr-2" />
              Approve Credentials
            </button>
            <button v-else-if="agent.status === 'Active'" class="text-[11px] font-black text-red-400 uppercase tracking-[0.2em] flex items-center hover:scale-105 transition-premium">
              <NoSymbolIcon class="h-4 w-4 mr-2" />
              Restrict Agency Access
            </button>
         </div>
      </UiBaseCard>
    </div>

    <!-- Add Agent Side Drawer -->
    <UiSideDrawer 
      :show="showAddAgent" 
      title="Partner Vetting" 
      @close="showAddAgent = false"
    >
      <div class="space-y-10">
        <p class="text-sm text-brand-gray/70 leading-relaxed font-medium">Verify and add a new travel agency to the Flybeth Global Network.</p>
        
        <div class="space-y-6">
          <UiBaseInput label="Legal Agency Name" placeholder="Agency Registration Name" :icon="BuildingOfficeIcon" />
          <UiBaseInput label="Partner Primary Email" type="email" placeholder="owner@agency.com" :icon="EnvelopeIcon" />
          <UiBaseInput label="Agency Website/Slug" placeholder="agency-name" :icon="GlobeAltIcon" />
          
          <div class="bg-gray-50/50 p-6 rounded-[2rem] border border-dashed border-gray-200 group hover:border-brand-blue/30 transition-premium cursor-pointer text-center">
             <CloudIcon class="h-8 w-8 mx-auto text-brand-gray/30 group-hover:text-brand-blue transition-premium" />
             <p class="text-[10px] font-black text-brand-gray/40 uppercase tracking-widest mt-2">Upload Registration Documents (PDF)</p>
          </div>
        </div>
      </div>

      <template #footer>
        <div class="flex gap-4 w-full">
          <UiBaseButton variant="ghost" class="flex-1" @click="showAddAgent = false">Dismiss</UiBaseButton>
          <UiBaseButton variant="primary" class="flex-1">Initiate Vetting</UiBaseButton>
        </div>
      </template>
    </UiSideDrawer>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { 
  MagnifyingGlassIcon, 
  PlusIcon,
  Cog6ToothIcon,
  CheckBadgeIcon,
  NoSymbolIcon,
  BuildingOfficeIcon,
  EnvelopeIcon,
  GlobeAltIcon,
  CloudIcon
} from '@heroicons/vue/24/outline'

definePageMeta({
  layout: 'admin'
})

const showAddAgent = ref(false)
const searchQuery = ref('')

const statusClass = (status: string) => {
  switch (status) {
    case 'Active': return 'bg-brand-green/10 text-brand-green border border-brand-green/20'
    case 'Pending': return 'bg-yellow-100 text-yellow-600 border border-yellow-200'
    case 'Suspended': return 'bg-red-50 text-red-500 border border-red-100'
    default: return 'bg-gray-100 text-gray-600'
  }
}

const agents = [
  { id: 1, name: 'Elite Travel Group', email: 'contact@elitetravel.com', status: 'Active', bookings: '1,204', revenue: '450,300' },
  { id: 2, name: 'Global Voyages', email: 'hello@globalvoyages.net', status: 'Active', bookings: '945', revenue: '312,250' },
  { id: 3, name: 'SkyHigh Agency', email: 'support@skyhigh.io', status: 'Suspended', bookings: '782', revenue: '280,400' },
  { id: 4, name: 'Budget Holidays', email: 'bookings@budgethols.co.uk', status: 'Pending', bookings: '412', revenue: '120,500' },
  { id: 5, name: 'Luxe Escapes', email: 'vip@luxeescapes.com', status: 'Active', bookings: '1,560', revenue: '890,100' },
  { id: 6, name: 'Discovery Travel', email: 'info@discovery.travel', status: 'Active', bookings: '320', revenue: '85,400' },
]
</script>

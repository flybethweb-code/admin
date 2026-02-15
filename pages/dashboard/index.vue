<template>
  <div class="space-y-10 pb-12">
    <!-- Main Stats Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <UiBaseCard 
        v-for="stat in stats" 
        :key="stat.name" 
        padding 
        class="group hover:scale-[1.02] transition-premium"
      >
        <div class="flex items-center space-x-4">
          <div class="p-4 rounded-2xl transition-premium group-hover:rotate-6 shadow-sm" :class="stat.bgClass">
            <component :is="stat.icon" class="h-6 w-6" :class="stat.iconClass" />
          </div>
          <div>
            <p class="text-[10px] font-black text-brand-gray/40 uppercase tracking-[0.2em] mb-1 leading-none">{{ stat.name }}</p>
            <div class="flex items-baseline space-x-2">
              <h3 class="text-2xl font-serif font-black text-brand-blue leading-none">{{ stat.value }}</h3>
              <span class="text-[10px] font-bold" :class="stat.trendClass">{{ stat.trend }}</span>
            </div>
          </div>
        </div>
      </UiBaseCard>
    </div>

    <!-- Charts & System Section -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-10">
      <!-- System Health -->
      <UiBaseCard padding title="System Health Monitoring">
        <template #header>
          <div class="flex items-center justify-between w-full">
            <h3 class="text-xl font-serif font-black text-brand-blue">Core Infrastructure</h3>
            <span class="flex items-center text-[10px] font-black text-brand-green bg-brand-green/5 border border-brand-green/10 px-4 py-1.5 rounded-full uppercase tracking-widest shadow-sm">
              <div class="h-2 w-2 bg-brand-green rounded-full mr-2 animate-pulse shadow-[0_0_8px_rgba(50,180,4,0.5)]"></div>
              Network Stable
            </span>
          </div>
        </template>
        
        <div class="space-y-8 mt-4">
          <div v-for="service in services" :key="service.name" class="flex items-center justify-between group">
            <div class="flex items-center space-x-4">
              <div class="h-12 w-12 rounded-2xl bg-gray-50 flex items-center justify-center border border-gray-100 group-hover:border-brand-blue/30 transition-premium shadow-sm">
                <component :is="service.icon" class="h-5 w-5 text-brand-gray/40 group-hover:text-brand-blue transition-premium" />
              </div>
              <div class="flex flex-col">
                <span class="text-sm font-black text-brand-blue">{{ service.name }}</span>
                <span class="text-[10px] font-bold text-brand-gray/40 uppercase tracking-widest">Active connection</span>
              </div>
            </div>
            <div class="flex items-center space-x-1.5 px-4 h-8 bg-gray-50/50 rounded-full border border-gray-50">
               <div v-for="i in 15" :key="i" class="h-4 w-[3px] rounded-full transition-premium" :class="i > 13 ? 'bg-yellow-400 group-hover:bg-yellow-500' : 'bg-brand-green group-hover:shadow-[0_0_5px_rgba(50,180,4,0.3)]'"></div>
            </div>
            <span class="text-xs font-black text-brand-blue">{{ service.uptime }}%</span>
          </div>
        </div>
      </UiBaseCard>

      <!-- Revenue Distribution -->
      <UiBaseCard padding class="!bg-brand-blue relative overflow-hidden flex flex-col justify-between">
         <!-- Card Decoration -->
         <div class="absolute right-0 bottom-0 p-12 opacity-[0.05] pointer-events-none">
            <TicketIcon class="h-64 w-64 text-white" />
         </div>

         <div class="flex items-center justify-between relative z-10 mb-10">
            <h3 class="text-xl font-serif font-black text-white">Yield Performance</h3>
            <UiBaseButton variant="ghost" class="!bg-white/5 !text-white/60 hover:!bg-white/10 !px-4 !py-2 !rounded-xl !border-transparent shadow-none">
              View All Reports
            </UiBaseButton>
         </div>

         <div class="h-48 flex items-end justify-between space-x-3 relative z-10 mb-6">
            <div v-for="(v, i) in [40, 70, 45, 90, 65, 80, 50]" :key="i" class="flex-grow rounded-2xl bg-white/10 hover:bg-brand-green transition-premium cursor-pointer relative group flex items-end justify-center" :style="{ height: v + '%' }">
              <span class="absolute -top-10 text-xs font-black text-white opacity-0 group-hover:opacity-100 transition-premium transform -translate-y-2 group-hover:translate-y-0 whitespace-nowrap bg-black/30 backdrop-blur-md px-3 py-1.5 rounded-xl shadow-xl">${{ v }}k</span>
            </div>
         </div>

         <div class="flex justify-between relative z-10 text-[9px] font-black text-white/30 uppercase tracking-[0.25em] px-2">
            <span>Stays</span>
            <span>Flights</span>
            <span>Cars</span>
            <span>Pkgs</span>
            <span>Activities</span>
            <span>Cruises</span>
            <span>Other</span>
         </div>
      </UiBaseCard>
    </div>

    <!-- Active Agents Overview -->
    <UiBaseCard>
      <div class="p-8 border-b border-gray-50 flex flex-col md:flex-row md:items-center justify-between gap-6">
        <div>
          <h3 class="text-2xl font-serif font-black text-brand-blue">Agent Marketplace Performance</h3>
          <p class="text-sm text-brand-gray/50 font-medium mt-1">High-performing travel agencies currently scaling</p>
        </div>
        <UiBaseButton variant="secondary" size="md" @click="navigateTo('/tenants')">
          Browse Global Database
          <ArrowRightIcon class="h-4 w-4 ml-2" />
        </UiBaseButton>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-left">
          <thead>
            <tr class="bg-gray-50/50">
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Agency Brand</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Operation Status</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Bookings (MTD)</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Total Contribution</th>
              <th class="px-8 py-6 text-right text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Actions</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-50">
            <tr v-for="agent in topAgents" :key="agent.name" class="hover:bg-gray-50/50 transition-premium group">
              <td class="px-8 py-8">
                <div class="flex items-center space-x-4">
                  <div class="h-12 w-12 rounded-2xl bg-brand-blue/5 border border-brand-blue/5 flex items-center justify-center font-black text-brand-blue transition-premium group-hover:bg-brand-blue group-hover:text-white group-hover:rotate-2 shadow-sm">
                    {{ agent.name[0] }}
                  </div>
                  <div>
                    <div class="text-sm font-black text-brand-blue">{{ agent.name }}</div>
                    <div class="text-[11px] font-bold text-brand-gray/40 mt-0.5">{{ agent.email }}</div>
                  </div>
                </div>
              </td>
              <td class="px-8 py-8">
                <span class="px-4 py-1.5 rounded-full text-[10px] font-black uppercase tracking-widest inline-block text-center min-w-[100px]" :class="agent.status === 'Active' ? 'bg-brand-green/10 text-brand-green' : 'bg-yellow-100 text-yellow-600'">
                  {{ agent.status }}
                </span>
              </td>
              <td class="px-8 py-8 font-black text-brand-blue text-sm tracking-tight">{{ agent.bookings }}</td>
              <td class="px-8 py-8">
                <span class="font-black text-brand-blue text-base tracking-tighter">${{ agent.revenue }}</span>
              </td>
              <td class="px-8 py-8 text-right">
                <button class="bg-white border border-gray-100 text-brand-gray/30 hover:bg-brand-blue hover:text-white h-10 w-10 flex items-center justify-center rounded-xl transition-premium shadow-sm">
                  <ArrowTopRightOnSquareIcon class="h-4 w-4" />
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </UiBaseCard>
  </div>
</template>

<script setup lang="ts">
import { 
  BuildingOfficeIcon, 
  BanknotesIcon, 
  TicketIcon, 
  GlobeAltIcon,
  CloudIcon,
  ServerIcon,
  ShieldCheckIcon,
  EllipsisHorizontalIcon,
  ArrowRightIcon,
  ArrowUpIcon,
  ArrowTopRightOnSquareIcon
} from '@heroicons/vue/24/outline'

definePageMeta({
  layout: 'admin'
})

const stats = [
  { name: 'Global Gross Revenue', value: '$2.42M', trend: '+18.5%', trendClass: 'text-brand-green', icon: BanknotesIcon, bgClass: 'bg-brand-green/10', iconClass: 'text-brand-green' },
  { name: 'Verified Agencies', value: '1,240', trend: '+5.2%', trendClass: 'text-brand-green', icon: BuildingOfficeIcon, bgClass: 'bg-brand-blue/10', iconClass: 'text-brand-blue' },
  { name: 'Active Inventory', value: '45.2k', trend: '+12.1%', trendClass: 'text-brand-green', icon: TicketIcon, bgClass: 'bg-indigo-50', iconClass: 'text-indigo-600' },
  { name: 'Unique Global Travelers', value: '180k', trend: '-2.4%', trendClass: 'text-red-400', icon: GlobeAltIcon, bgClass: 'bg-amber-50', iconClass: 'text-amber-600' },
]

const services = [
  { name: 'Central API Cluster', uptime: '99.99', icon: ServerIcon },
  { name: 'Global GDS Gateway', uptime: '99.95', icon: GlobeAltIcon },
  { name: 'Property Management Engine', uptime: '100', icon: BuildingOfficeIcon },
  { name: 'Secure Settlement Layer', uptime: '99.98', icon: ShieldCheckIcon },
]

const topAgents = [
  { name: 'Elite Travel Group', email: 'contact@elitetravel.com', status: 'Active', bookings: '1,204', revenue: '450,300' },
  { name: 'Global Voyages', email: 'hello@globalvoyages.net', status: 'Active', bookings: '945', revenue: '312,250' },
  { name: 'SkyHigh Agency', email: 'support@skyhigh.io', status: 'Active', bookings: '782', revenue: '280,400' },
  { name: 'Budget Holidays', email: 'bookings@budgethols.co.uk', status: 'Pending', bookings: '412', revenue: '120,500' },
]
</script>

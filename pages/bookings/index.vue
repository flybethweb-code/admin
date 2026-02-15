<template>
  <div class="space-y-10 pb-12">
    <!-- Header Section -->
    <div class="flex flex-col md:flex-row md:items-end justify-between gap-6">
       <div class="space-y-1">
         <h1 class="text-4xl font-serif font-black text-brand-blue tracking-tight">Global Bookings</h1>
         <p class="text-brand-gray/60 font-medium">Monitor and manage all bookings across all agents</p>
       </div>
       <div class="flex items-center gap-3">
         <UiBaseButton variant="outline" size="md">
           <ArrowDownTrayIcon class="h-4 w-4 mr-2" />
           Export CSV
         </UiBaseButton>
         <UiBaseButton variant="primary" size="lg" @click="showNewBooking = true">
           <PlusIcon class="h-5 w-5 mr-2" />
           New Direct Booking
         </UiBaseButton>
       </div>
    </div>

    <!-- Stats Overview -->
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
      <UiBaseCard padding class="group relative overflow-hidden">
        <div class="absolute right-0 top-0 p-8 opacity-[0.03] group-hover:scale-110 transition-premium">
          <CalendarDaysIcon class="h-24 w-24 text-brand-blue" />
        </div>
        <p class="text-[10px] font-black text-brand-gray uppercase tracking-[0.2em] mb-1">Total Bookings Today</p>
        <h4 class="text-3xl font-serif font-black text-brand-blue">142</h4>
        <div class="mt-4 flex items-center text-[10px] font-bold text-brand-green">
          <ArrowUpIcon class="h-3 w-3 mr-1" /> 12% vs yesterday
        </div>
      </UiBaseCard>
      
      <UiBaseCard padding>
        <p class="text-[10px] font-black text-brand-gray uppercase tracking-[0.2em] mb-1">Pending Confirmations</p>
        <h4 class="text-3xl font-serif font-black text-brand-blue">28</h4>
        <div class="mt-4 flex items-center text-[10px] font-bold text-brand-blue opacity-50 font-medium">
          Awaiting agent action
        </div>
      </UiBaseCard>

      <UiBaseCard padding>
        <p class="text-[10px] font-black text-brand-gray uppercase tracking-[0.2em] mb-1">Weekly Gross Volume</p>
        <h4 class="text-3xl font-serif font-black text-brand-blue">$82,405</h4>
        <div class="mt-4 flex items-center text-[10px] font-bold text-brand-green">
          <ArrowUpIcon class="h-3 w-3 mr-1" /> 8.4% growth
        </div>
      </UiBaseCard>

      <UiBaseCard padding>
        <p class="text-[10px] font-black text-brand-gray uppercase tracking-[0.2em] mb-1">Success Rate</p>
        <h4 class="text-3xl font-serif font-black text-brand-blue">94.2%</h4>
        <div class="mt-4 bg-gray-100 h-1.5 rounded-full overflow-hidden">
          <div class="bg-brand-green h-full" style="width: 94.2%"></div>
        </div>
      </UiBaseCard>
    </div>

    <!-- Filter & Search Bar -->
    <UiBaseCard padding class="!py-4">
      <div class="flex flex-col lg:flex-row gap-6 items-center">
        <div class="w-full lg:flex-grow max-w-xl">
          <UiBaseInput 
            v-model="searchQuery" 
            placeholder="Search by Reference, PNR, Customer Name..."
            :icon="MagnifyingGlassIcon"
          />
        </div>
        
        <div class="flex flex-wrap items-center gap-6">
          <div v-for="filter in filters" :key="filter.label" class="flex flex-col space-y-1">
            <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-widest ml-1">{{ filter.label }}</span>
            <select class="bg-white border border-gray-100 rounded-xl py-2.5 px-4 text-sm font-bold text-brand-blue focus:ring-4 focus:ring-brand-blue/5 outline-none transition-premium hover:border-brand-blue/30 cursor-pointer min-w-[140px]">
              <option v-for="opt in filter.options" :key="opt">{{ opt }}</option>
            </select>
          </div>
          
          <div class="flex flex-col space-y-1">
            <span class="text-[10px] font-black text-brand-gray/40 uppercase tracking-widest ml-1">Custom Date</span>
            <div class="w-40">
              <UiDatePicker v-model="selectedDate" />
            </div>
          </div>
        </div>
      </div>
    </UiBaseCard>

    <!-- Table Section -->
    <UiBaseCard class="border-none shadow-xl shadow-brand-blue/5">
      <div class="overflow-x-auto">
        <table class="w-full border-collapse text-left">
          <thead>
            <tr class="bg-gray-50/50">
              <th class="px-8 py-6 w-12"><input type="checkbox" class="rounded-lg border-gray-200 text-brand-blue focus:ring-brand-blue h-5 w-5 transition-premium"></th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Reference/PNR</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Service Details</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Agent Info</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Status</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Total Price</th>
              <th class="px-8 py-6 text-right text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Actions</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-50">
            <tr v-for="booking in bookings" :key="booking.id" class="hover:bg-brand-blue/[0.02] transition-colors group">
              <td class="px-8 py-8"><input type="checkbox" class="rounded-lg border-gray-200 text-brand-blue focus:ring-brand-blue h-5 w-5 transition-premium"></td>
              <td class="px-8 py-8">
                <div class="flex flex-col">
                  <span class="text-sm font-black text-brand-blue">{{ booking.ref }}</span>
                  <span class="text-[11px] font-bold text-brand-green tracking-widest">{{ booking.pnr }}</span>
                </div>
              </td>
              <td class="px-8 py-8">
                <div class="flex items-center space-x-4">
                  <div class="h-10 w-10 rounded-xl bg-brand-blue/5 flex items-center justify-center text-brand-blue group-hover:bg-brand-blue group-hover:text-white transition-premium transition-colors">
                    <component :is="serviceIcon(booking.type)" class="h-5 w-5" />
                  </div>
                  <div>
                    <div class="text-sm font-bold text-brand-blue">{{ booking.service }}</div>
                    <div class="text-[11px] font-medium text-brand-gray/60 mt-0.5 tracking-tight font-sans">{{ booking.date }}</div>
                  </div>
                </div>
              </td>
              <td class="px-8 py-8">
                <div class="flex flex-col">
                  <div class="text-[11px] font-black text-brand-blue underline decoration-brand-blue/10 underline-offset-4 uppercase tracking-wider">{{ booking.agent }}</div>
                  <div class="text-[10px] text-brand-gray/50 mt-1 font-bold">{{ booking.customer }}</div>
                </div>
              </td>
              <td class="px-8 py-8">
                <span class="px-4 py-1.5 rounded-full text-[10px] font-black uppercase tracking-widest inline-block text-center min-w-[100px]" :class="statusClass(booking.status)">
                  {{ booking.status }}
                </span>
              </td>
              <td class="px-8 py-8">
                <span class="font-black text-brand-blue text-base tracking-tighter">${{ booking.amount }}</span>
              </td>
              <td class="px-8 py-8 text-right">
                <div class="flex items-center justify-end space-x-2">
                  <button class="bg-gray-50 text-brand-gray/40 hover:bg-brand-blue hover:text-white h-10 w-10 flex items-center justify-center rounded-xl transition-premium shadow-sm group/btn">
                    <EyeIcon class="h-4 w-4" />
                  </button>
                  <button class="bg-gray-50 text-brand-gray/40 hover:bg-brand-blue hover:text-white h-10 w-10 flex items-center justify-center rounded-xl transition-premium shadow-sm group/btn">
                    <ArrowTopRightOnSquareIcon class="h-4 w-4" />
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>

       <!-- Pagination -->
       <div class="px-8 py-8 flex items-center justify-between bg-gray-50/30">
         <p class="text-[10px] font-black text-brand-gray/40 uppercase tracking-[0.2em] font-sans">Showing 5 of 4,205 total records</p>
         <div class="flex items-center space-x-2">
            <button class="h-10 w-10 flex items-center justify-center rounded-xl bg-white border border-gray-100 text-brand-gray/40 hover:border-brand-blue/30 hover:text-brand-blue transition-premium">
              <ChevronLeftIcon class="h-4 w-4" />
            </button>
            <button v-for="p in 3" :key="p" class="h-10 w-10 flex items-center justify-center rounded-xl text-[11px] font-black transition-all" :class="p === 1 ? 'bg-brand-blue text-white shadow-xl shadow-brand-blue/20' : 'bg-white text-brand-gray/40 hover:bg-gray-50 hover:text-brand-blue border border-gray-50'">{{ p }}</button>
            <span class="text-brand-gray/30 px-2 font-black">...</span>
            <button class="h-10 w-10 flex items-center justify-center rounded-xl bg-white border border-gray-100 text-brand-gray/40 hover:border-brand-blue/30 hover:text-brand-blue transition-premium">
              <ChevronRightIcon class="h-4 w-4" />
            </button>
         </div>
       </div>
    </UiBaseCard>

    <!-- New Booking Side Drawer -->
    <UiSideDrawer 
      :show="showNewBooking" 
      title="Create Direct Booking" 
      @close="showNewBooking = false"
    >
      <div class="space-y-10">
        <p class="text-sm text-brand-gray/70 leading-relaxed font-medium">Create a new booking manually for an agent or direct customer.</p>
        
        <div class="space-y-6">
          <UiBaseInput label="Customer Full Name" placeholder="e.g. John Doe" :icon="UserIcon" />
          <UiBaseInput label="Email Address" type="email" placeholder="john@example.com" :icon="EnvelopeIcon" />
          
          <div class="grid grid-cols-2 gap-6">
            <div class="space-y-1">
              <label class="text-[10px] font-black text-brand-gray uppercase tracking-widest ml-1">Service category</label>
              <select class="w-full bg-white border border-gray-100 rounded-[1.5rem] py-4 px-6 text-sm font-bold text-brand-blue outline-none transition-premium focus:border-brand-blue focus:ring-4 focus:ring-brand-blue/5 shadow-[0_4px_20px_-4px_rgba(0,0,0,0.05)]">
                <option>Flight</option>
                <option>Hotel / Stay</option>
                <option>Car Rental</option>
                <option>Travel Package</option>
              </select>
            </div>
            <UiDatePicker label="Preferred Date" v-model="newBookingDate" />
          </div>

          <UiBaseInput label="Booking Amount ($)" placeholder="0.00" :icon="BanknotesIcon" />
        </div>
      </div>

      <template #footer>
        <div class="flex gap-4 w-full">
          <UiBaseButton variant="ghost" class="flex-1" @click="showNewBooking = false">Cancel Entry</UiBaseButton>
          <UiBaseButton variant="primary" class="flex-1">Confirm Booking</UiBaseButton>
        </div>
      </template>
    </UiSideDrawer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { 
  MagnifyingGlassIcon, 
  BuildingOfficeIcon, 
  MapPinIcon, 
  PaperAirplaneIcon, 
  TruckIcon,
  ArrowTopRightOnSquareIcon,
  PlusIcon,
  ArrowDownTrayIcon,
  CalendarDaysIcon,
  ArrowUpIcon,
  EyeIcon,
  ChevronLeftIcon,
  ChevronRightIcon,
  UserIcon,
  EnvelopeIcon,
  BanknotesIcon
} from '@heroicons/vue/24/outline'

definePageMeta({
  layout: 'admin'
})

const showNewBooking = ref(false)
const searchQuery = ref('')
const selectedDate = ref('')
const newBookingDate = ref('')

const filters = [
  { label: 'Type', options: ['All Services', 'Stays', 'Flights', 'Cars', 'Packages'] },
  { label: 'Status', options: ['All Status', 'Confirmed', 'Pending', 'Cancelled', 'Failed'] },
]

const serviceIcon = (type: string) => {
  switch (type) {
    case 'Stay': return BuildingOfficeIcon
    case 'Flight': return PaperAirplaneIcon
    case 'Car': return TruckIcon
    default: return MapPinIcon
  }
}

const statusClass = (status: string) => {
  switch (status) {
    case 'Confirmed': return 'bg-brand-green/10 text-brand-green'
    case 'Pending': return 'bg-yellow-100 text-yellow-600'
    case 'Cancelled': return 'bg-brand-gray/10 text-brand-gray'
    case 'Failed': return 'bg-red-100 text-red-600'
    default: return 'bg-blue-50 text-blue-400'
  }
}

const bookings = [
  { id: 1, ref: 'FB-920381', pnr: 'AX92LL', type: 'Stay', service: 'Marriott Bonvoy N.Y', agent: 'Elite Travel Group', customer: 'John Snow', email: 'j.snow@winter.com', status: 'Confirmed', amount: '2,405', date: 'Feb 14, 2025' },
  { id: 2, ref: 'FB-920382', pnr: 'B829SK', type: 'Flight', service: 'EK204: JFK -> DXB', agent: 'Global Voyages', customer: 'Sansa Stark', email: 'sansa@winter.com', status: 'Confirmed', amount: '4,820', date: 'Feb 14, 2025' },
  { id: 3, ref: 'FB-920383', pnr: 'QL290X', type: 'Stay', service: 'Burj Al Arab Suite', agent: 'SkyHigh Agency', customer: 'Arya Stark', email: 'arya@winter.com', status: 'Pending', amount: '12,500', date: 'Feb 14, 2025' },
  { id: 4, ref: 'FB-920384', pnr: 'PP291M', type: 'Car', service: 'Lamborghini Rental', agent: 'Luxe Escapes', customer: 'Tyrion Lannister', email: 'tyrion@casterly.com', status: 'Failed', amount: '1,200', date: 'Feb 13, 2025' },
  { id: 5, ref: 'FB-920385', pnr: 'ZZX922', type: 'Stay', service: 'The Ritz Paris', agent: 'Elite Travel Group', customer: 'Jamie Lannister', email: 'jamie@casterly.com', status: 'Cancelled', amount: '3,100', date: 'Feb 13, 2025' },
]
</script>

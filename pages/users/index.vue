<template>
  <div class="space-y-10 pb-12 text-brand-gray">
    <div class="flex flex-col md:flex-row md:items-end justify-between gap-6">
       <div class="space-y-1">
         <h1 class="text-4xl font-serif font-black text-brand-blue tracking-tight">Staffing & Permissions</h1>
         <p class="text-brand-gray/60 font-medium tracking-tight">Manage your core team, administrators, and support staff</p>
       </div>
       <UiBaseButton variant="primary" size="lg">
         <UserPlusIcon class="h-5 w-5 mr-3" />
         Invite Team Member
       </UiBaseButton>
    </div>

    <!-- Quick Stats -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      <UiBaseCard v-for="s in userStats" :key="s.label" padding class="group hover:border-brand-blue/20 transition-premium">
         <p class="text-[10px] font-black text-brand-gray/40 uppercase tracking-[0.25em] mb-1">{{ s.label }}</p>
         <div class="flex items-center justify-between">
           <h3 class="text-3xl font-serif font-black text-brand-blue">{{ s.value }}</h3>
           <div class="h-10 w-10 bg-brand-blue/5 rounded-xl flex items-center justify-center text-brand-blue group-hover:bg-brand-blue group-hover:text-white transition-premium">
             <component :is="s.icon" class="h-5 w-5" />
           </div>
         </div>
      </UiBaseCard>
    </div>

    <!-- User Table -->
    <UiBaseCard>
      <div class="overflow-x-auto overflow-y-hidden">
        <table class="w-full text-left border-collapse">
          <thead>
            <tr class="bg-gray-50/50">
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Staff Profile</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Assigned Role</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Activity Trace</th>
              <th class="px-8 py-6 text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Access Level</th>
              <th class="px-8 py-6 text-right text-[11px] font-black text-brand-gray/50 uppercase tracking-[0.2em]">Actions</th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-50 font-sans">
            <tr v-for="user in users" :key="user.email" class="hover:bg-brand-blue/[0.01] transition-premium group">
              <td class="px-8 py-8">
                <div class="flex items-center space-x-4">
                  <div class="h-12 w-12 rounded-2xl bg-brand-blue/5 border border-brand-blue/5 flex items-center justify-center font-black text-brand-blue transition-premium group-hover:rotate-6 shadow-sm overflow-hidden relative">
                    <div class="absolute inset-0 bg-brand-blue/5 group-hover:bg-brand-blue transition-premium" />
                    <span class="relative group-hover:text-white transition-premium">{{ user.name[0] }}</span>
                  </div>
                  <div>
                     <div class="text-sm font-black text-brand-blue">{{ user.name }}</div>
                     <div class="text-[10px] font-bold text-brand-gray/40 uppercase tracking-widest mt-0.5">{{ user.email }}</div>
                  </div>
                </div>
              </td>
              <td class="px-8 py-8">
                <span class="text-[10px] font-black text-brand-blue uppercase tracking-widest bg-brand-blue/5 px-4 py-2 rounded-xl border border-brand-blue/10">
                  {{ user.role }}
                </span>
              </td>
              <td class="px-8 py-8">
                <div class="flex flex-col">
                  <span class="text-xs font-bold text-brand-blue">{{ user.lastActive }}</span>
                  <span class="text-[10px] text-brand-gray/40 font-medium mt-1">Global IP verified</span>
                </div>
              </td>
              <td class="px-8 py-8">
                 <div class="flex items-center px-4 py-2 rounded-2xl bg-gray-50/50 border border-gray-100 w-fit">
                   <div class="h-2 w-2 rounded-full mr-3 shadow-sm" :class="user.status === 'Active' ? 'bg-brand-green animate-pulse' : 'bg-brand-gray/40'"></div>
                   <span class="text-[10px] font-black uppercase tracking-widest" :class="user.status === 'Active' ? 'text-brand-green' : 'text-brand-gray/60'">
                     {{ user.status }}
                   </span>
                 </div>
              </td>
              <td class="px-8 py-8 text-right">
                <div class="flex items-center justify-end space-x-2">
                  <button class="h-10 w-10 flex items-center justify-center rounded-xl bg-gray-50 text-brand-gray/40 hover:bg-brand-blue hover:text-white transition-premium shadow-sm">
                    <PencilSquareIcon class="h-4 w-4" />
                  </button>
                  <button class="h-10 w-10 flex items-center justify-center rounded-xl bg-gray-50 text-red-300 hover:bg-red-500 hover:text-white transition-premium shadow-sm">
                    <TrashIcon class="h-4 w-4" />
                  </button>
                </div>
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
  UserPlusIcon, 
  UsersIcon, 
  ShieldCheckIcon, 
  HeadsetIcon,
  PencilSquareIcon,
  TrashIcon
} from '@heroicons/vue/24/outline'

definePageMeta({
  layout: 'admin'
})

const userStats = [
  { label: 'Total Infrastructure Staff', value: '42', icon: UsersIcon },
  { label: 'Verified Global Admins', value: '5', icon: ShieldCheckIcon },
  { label: 'Active Support Tier 1/2', value: '28', icon: HeadsetIcon },
]

const users = [
  { name: 'John Admin', email: 'john@flybeth.com', role: 'Super Admin', lastActive: '2 mins ago', status: 'Active' },
  { name: 'Sarah Support', email: 'sarah.s@flybeth.com', role: 'Support Tier 2', lastActive: '1 hour ago', status: 'Active' },
  { name: 'Mike Finance', email: 'mike.f@flybeth.com', role: 'Finance Manager', lastActive: '1 day ago', status: 'Inactive' },
  { name: 'Emily Content', email: 'emily.c@flybeth.com', role: 'Content Moderator', lastActive: '5 mins ago', status: 'Active' },
]
</script>

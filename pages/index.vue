<template>
  <div class="min-h-screen bg-white flex overflow-hidden font-sans text-brand-gray relative">
    <!-- Decorative Background Elements -->
    <div class="absolute -top-24 -left-24 w-96 h-96 bg-brand-blue/5 rounded-full blur-3xl animate-pulse" />
    <div class="absolute top-1/2 -right-24 w-64 h-64 bg-brand-green/10 rounded-full blur-3xl" />
    <div class="absolute -bottom-24 left-1/2 w-80 h-80 bg-brand-blue/5 rounded-full blur-3xl" />

    <!-- Left: Branding & Visuals (Desktop) -->
    <div class="hidden lg:flex lg:w-1/2 bg-brand-blue p-20 flex-col justify-between relative overflow-hidden">
       <!-- Background Pattern -->
       <div class="absolute inset-0 opacity-10 pointer-events-none">
         <div class="absolute top-0 left-0 w-full h-full bg-[radial-gradient(circle_at_30%_20%,rgba(255,255,255,0.2)_0%,transparent_50%)]" />
       </div>

       <div class="relative z-10">
         <div class="flex items-center space-x-3 group cursor-pointer">
            <div class="h-12 w-12 bg-white rounded-2xl flex items-center justify-center shadow-2xl transform group-hover:rotate-6 transition-premium">
              <PaperAirplaneIcon class="h-7 w-7 text-brand-blue" />
            </div>
            <div class="flex flex-col">
              <span class="text-3xl font-serif font-black tracking-tighter text-white">FLYBETH</span>
              <span class="text-[10px] font-black uppercase tracking-[0.4em] text-brand-green/80 -mt-1">Portal HQ</span>
            </div>
         </div>
       </div>

       <div class="relative z-10 max-w-lg">
         <h2 class="text-6xl font-serif font-black text-white leading-tight mb-8">
           Redefining <br/> 
           <span class="text-brand-green">Global Travel</span> <br/>
           Management.
         </h2>
         <p class="text-lg text-white/50 font-medium leading-relaxed">
           The Flybeth Super Console provides unified governance over travel agents, financial settlements, and inventory worldwide.
         </p>
       </div>

       <div class="relative z-10 flex items-center space-x-8 text-white/30 text-[10px] font-black uppercase tracking-[0.3em]">
          <span>Security Protocol 8.2</span>
          <span class="h-1 w-1 rounded-full bg-white/20"></span>
          <span>Encrypted Session</span>
          <span class="h-1 w-1 rounded-full bg-white/20"></span>
          <span>Flybeth HQ</span>
       </div>
    </div>

    <!-- Right: Authentication Form -->
    <div class="w-full lg:w-1/2 flex items-center justify-center p-8 md:p-12 relative z-10">
      <div class="w-full max-w-md space-y-12">
        <div class="lg:hidden flex items-center space-x-3 mb-12">
            <div class="h-10 w-10 bg-brand-blue rounded-xl flex items-center justify-center shadow-lg">
              <PaperAirplaneIcon class="h-6 w-6 text-white" />
            </div>
            <span class="text-2xl font-serif font-black tracking-tighter text-brand-blue">FLYBETH</span>
        </div>

        <div>
          <h1 class="text-4xl font-serif font-black text-brand-blue leading-tight mb-3">Authentication</h1>
          <p class="text-brand-gray/60 font-medium">Enter your secure credentials to initialize your session.</p>
        </div>

        <form @submit.prevent="handleLogin" class="space-y-8">
           <div class="space-y-6">
              <UiBaseInput 
                v-model="form.email"
                label="Staff Email Identifier" 
                placeholder="staff@flybeth.com" 
                :icon="EnvelopeIcon"
                required
              />
              
              <div class="space-y-2">
                 <div class="flex justify-between items-center px-1">
                    <label class="text-[10px] font-black text-brand-gray uppercase tracking-widest opacity-60">Security Secret</label>
                    <a href="#" class="text-[10px] font-black text-brand-blue hover:text-brand-green uppercase tracking-widest transition-premium">Trouble Signing In?</a>
                 </div>
                 <div class="relative group">
                    <LockClosedIcon class="absolute left-4 top-1/2 -translate-y-1/2 h-5 w-5 text-brand-gray group-focus-within:text-brand-blue transition-premium" />
                    <input 
                      v-model="form.password"
                      type="password" 
                      required
                      placeholder="••••••••"
                      class="w-full bg-white border border-gray-100 rounded-[1.5rem] py-4 pl-12 pr-4 text-sm font-bold text-brand-blue focus:ring-4 focus:ring-brand-blue/5 focus:border-brand-blue transition-premium outline-none shadow-sm hover:border-brand-blue/30"
                    >
                 </div>
              </div>
           </div>

           <div class="flex items-center">
              <input 
                id="trust-device" 
                v-model="form.remember"
                type="checkbox" 
                class="h-5 w-5 text-brand-blue border-gray-100 rounded-lg focus:ring-brand-blue/20 transition-premium cursor-pointer"
              >
              <label for="trust-device" class="ml-3 text-xs font-bold text-brand-gray/50 cursor-pointer hover:text-brand-blue transition-premium">
                Trust this device for 30 days
              </label>
           </div>

           <UiBaseButton 
             type="submit" 
             variant="primary" 
             size="lg" 
             block 
             :loading="loading"
             class="!py-5 !rounded-[1.5rem] !text-base shadow-xl shadow-brand-blue/10"
           >
             Initialize Secure Session
           </UiBaseButton>
        </form>

        <div class="pt-10 border-t border-gray-50 flex flex-col sm:flex-row items-center justify-between gap-4">
           <p class="text-[10px] font-black text-brand-gray/30 uppercase tracking-[0.2em]">Flybeth Unified Portal v2.0</p>
           <div class="flex items-center space-x-4">
              <div class="h-2 w-2 bg-brand-green rounded-full shadow-[0_0_8px_rgba(50,180,4,0.5)]"></div>
              <span class="text-[10px] font-black text-brand-green uppercase tracking-widest">Global Ops Stable</span>
           </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { 
  EnvelopeIcon, 
  LockClosedIcon,
  PaperAirplaneIcon
} from '@heroicons/vue/24/outline'

definePageMeta({
  layout: false
})

const form = ref({
  email: '',
  password: '',
  remember: false
})

const loading = ref(false)

const handleLogin = async () => {
  if (!form.value.email || !form.value.password) return
  
  loading.value = true
  // Simulate network delay for premium feel
  await new Promise(resolve => setTimeout(resolve, 1800))
  loading.value = false
  navigateTo('/dashboard')
}
</script>

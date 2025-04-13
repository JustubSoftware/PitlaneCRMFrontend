<!-- src/routes/+page.svelte -->
<script>
    import { onMount } from 'svelte';
  
    // Kachel-Animationen
    const animateTile = (event) => {
      const tile = event.currentTarget;
      tile.style.transform = 'perspective(1000px) rotateX(5deg) rotateY(-5deg) translateZ(20px)';
      tile.querySelector('.status-led').style.boxShadow = '0 0 15px currentColor';
    };
  
    const resetTile = (event) => {
      const tile = event.currentTarget;
      tile.style.transform = '';
      tile.querySelector('.status-led').style.boxShadow = '';
    };
  </script>
  
  <div class="min-h-screen bg-gray-900 overflow-hidden relative">
    <!-- Rennstrecken-Hintergrund -->
    <div class="absolute inset-0 opacity-10">
      <svg class="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
        <pattern id="grid" width="5" height="5" patternUnits="userSpaceOnUse">
          <path d="M5 0L0 0 0 5" fill="none" stroke="rgba(234,179,8,0.1)" stroke-width="0.3" />
        </pattern>
        <rect width="100%" height="100%" fill="url(#grid)" />
        <path d="M0 50 L100 50" stroke="rgba(234,179,8,0.2)" stroke-width="1" stroke-dasharray="5" />
      </svg>
    </div>
  
    <!-- Speed Lines Animation -->
    <div class="speed-lines absolute inset-0 pointer-events-none">
      {#each Array(15) as _, i}
        <div
          class="absolute w-0.5 h-20 bg-gradient-to-b from-transparent via-yellow-500 to-transparent"
          style:left={`${Math.random() * 100}%`}
          style:animation={`moveLine ${2 + Math.random() * 2}s linear infinite`}
        ></div>
      {/each}
    </div>
  
    <!-- Hauptinhalt -->
    <main class="relative z-10 container mx-auto px-4 py-16 min-h-screen flex flex-col items-center justify-center">
      <!-- Header -->
      <div class="text-center mb-16 animate-fadeIn">
        <div class="logo-container mb-8 mx-auto w-32 h-32 relative">
          <div class="absolute inset-0 border-4 border-yellow-500 rounded-full transform rotate-45 animate-spin-slow"></div>
          <div class="absolute inset-4 border-4 border-red-600 rounded-full"></div>
          <span class="absolute inset-0 flex items-center justify-center text-4xl font-bold text-yellow-500">PL</span>
        </div>
        <h1 class="text-5xl md:text-6xl font-black bg-clip-text text-transparent bg-gradient-to-r from-yellow-400 to-red-600 mb-4">
          PITLANE <span class="text-white">CRM</span>
        </h1>
        <p class="text-xl text-gray-300 font-medium">
          <span class="inline-block mr-2">🏎️⚡</span>Rennsport-Performance für deine Werkstatt
        </p>
      </div>
  
      <!-- Interaktive Kacheln -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 w-full max-w-6xl">
        <!-- Werkstatt Dashboard -->
        <a
          href="/dashboard"
          on:mouseenter={animateTile}
          on:mouseleave={resetTile}
          class="group relative bg-gray-800/80 rounded-xl p-8 border-2 border-gray-700 hover:border-yellow-500 transition-all duration-300 hover:shadow-[0_0_40px_rgba(234,179,8,0.3)]"
        >
          <div class="status-led absolute top-4 right-4 w-3 h-3 bg-red-500 rounded-full transition-all duration-300"></div>
          <div class="flex items-start mb-6">
            <svg class="w-10 h-10 mr-4 text-yellow-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
            </svg>
            <h3 class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-red-500">
              RACING DASHBOARD
            </h3>
          </div>
          <p class="text-gray-300 mb-6">Echtzeit-Überblick aller Werkstattaktivitäten</p>
          <div class="flex justify-between items-center text-sm">
            <span class="text-yellow-400 font-mono">>{'>>'} START</span>
            <span class="text-gray-500">v12.4 Turbo</span>
          </div>
        </a>
  
        <!-- Fahrzeugverwaltung -->
        <a
          href="/vehicles"
          on:mouseenter={animateTile}
          on:mouseleave={resetTile}
          class="group relative bg-gray-800/80 rounded-xl p-8 border-2 border-gray-700 hover:border-blue-400 transition-all duration-300 hover:shadow-[0_0_40px_rgba(59,130,246,0.3)]"
        >
          <div class="status-led absolute top-4 right-4 w-3 h-3 bg-blue-500 rounded-full transition-all duration-300"></div>
          <div class="flex items-start mb-6">
            <svg class="w-10 h-10 mr-4 text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-.553-.894L15 4m0 13V4m0 0L9 7"
              />
            </svg>
            <h3 class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-cyan-500">
              FAHRZEUG DATENBANK
            </h3>
          </div>
          <p class="text-gray-300 mb-6">Vollständige Fahrzeughistorie</p>
          <div class="flex justify-between items-center text-sm">
            <span class="text-cyan-400 font-mono">>{'>>'} START</span>
            <span class="text-gray-500">OBD-II Ready</span>
          </div>
        </a>
  
        <!-- Kundenverwaltung -->
        <a
          href="/customers"
          on:mouseenter={animateTile}
          on:mouseleave={resetTile}
          class="group relative bg-gray-800/80 rounded-xl p-8 border-2 border-gray-700 hover:border-green-400 transition-all duration-300 hover:shadow-[0_0_40px_rgba(34,197,94,0.3)]"
        >
          <div class="status-led absolute top-4 right-4 w-3 h-3 bg-green-500 rounded-full transition-all duration-300"></div>
          <div class="flex items-start mb-6">
            <svg class="w-10 h-10 mr-4 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"
              />
            </svg>
            <h3 class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-green-400 to-teal-500">
              KUNDEN MANAGER
            </h3>
          </div>
          <p class="text-gray-300 mb-6">Premium-Kundenbetreuung</p>
          <div class="flex justify-between items-center text-sm">
            <span class="text-teal-400 font-mono">>{'>>'} START</span>
            <span class="text-gray-500">Team Radio ON</span>
          </div>
        </a>
      </div>
  
      <!-- Footer -->
      <footer class="mt-20 pt-8 border-t border-gray-800 text-center w-full max-w-4xl">
        <div class="font-mono text-sm text-gray-600 flex justify-center space-x-6">
          <span>⚡ PERFORMANCE MODE</span>
          <span>🏁 TRACK READY</span>
          <span>🔧 TOOLS ONLINE</span>
        </div>
        <div class="mt-4 text-xs text-gray-700">
          © {new Date().getFullYear()} PITLANE CRM - Engineered for Racing Performance
        </div>
      </footer>
    </main>
  </div>
  
  <style global>
    @keyframes moveLine {
      0% {
        transform: translateY(-100vh) rotate(15deg);
      }
      100% {
        transform: translateY(100vh) rotate(15deg);
      }
    }
  
    @keyframes spin-slow {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(360deg);
      }
    }
  
    .animate-spin-slow {
      animation: spin-slow 12s linear infinite;
    }
  
    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
  </style>
  
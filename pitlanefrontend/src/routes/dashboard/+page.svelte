<script>
    let workshopActivity = [
      { time: '08:00', description: 'Motorwartung an Ferrari SF90' },
      { time: '09:30', description: 'Ölwechsel an Mercedes-AMG GT Black Series' },
      { time: '11:00', description: 'Bremsenprüfung an Red Bull RB16B' }
    ];
  
    let currentOrders = [
      { id: 101, customer: 'Max Verstappen', vehicle: 'Red Bull RB16B', status: 'In Bearbeitung' },
      { id: 102, customer: 'Lewis Hamilton', vehicle: 'Mercedes-AMG GT', status: 'Wartet auf Teile' }
    ];
  
    let performanceMetrics = {
      revenue: '120,000 €',
      customerSatisfaction: '98%',
      averageRepairTime: '3.5 Stunden'
    };
  </script>
  
  <div class="min-h-screen bg-gray-900 overflow-hidden relative">
    <!-- Overlay für bessere Lesbarkeit -->
    <div class="absolute inset-0 bg-gray-900 opacity-80"></div>
  
    <!-- Hauptinhalt -->
    <main class="relative z-10 container mx-auto px-4 py-16 min-h-screen flex flex-col">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-red-500 mb-4">
          Werkstatt Dashboard
        </h1>
        <p class="text-xl text-gray-300 font-medium">
          Echtzeit-Überblick aller Werkstattaktivitäten
        </p>
      </div>
  
      <!-- Werkstattaktivitäten -->
      <section class="mb-8">
        <h2 class="text-2xl font-bold text-white mb-4">Werkstattaktivitäten</h2>
        <ul class="bg-gray-800 bg-opacity-70 rounded-lg shadow-md p-6">
          {#each workshopActivity as activity}
            <li class="text-gray-300 py-2 border-b border-gray-700 last:border-b-0">
              <span class="font-mono text-sm text-yellow-400 mr-4">{activity.time}</span>
              {activity.description}
            </li>
          {/each}
        </ul>
      </section>
  
      <!-- Aktuelle Aufträge -->
      <section class="mb-8">
        <h2 class="text-2xl font-bold text-white mb-4">Aktuelle Aufträge</h2>
        <table class="min-w-full bg-gray-800 bg-opacity-70 rounded-lg shadow-md">
          <thead>
            <tr class="text-white">
              <th class="py-3 px-4 border-b border-gray-700 text-left">Auftrags-ID</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Kunde</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Fahrzeug</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Status</th>
            </tr>
          </thead>
          <tbody>
            {#each currentOrders as order}
              <tr class="text-gray-300 hover:bg-gray-700 transition-colors duration-200">
                <td class="py-3 px-4 border-b border-gray-700">{order.id}</td>
                <td class="py-3 px-4 border-b border-gray-700">{order.customer}</td>
                <td class="py-3 px-4 border-b border-gray-700">{order.vehicle}</td>
                <td class="py-3 px-4 border-b border-gray-700">{order.status}</td>
              </tr>
            {/each}
          </tbody>
        </table>
      </section>
  
      <!-- Leistungsmetriken -->
      <section class="mb-8">
        <h2 class="text-2xl font-bold text-white mb-4">Leistungsmetriken</h2>
        <div class="bg-gray-800 bg-opacity-70 rounded-lg shadow-md p-6">
          <div class="flex justify-between items-center mb-4">
            <span class="text-gray-300">Umsatz:</span>
            <span class="text-yellow-400 font-bold">{performanceMetrics.revenue}</span>
          </div>
          <div class="flex justify-between items-center mb-4">
            <span class="text-gray-300">Kundenzufriedenheit:</span>
            <span class="text-green-400 font-bold">{performanceMetrics.customerSatisfaction}</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-gray-300">Durchschnittliche Reparaturzeit:</span>
            <span class="text-blue-400 font-bold">{performanceMetrics.averageRepairTime}</span>
          </div>
        </div>
      </section>
  
      <!-- Zurück-Link -->
      <div class="mt-8">
        <a href="/" class="text-blue-400 hover:text-blue-300 transition-colors duration-200">
          Zurück zum Hauptmenü
        </a>
      </div>
    </main>
  </div>
  
  <style>
    /* Hier können zusätzliche Stile eingefügt werden */
  </style>
  
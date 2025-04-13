<script>
    let vehicles = [
      { id: 1, make: 'Ferrari', model: 'SF90 Stradale', year: 2020, owner: 'Charles Leclerc' },
      { id: 2, make: 'Mercedes-AMG', model: 'GT Black Series', year: 2021, owner: 'Lewis Hamilton' },
      { id: 3, make: 'Red Bull Racing', model: 'RB16B', year: 2021, owner: 'Max Verstappen' }
    ];
  
    let newVehicle = {
      make: '',
      model: '',
      year: '',
      owner: ''
    };
  
    const addVehicle = () => {
      if (newVehicle.make && newVehicle.model && newVehicle.year && newVehicle.owner) {
        const newId = vehicles.length > 0 ? Math.max(...vehicles.map(v => v.id)) + 1 : 1;
        vehicles = [...vehicles, { id: newId, ...newVehicle }];
        newVehicle = { make: '', model: '', year: '', owner: '' };
      } else {
        alert('Bitte füllen Sie alle Felder aus.');
      }
    };
  
    const deleteVehicle = (id) => {
      vehicles = vehicles.filter(vehicle => vehicle.id !== id);
    };
  </script>
  
  <div class="min-h-screen bg-gray-900 overflow-hidden relative">
    <!-- Rennstrecken-Hintergrund -->
    <div class="absolute inset-0 opacity-10">
      <svg class="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
        <pattern id="grid" width="5" height="5" patternUnits="userSpaceOnUse">
          <path d="M5 0L0 0 0 5" fill="none" stroke="rgba(59,130,246,0.1)" stroke-width="0.3" />
        </pattern>
        <rect width="100%" height="100%" fill="url(#grid)" />
        <path d="M0 50 L100 50" stroke="rgba(59,130,246,0.2)" stroke-width="1" stroke-dasharray="5" />
      </svg>
    </div>
  
    <!-- Hauptinhalt -->
    <main class="relative z-10 container mx-auto px-4 py-16 min-h-screen flex flex-col items-center">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-cyan-500 mb-4">
          Fahrzeug Datenbank
        </h1>
        <p class="text-xl text-gray-300 font-medium">
          Verwalte deine Fahrzeuge wie ein Boxen-Crew-Chef
        </p>
      </div>
  
      <!-- Formular zum Erstellen von Fahrzeugen -->
      <div class="w-full max-w-4xl mb-8">
        <h2 class="text-2xl font-bold text-white mb-4">Fahrzeug erstellen</h2>
        <div class="bg-gray-800 border border-gray-700 rounded-lg shadow-md p-6">
          <div class="mb-4">
            <label for="make" class="block text-white text-sm font-bold mb-2">Hersteller:</label>
            <input type="text" id="make"
              class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
              bind:value={newVehicle.make} />
          </div>
          <div class="mb-4">
            <label for="model" class="block text-white text-sm font-bold mb-2">Modell:</label>
            <input type="text" id="model"
              class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
              bind:value={newVehicle.model} />
          </div>
          <div class="mb-4">
            <label for="year" class="block text-white text-sm font-bold mb-2">Baujahr:</label>
            <input type="number" id="year"
              class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
              bind:value={newVehicle.year} />
          </div>
          <div class="mb-6">
            <label for="owner" class="block text-white text-sm font-bold mb-2">Besitzer:</label>
            <input type="text" id="owner"
              class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
              bind:value={newVehicle.owner} />
          </div>
          <button on:click={addVehicle}
            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300">
            Fahrzeug hinzufügen
          </button>
        </div>
      </div>
  
      <!-- Fahrzeugtabelle -->
      <div class="w-full max-w-4xl overflow-x-auto">
        <table class="min-w-full bg-gray-800 border border-gray-700 rounded-lg shadow-md">
          <thead>
            <tr class="text-white">
              <th class="py-3 px-4 border-b border-gray-700 text-left">ID</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Hersteller</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Modell</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Baujahr</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Besitzer</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Aktionen</th>
            </tr>
          </thead>
          <tbody>
            {#each vehicles as vehicle (vehicle.id)}
              <tr class="text-white hover:bg-gray-700 transition-colors duration-200 underline-offset-4 hover:underline">
                <td class="py-4 px-4">{vehicle.id}</td>
                <td class="py-4 px-4">{vehicle.make}</td>
                <td class="py-4 px-4">{vehicle.model}</td>
                <td class="py-4 px-4">{vehicle.year}</td>
                <td class="py-4 px-4">{vehicle.owner}</td>
                <td class="py-4 px-4">
                  <button on:click={() => deleteVehicle(vehicle.id)}
                    class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300">
                    Löschen
                  </button>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
  
      <!-- Zurück-Link -->
      <div class="mt-8">
        <a href="/"
          class="text-blue-400 hover:text-blue-500 transition-colors duration-200 underline-offset-4 hover:underline">
          Zurück zur Startseite
        </a>
      </div>
    </main>
  </div>
  
  <style>
    table {
      border-collapse: collapse;
      width: 100%;
      margin-bottom: 20px;
    }
  
    th,
    td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }
  
    th {
      background-color: #f2f2f2;
      color: #333;
    }
  
    tr:hover {
      background-color: darkslategray;
    }
  
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  
    .add-button {
      background-color: #4caf50;
      color: white;
    }
  
    .delete-button {
      background-color: #f44336;
      color: white;
    }

    input[type="text"],
    input[type="number"]{
      color:white;
      border: 2px solid #ccc; /* Standardrahmenfarbe */
      border-radius: 5px;
      padding: 8px;
      margin-bottom: 10px;
      width: 100%;
      box-sizing: border-box;
    }
  
    input[type="text"]:focus,
    input[type="number"]:focus{
      border-color: #66afe9; /* Rahmenfarbe im Fokus */
      outline: 0;
      box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102,175,233,.6);
    }
  </style>
  
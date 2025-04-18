<svelte:head>
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons" />
</svelte:head>

<script>
  import { onMount } from 'svelte';

  let vehicles = [];
  let customers = [];
  let loading = true;
  let error = null;
  let showModal = false;

  let newVehicle = {
    id: null,
    brand: '',
    model: '',
    year: '',
    vin: '',
    customer: ''
  };

  let confirmDelete = false;
  let vehicleToDelete = null;

  const API_BASE_URL = 'http://localhost:8000/api';

  // Daten laden
  const fetchVehicles = async () => {
    loading = true;
    error = null;
    try {
      const [vRes, cRes] = await Promise.all([
        fetch(`${API_BASE_URL}/vehicles`),
        fetch(`${API_BASE_URL}/customers`)
      ]);
      if (!vRes.ok || !cRes.ok) throw new Error('API-Fehler');
      vehicles = await vRes.json();
      customers = await cRes.json();
    } catch (e) {
      error = e.message;
    } finally {
      loading = false;
    }
  };

  // Fahrzeug hinzufügen
  const addVehicle = async () => {
    try {
      const payload = {
        brand: newVehicle.brand,
        model: newVehicle.model,
        year: parseInt(newVehicle.year, 10),
        vin: newVehicle.vin,
        customer: parseInt(newVehicle.customer, 10)  // Wichtig: Dies sollte mit dem Backend übereinstimmen
      };

      console.log('Adding Vehicle Payload:', JSON.stringify(payload, null, 2));
      const response = await fetch(`${API_BASE_URL}/vehicles`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });

      if (!response.ok) {
        const errText = await response.text();
        console.error('Backend-Fehler:', errText);
        throw new Error(`API-Fehler: ${response.status}`);
      }
      showModal = false;
      newVehicle = { id: null, brand: '', model: '', year: '', vin: '', customer: '' };
      await fetchVehicles();
    } catch (e) {
      error = e.message;
    }
  };

  // Fahrzeug bearbeiten
  const editVehicle = async () => {
    try {
      const payload = {
        brand: newVehicle.brand,
        model: newVehicle.model,
        year: parseInt(newVehicle.year, 10),  // Wichtig: Konvertiere zu einer Zahl
        vin: newVehicle.vin,
        customer: parseInt(newVehicle.customer, 10)  // Wichtig: Dies sollte mit dem Backend übereinstimmen
      };
      const response = await fetch(`${API_BASE_URL}/vehicles/${newVehicle.id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
      });
      if (!response.ok) throw new Error(`API-Fehler: ${response.status}`);
      showModal = false;
      newVehicle = { id: null, brand: '', model: '', year: '', vin: '', customer: '' };
      await fetchVehicles();
    } catch (e) {
      error = e.message;
    }
  };

  const openDeleteConfirmation = (vehicle) => {
    confirmDelete = true;
    vehicleToDelete = vehicle;
  };

  const cancelDelete = () => {
    confirmDelete = false;
    vehicleToDelete = null;
  };

  const confirmAndDelete = async () => {
    if (vehicleToDelete) {
      await deleteVehicle(vehicleToDelete.id);
      confirmDelete = false;
      vehicleToDelete = null;
    }
  };

  const deleteVehicle = async (id) => {
    try {
      const response = await fetch(`${API_BASE_URL}/vehicles/${id}`, { method: 'DELETE' });
      if (!response.ok) throw new Error(`API-Fehler: ${response.status}`);
      await fetchVehicles();
    } catch (e) {
      error = e.message;
    }
  };

  // Modal öffnen
  const openEditModal = (vehicle) => {
    if (vehicle) {
      newVehicle = {
        id: vehicle.id,
        brand: vehicle.brand,
        model: vehicle.model,
        year: String(vehicle.year),  // Wichtig: Behandle year als String
        vin: vehicle.vin,
        customer: vehicle.customer?.id || ''  // Wichtig: Verwende 'customer'
      };
    } else {
      newVehicle = { id: null, brand: '', model: '', year: '', vin: '', customer: customers[0]?.id || '' };
    }
    showModal = true;
  };

  const closeModal = () => {
    showModal = false;
    newVehicle = { id: null, brand: '', model: '', year: '', vin: '', customer: '' };
  };

  onMount(fetchVehicles);
</script>

<div class="min-h-screen bg-gray-900 overflow-hidden relative">
  <main class="relative z-10 container mx-auto px-4 py-16 min-h-screen flex flex-col items-center">
    <div class="text-center mb-8">
      <h1 class="text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-cyan-500 mb-4">
        Fahrzeug Manager
      </h1>
      <p class="text-xl text-gray-300 font-medium">
        Verwalte deine Fahrzeuge und ordne sie Kunden zu
      </p>
    </div>

    <div class="w-full max-w-4xl mb-8">
      <h2 class="text-2xl font-bold text-white mb-4">
        Fahrzeuge erstellen
        <button
          on:click={() => {
            newVehicle = { id: null, brand: '', model: '', year: '', vin: '', customer_id: customers[0]?.id || '' };
            showModal = true;
          }}
          class="ml-4 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-blue-300"
        >
          Neues Fahrzeug hinzufügen
        </button>
      </h2>
    </div>

    {#if loading}
      <p class="text-white">Lade Fahrzeuge...</p>
    {:else if error}
      <p class="text-red-500">Fehler: {error}</p>
    {:else}
      <div class="w-full max-w-4xl overflow-x-auto">
        <table class="min-w-full bg-gray-800 border border-gray-700 rounded-lg shadow-md">
          <thead>
            <tr class="text-white">
              <th class="py-3 px-4 border-b border-gray-700 text-left">ID</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Hersteller</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Modell</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Baujahr</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">VIN</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Kunde</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Aktionen</th>
            </tr>
          </thead>
          <tbody>
            {#each vehicles as vehicle (vehicle.id)}
              <tr class="text-white hover:bg-gray-700 transition-colors duration-200">
                <td class="py-4 px-4">{vehicle.id}</td>
                <td class="py-4 px-4">{vehicle.brand}</td>
                <td class="py-4 px-4">{vehicle.model}</td>
                <td class="py-4 px-4">{vehicle.year}</td>
                <td class="py-4 px-4">{vehicle.vin}</td>
                <td class="py-4 px-4">
                  {vehicle.customer
                    ? `${vehicle.customer.first_name} ${vehicle.customer.last_name}`
                    : ''}
                </td>
                <td class="py-4 px-4 whitespace-nowrap">
                  <span
                    on:click={() => openEditModal(vehicle)}
                    class="material-icons text-blue-500 hover:text-blue-600 cursor-pointer align-middle mr-2"
                    title="Bearbeiten"
                    style="font-size: 20px; vertical-align: middle;"
                  >
                    edit
                  </span>
                  <span
                    on:click={() => openDeleteConfirmation(vehicle)}
                    class="material-icons text-red-500 hover:text-red-600 cursor-pointer align-middle"
                    title="Löschen"
                    style="font-size: 20px; vertical-align: middle;"
                  >
                    delete
                  </span>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    {/if}

    {#if showModal}
      <div class="absolute inset-0 overflow-y-auto h-full w-full" id="vehicle-modal">
        <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-gray-800">
          <div class="mt-3 text-center">
            <h3 class="text-lg leading-6 font-medium text-white">{newVehicle.id ? 'Fahrzeug bearbeiten' : 'Neues Fahrzeug hinzufügen'}</h3>
            <div class="mt-2">
              <label for="brand" class="block text-white text-sm font-bold mb-2">Hersteller:</label>
              <input
                type="text"
                id="brand"
                class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
                bind:value={newVehicle.brand}
              />

              <label for="model" class="block text-white text-sm font-bold mb-2">Modell:</label>
              <input
                type="text"
                id="model"
                class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
                bind:value={newVehicle.model}
              />

              <label for="year" class="block text-white text-sm font-bold mb-2">Baujahr:</label>
              <input
                type="number"
                id="year"
                class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
                bind:value={newVehicle.year}
              />

              <label for="vin" class="block text-white text-sm font-bold mb-2">VIN:</label>
              <input
                type="text"
                id="vin"
                class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
                bind:value={newVehicle.vin}
              />

              <label for="customer_id" class="block text-white text-sm font-bold mb-2">Kunde:</label>
              <select
                id="customer_id"
                class="shadow appearance-none border border-blue-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-blue-500 bg-white hover:border-blue-400"
                bind:value={newVehicle.customer}
              >
                {#each customers as customer}
                  <option value={customer.id}>
                    {customer.first_name} {customer.last_name} (ID: {customer.id})
                  </option>
                {/each}
              </select>
            </div>
            <div class="items-center px-4 py-3">
              {#if newVehicle.id}
                <button
                  class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-blue-300 mr-2"
                  on:click={editVehicle}
                >
                  Speichern
                </button>
              {:else}
                <button
                  class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-green-300 mr-2"
                  on:click={addVehicle}
                >
                  Hinzufügen
                </button>
              {/if}
              <button
                class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300"
                on:click={closeModal}
              >
                Abbrechen
              </button>
            </div>
          </div>
        </div>
      </div>
    {/if}

    {#if confirmDelete}
      <div class="fixed inset-0 flex items-center justify-center bg-gray-600 bg-opacity-50">
        <div class="bg-gray-800 p-8 rounded-md shadow-lg text-center">
          <span class="material-icons text-yellow-500 mx-auto mb-4" style="font-size:48px">
            warning
          </span>
          <p class="text-white mb-4">
            Möchten Sie dieses Fahrzeug wirklich löschen?
          </p>
          <div class="flex justify-center gap-4">
            <button
              on:click={confirmAndDelete}
              class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300"
            >
              Löschen
            </button>
            <button
              on:click={cancelDelete}
              class="bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-gray-300"
            >
              Abbrechen
            </button>
          </div>
        </div>
      </div>
    {/if}

    <div class="mt-8">
      <a
        href="/"
        class="text-blue-400 hover:text-blue-500 transition-colors duration-200 underline-offset-4 hover:underline"
      >
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
  th, td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #374151;
  }
  th {
    background-color: #1f2937;
    color: #fff;
  }
  tr:hover {
    background-color: #374151;
  }
  input, select {
    outline: none;
  }
  .material-icons {
    vertical-align: middle;
  }
</style>

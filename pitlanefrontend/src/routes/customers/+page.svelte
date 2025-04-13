<script>
    let customers = [
      { id: 1, name: 'Max Verstappen', email: 'max@redbullracing.com', phone: '+49 123 456789', address: 'Red Bull Ring, Spielberg' },
      { id: 2, name: 'Lewis Hamilton', email: 'lewis@mercedes.com', phone: '+44 987 654321', address: 'Silverstone, UK' },
      { id: 3, name: 'Charles Leclerc', email: 'charles@ferrari.com', phone: '+33 112 233445', address: 'Maranello, Italy' }
    ];
  
    let newCustomer = {
      name: '',
      email: '',
      phone: '',
      address: ''
    };
  
    const addCustomer = () => {
      if (newCustomer.name && newCustomer.email && newCustomer.phone && newCustomer.address) {
        const newId = customers.length > 0 ? Math.max(...customers.map(c => c.id)) + 1 : 1;
        customers = [...customers, { id: newId, ...newCustomer }];
        newCustomer = { name: '', email: '', phone: '', address: '' };
      } else {
        alert('Bitte füllen Sie alle Felder aus.');
      }
    };
  
    const deleteCustomer = (id) => {
      customers = customers.filter(customer => customer.id !== id);
    };
  </script>
  
  <div class="min-h-screen bg-gray-900 overflow-hidden relative">
    <div class="absolute inset-0 opacity-10">
      <svg class="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
        <pattern id="grid" width="5" height="5" patternUnits="userSpaceOnUse">
          <path d="M5 0L0 0 0 5" fill="none" stroke="rgba(234,179,8,0.1)" stroke-width="0.3" />
        </pattern>
        <rect width="100%" height="100%" fill="url(#grid)" />
        <path d="M0 50 L100 50" stroke="rgba(234,179,8,0.2)" stroke-width="1" stroke-dasharray="5" />
      </svg>
    </div>
  
    <main class="relative z-10 container mx-auto px-4 py-16 min-h-screen flex flex-col items-center">
      <div class="text-center mb-8">
        <h1 class="text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-green-400 to-teal-500 mb-4">
          Kunden Manager
        </h1>
        <p class="text-xl text-gray-300 font-medium">
          Verwalte deine Kunden wie ein Boxen-Crew-Chef
        </p>
      </div>
  
      <div class="w-full max-w-4xl mb-8">
        <h2 class="text-2xl font-bold text-white mb-4">Kunden erstellen</h2>
        <div class="bg-gray-800 border border-gray-700 rounded-lg shadow-md p-6">
          <div class="mb-4">
            <label for="name" class="block text-white text-sm font-bold mb-2">Name:</label>
            <input
              type="text"
              id="name"
              class="shadow appearance-none border border-yellow-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-yellow-500 bg-white hover:border-yellow-400"
              bind:value={newCustomer.name}
            />
          </div>
          <div class="mb-4">
            <label for="email" class="block text-white text-sm font-bold mb-2">E-Mail:</label>
            <input
              type="email"
              id="email"
              class="shadow appearance-none border border-yellow-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-yellow-500 bg-white hover:border-yellow-400"
              bind:value={newCustomer.email}
            />
          </div>
          <div class="mb-4">
            <label for="phone" class="block text-white text-sm font-bold mb-2">Telefon:</label>
            <input
              type="tel"
              id="phone"
              class="shadow appearance-none border border-yellow-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-yellow-500 bg-white hover:border-yellow-400"
              bind:value={newCustomer.phone}
            />
          </div>
          <div class="mb-6">
            <label for="address" class="block text-white text-sm font-bold mb-2">Adresse:</label>
            <input
              type="text"
              id="address"
              class="shadow appearance-none border border-yellow-500 rounded w-full py-2 px-3 text-gray-800 leading-tight focus:outline-none focus:ring focus:ring-yellow-500 bg-white hover:border-yellow-400"
              bind:value={newCustomer.address}
            />
          </div>
          <button
            on:click={addCustomer}
            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300"
          >
            Kunden hinzufügen
          </button>
        </div>
      </div>
  
      <div class="w-full max-w-4xl overflow-x-auto">
        <table class="min-w-full bg-gray-800 border border-gray-700 rounded-lg shadow-md">
          <thead>
            <tr class="text-white">
              <th class="py-3 px-4 border-b border-gray-700 text-left">ID</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Name</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">E-Mail</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Telefon</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Adresse</th>
              <th class="py-3 px-4 border-b border-gray-700 text-left">Aktionen</th>
            </tr>
          </thead>
          <tbody>
            {#each customers as customer (customer.id)}
              <tr class="text-white hover:bg-gray-700 transition-colors duration-200 underline-offset-4 hover:underline">
                <td class="py-4 px-4">{customer.id}</td>
                <td class="py-4 px-4">{customer.name}</td>
                <td class="py-4 px-4">{customer.email}</td>
                <td class="py-4 px-4">{customer.phone}</td>
                <td class="py-4 px-4">{customer.address}</td>
                <td class="py-4 px-4">
                  <button
                    on:click={() => deleteCustomer(customer.id)}
                    class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring focus:ring-red-300"
                  >
                    Löschen
                  </button>
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
  
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
    input[type="email"],
    input[type="tel"] {
      color:white;
      border: 2px solid #ccc; /* Standardrahmenfarbe */
      border-radius: 5px;
      padding: 8px;
      margin-bottom: 10px;
      width: 100%;
      box-sizing: border-box;
    }
  
    input[type="text"]:focus,
    input[type="email"]:focus,
    input[type="tel"]:focus {
      border-color: #4caf50; /* Rahmenfarbe im Fokus */
      outline: 0;
      box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102,175,233,.6);
    }
  </style>
  
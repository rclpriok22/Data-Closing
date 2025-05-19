
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DATA CLOSING TIME</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .hidden { display: none; }
    .row-enter { animation: fadeIn 0.3s ease-in; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
    .radio-group label { cursor: pointer; touch-action: manipulation; }
    .radio-group input:checked + span { background-color: #3b82f6; color: white; }
    input, select, button { min-height: 44px; }
    @media (max-width: 640px) {
      th, td { font-size: 12px; padding: 6px; white-space: nowrap; }
      .radio-group { flex-direction: column; gap: 10px; }
      .container { padding: 12px; }
      .table-container { max-width: 100%; overflow-x: auto; }
      select, input[type="text"] { font-size: 14px; }
    }
    @media (max-width: 360px) {
      th, td { font-size: 11px; padding: 5px; }
    }
  </style>
</head>
<body class="bg-gray-100 font-sans">
  <div class="container mx-auto p-4 sm:p-6 max-w-4xl">
    <h1 class="text-2xl sm:text-3xl font-bold text-gray-800 mb-2">DATA CLOSING TIME</h1>
    <p class="text-sm text-gray-600 italic mb-6">setelah input data tunggu pengecekan dari pelayaran setelah pengecekan nanti akan di hubungin dan di email ke terminal</p>
    <form id="closing-form" onsubmit="handleSubmit(event)">
      <div class="bg-white p-4 sm:p-6 rounded-lg shadow-md mb-6">
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700">Booking Number</label>
            <input type="text" name="booking_number" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700">POD</label>
            <input type="text" name="pod" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700">Vessel Name</label>
            <input type="text" name="vessel_name" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700">OPR</label>
            <input type="text" name="opr" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-lg shadow-md table-container">
        <table class="w-full border-collapse" id="container_table">
          <thead>
            <tr class="bg-gray-200">
              <th class="p-3 text-left text-sm font-semibold text-gray-700">Container No</th>
              <th class="p-3 text-left text-sm font-semibold text-gray-700">Type</th>
              <th class="p-3 text-left text-sm font-semibold text-gray-700">Size</th>
              <th class="p-3 text-left text-sm font-semibold text-gray-700">Cetak Kartu</th>
              <th class="p-3"></th>
            </tr>
          </thead>
          <tbody>
            <tr id="row_1">
              <td class="p-3">
                <input type="text" name="container_number_1" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
                <button type="button" onclick="addRow()" class="mt-2 w-full bg-blue-500 text-white p-3 rounded-md hover:bg-blue-600 active:bg-blue-700 transition">Add</button>
              </td>
              <td class="p-3">
                <select name="type_container_1" id="type_container_1" onchange="updateForm(1)" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
                  <option value="DRY">DRY</option>
                  <option value="DG">DG</option>
                  <option value="Reefer">Reefer</option>
                  <option value="MTY">MTY</option>
                </select>
              </td>
              <td class="p-3">
                <select name="size_container_1" id="size_container_1" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
                  <option value="20'GP">20'GP</option>
                  <option value="40'HC">40'HC</option>
                  <option value="45'Ft">45'Ft</option>
                </select>
              </td>
              <td class="p-3">
                <div class="radio-group flex flex-col gap-2">
                  <label class="flex items-center"><input type="radio" name="cetak_kartu_1" value="sudah" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Sudah</span></label>
                  <label class="flex items-center"><input type="radio" name="cetak_kartu_1" value="belum" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Belum</span></label>
                </div>
              </td>
              <td class="p-3">
                <button type="button" onclick="removeRow(1)" class="text-red-500 hover:text-red-700 active:text-red-800 hidden">Remove</button>
              </td>
            </tr>
            <tr id="reefer_fields_1" class="hidden">
              <td colspan="5" class="p-3 bg-gray-50">
                <label class="block text-sm font-medium text-gray-700">Set Temp</label>
                <input type="text" name="set_temp_1" placeholder="e.g., -18°C" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
              </td>
            </tr>
            <tr id="dg_fields_1" class="hidden">
              <td colspan="5" class="p-3 bg-gray-50">
                <div class="flex flex-col sm:flex-row gap-4">
                  <div class="flex-1">
                    <label class="block text-sm font-medium text-gray-700">IMO</label>
                    <input type="text" name="imo_1" placeholder="e.g., 3" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
                  </div>
                  <div class="flex-1">
                    <label class="block text-sm font-medium text-gray-700">UNNO</label>
                    <input type="text" name="unno_1" placeholder="e.g., 1234" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
                  </div>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <button type="submit" class="mt-6 w-full bg-green-500 text-white p-3 rounded-md hover:bg-green-600 active:bg-blue-700 transition text-lg">Submit</button>
    </form>
  </div>
  <script>
    let rowCount = 1;

    function updateForm(rowIndex) {
      const type = document.getElementById(`type_container_${rowIndex}`).value;
      const sizeSelect = document.getElementById(`size_container_${rowIndex}`);
      const reeferFields = document.getElementById(`reefer_fields_${rowIndex}`);
      const dgFields = document.getElementById(`dg_fields_${rowIndex}`);

      sizeSelect.innerHTML = '';

      if (type === 'Reefer') {
        sizeSelect.innerHTML = `
          <option value="20'RF">20'RF</option>
          <option value="40'RH">40'RH</option>
        `;
        reeferFields.classList.remove('hidden');
        dgFields.classList.add('hidden');
      } else {
        sizeSelect.innerHTML = `
          <option value="20'GP">20'GP</option>
          <option value="40'HC">40'HC</option>
          <option value="45'Ft">45'Ft</option>
        `;
        if (type === 'DG') {
          dgFields.classList.remove('hidden');
          reeferFields.classList.add('hidden');
        } else {
          reeferFields.classList.add('hidden');
          dgFields.classList.add('hidden');
        }
      }
    }

    function addRow() {
      rowCount++;
      const table = document.getElementById('container_table').getElementsByTagName('tbody')[0];
      
      const newRow = document.createElement('tr');
      newRow.id = `row_${rowCount}`;
      newRow.classList.add('row-enter');
      newRow.innerHTML = `
        <td class="p-3">
          <input type="text" name="container_number_${rowCount}" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          <button type="button" onclick="addRow()" class="mt-2 w-full bg-blue-500 text-white p-3 rounded-md hover:bg-blue-600 active:bg-blue-700 transition">Add</button>
        </td>
        <td class="p-3">
          <select name="type_container_${rowCount}" id="type_container_${rowCount}" onchange="updateForm(${rowCount})" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
            <option value="DRY">DRY</option>
            <option value="DG">DG</option>
            <option value="Reefer">Reefer</option>
            <option value="MTY">MTY</option>
          </select>
        </td>
        <td class="p-3">
          <select name="size_container_${rowCount}" id="size_container_${rowCount}" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
            <option value="20'GP">20'GP</option>
            <option value="40'HC">40'HC</option>
            <option value="45'Ft">45'Ft</option>
          </select>
        </td>
        <td class="p-3">
          <div class="radio-group flex flex-col gap-2">
            <label class="flex items-center"><input type="radio" name="cetak_kartu_${rowCount}" value="sudah" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Sudah</span></label>
            <label class="flex items-center"><input type="radio" name="cetak_kartu_${rowCount}" value="belum" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Belum</span></label>
          </div>
        </td>
        <td class="p-3">
          <button type="button" onclick="removeRow(${rowCount})" class="text-red-500 hover:text-red-700 active:text-red-800">Remove</button>
        </td>
      `;
      table.appendChild(newRow);

      const reeferRow = document.createElement('tr');
      reeferRow.id = `reefer_fields_${rowCount}`;
      reeferRow.classList.add('hidden');
      reeferRow.innerHTML = `
        <td colspan="5" class="p-3 bg-gray-50">
          <label class="block text-sm font-medium text-gray-700">Set Temp</label>
          <input type="text" name="set_temp_${rowCount}" placeholder="e.g., -18°C" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
        </td>
      `;
      table.appendChild(reeferRow);

      const dgRow = document.createElement('tr');
      dgRow.id = `dg_fields_${rowCount}`;
      dgRow.classList.add('hidden');
      dgRow.innerHTML = `
        <td colspan="5" class="p-3 bg-gray-50">
          <div class="flex flex-col sm:flex-row gap-4">
            <div class="flex-1">
              <label class="block text-sm font-medium text-gray-700">IMO</label>
              <input type="text" name="imo_${rowCount}" placeholder="e.g., 3" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
            </div>
            <div class="flex-1">
              <label class="block text-sm font-medium text-gray-700">UNNO</label>
              <input type="text" name="unno_${rowCount}" placeholder="e.g., 1234" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
            </div>
          </div>
        </td>
      `;
      table.appendChild(dgRow);

      updateForm(rowCount);
    }

    function removeRow(rowIndex) {
      if (rowCount === 1) return;
      document.getElementById(`row_${rowIndex}`).remove();
      document.getElementById(`reefer_fields_${rowIndex}`).remove();
      document.getElementById(`dg_fields_${rowIndex}`).remove();
      if (rowIndex === rowCount) rowCount--;
    }

    async function handleSubmit(event) {
      event.preventDefault();
      const form = document.getElementById('closing-form');
      if (!form.checkValidity()) {
        form.reportValidity();
        return;
      }

      const formData = new FormData(form);
      const data = {
        booking_number: formData.get('booking_number'),
        pod: formData.get('pod'),
        vessel_name: formData.get('vessel_name'),
        opr: formData.get('opr'),
        containers: []
      };

      for (let i = 1; i <= rowCount; i++) {
        if (document.getElementById(`row_${i}`)) {
          const container = {
            container_number: formData.get(`container_number_${i}`),
            type_container: formData.get(`type_container_${i}`),
            size_container: formData.get(`size_container_${i}`),
            cetak_kartu: formData.get(`cetak_kartu_${i}`) || ''
          };
          if (container.type_container === 'Reefer') {
            container.set_temp = formData.get(`set_temp_${i}`) || '';
          }
          if (container.type_container === 'DG') {
            container.imo = formData.get(`imo_${i}`) || '';
            container.unno = formData.get(`unno_${i}`) || '';
          }
          data.containers.push(container);
        }
      }

      try {
        const response = await fetch('https://script.google.com/macros/s/AKfycbzY6UIYG9OrwF78KNT1iWkHmgepaVApP2lyoAbzk_qb6YGkLhR1NPpus3JJoyGXnnA4/exec', {
          method: 'POST',
          mode: 'no-cors',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(data)
        });
        alert('Data submitted successfully to Google Sheets!');
        form.reset();
        while (rowCount > 1) {
          removeRow(rowCount);
        }
        updateForm(1);
      } catch (error) {
        alert('Failed to submit data: ' + error.message + '. Please try again.');
      }
    }

    updateForm(1);
  </script>
</body>
</html>

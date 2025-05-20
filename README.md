
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DATA CLOSING TIME</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .hidden { display: none !important; } /* Added !important for robustness */
    .sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; border-width: 0; }
    .row-enter { animation: fadeIn 0.3s ease-in; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
    .radio-group label { cursor: pointer; touch-action: manipulation; }
    .radio-group input:checked + span { background-color: #3b82f6; color: white; }
    input, select, button { min-height: 44px; }
    @media (max-width: 640px) {
      .container-table-wrapper { display: block; overflow-x: auto; } /* Renamed for clarity */
      .container-row { display: flex; flex-direction: column; gap: 8px; padding: 8px 0; }
      .container-cell { width: 100%; padding: 4px; }
      th, td { font-size: 12px; padding: 4px; white-space: nowrap; }
      .add-button { width: 100%; margin-top: 8px; }
      select, input[type="text"] { font-size: 14px; width: 100%; }
      .radio-group { flex-direction: column; gap: 6px; }
    }
    @media (max-width: 360px) {
      th, td { font-size: 11px; padding: 3px; }
      .container-cell { padding: 2px; }
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
            <label for="booking_number_field" class="block text-sm font-medium text-gray-700">Booking Number</label>
            <input type="text" id="booking_number_field" name="booking_number" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label for="pod_field" class="block text-sm font-medium text-gray-700">POD</label>
            <input type="text" id="pod_field" name="pod" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label for="vessel_name_field" class="block text-sm font-medium text-gray-700">Vessel Name</label>
            <input type="text" id="vessel_name_field" name="vessel_name" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
          <div>
            <label for="opr_field" class="block text-sm font-medium text-gray-700">OPR</label>
            <input type="text" id="opr_field" name="opr" class="mt-1 block w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-lg shadow-md container-table-wrapper">
        <table class="w-full border-collapse" id="container_table">
          <thead>
            <tr class="bg-gray-200">
              <th scope="col" class="p-3 text-left text-sm font-semibold text-gray-700">Container No</th>
              <th scope="col" class="p-3 text-left text-sm font-semibold text-gray-700">Type</th>
              <th scope="col" class="p-3 text-left text-sm font-semibold text-gray-700">Size</th>
              <th scope="col" class="p-3 text-left text-sm font-semibold text-gray-700">Cetak Kartu</th>
              <th scope="col" class="p-3"><span class="sr-only">Actions</span></th>
            </tr>
          </thead>
          <tbody>
            <tr id="row_1" class="container-row">
              <td class="container-cell">
                <input type="text" name="container_number_1" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
              </td>
              <td class="container-cell">
                <select name="type_container_1" id="type_container_1" onchange="updateForm(1)" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
                  <option value="DRY">DRY</option>
                  <option value="DG">DG</option>
                  <option value="Reefer">Reefer</option>
                  <option value="MTY">MTY</option>
                </select>
              </td>
              <td class="container-cell">
                <select name="size_container_1" id="size_container_1" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
                  <option value="20'GP">20'GP</option>
                  <option value="40'HC">40'HC</option>
                  <option value="45'Ft">45'Ft</option>
                </select>
              </td>
              <td class="container-cell">
                <fieldset>
                    <legend class="sr-only">Cetak Kartu for container 1</legend>
                    <div class="radio-group flex flex-col gap-2">
                      <label class="flex items-center"><input type="radio" name="cetak_kartu_1" value="sudah" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Sudah</span></label>
                      <label class="flex items-center"><input type="radio" name="cetak_kartu_1" value="belum" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Belum</span></label>
                    </div>
                </fieldset>
              </td>
              <td class="container-cell">
                <button type="button" onclick="removeRow(1)" class="remove-row-button text-red-500 hover:text-red-700 active:text-red-800 hidden">Remove</button>
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
        <button type="button" id="add_container_button" onclick="addRow()" class="add-button bg-blue-500 text-white p-3 rounded-md hover:bg-blue-600 active:bg-blue-700 transition w-full mt-2">Add Container</button>
      </div>
      <button type="submit" class="mt-6 w-full bg-green-500 text-white p-3 rounded-md hover:bg-green-600 active:bg-green-700 transition text-lg">Submit</button>
    </form>
  </div>
  <script>
    let nextRowId = 1; // Used to generate unique IDs for new rows

    function updateForm(rowIndex) {
      const typeSelect = document.getElementById(`type_container_${rowIndex}`);
      if (!typeSelect) return; // Row might have been removed
      const type = typeSelect.value;
      
      const sizeSelect = document.getElementById(`size_container_${rowIndex}`);
      const reeferFields = document.getElementById(`reefer_fields_${rowIndex}`);
      const dgFields = document.getElementById(`dg_fields_${rowIndex}`);

      const setTempInput = document.querySelector(`input[name="set_temp_${rowIndex}"]`);
      const imoInput = document.querySelector(`input[name="imo_${rowIndex}"]`);
      const unnoInput = document.querySelector(`input[name="unno_${rowIndex}"]`);

      // Reset conditional required attributes
      if (setTempInput) setTempInput.required = false;
      if (imoInput) imoInput.required = false;
      if (unnoInput) unnoInput.required = false;

      // Clear current options
      if (sizeSelect) sizeSelect.innerHTML = '';

      if (type === 'Reefer') {
        if (sizeSelect) {
          sizeSelect.innerHTML = `
            <option value="20'RF">20'RF</option>
            <option value="40'RH">40'RH</option>
          `;
        }
        if (reeferFields) reeferFields.classList.remove('hidden');
        if (dgFields) dgFields.classList.add('hidden');
        if (setTempInput) setTempInput.required = true;

      } else {
        if (sizeSelect) {
          sizeSelect.innerHTML = `
            <option value="20'GP">20'GP</option>
            <option value="40'HC">40'HC</option>
            <option value="45'Ft">45'Ft</option>
          `;
        }
        if (reeferFields) reeferFields.classList.add('hidden');
        
        if (type === 'DG') {
          if (dgFields) dgFields.classList.remove('hidden');
          if (imoInput) imoInput.required = true;
          if (unnoInput) unnoInput.required = true;
        } else {
          if (dgFields) dgFields.classList.add('hidden');
        }
      }
    }

    function updateRemoveButtonVisibility() {
      const tableBody = document.getElementById('container_table').getElementsByTagName('tbody')[0];
      const allMainRows = tableBody.querySelectorAll('tr.container-row');
      allMainRows.forEach(row => {
        const removeBtn = row.querySelector('.remove-row-button');
        if (removeBtn) {
          if (allMainRows.length > 1) {
            removeBtn.classList.remove('hidden');
          } else {
            removeBtn.classList.add('hidden');
          }
        }
      });
    }

    function addRow() {
      nextRowId++;
      const table = document.getElementById('container_table').getElementsByTagName('tbody')[0];
      
      const newRow = document.createElement('tr');
      newRow.id = `row_${nextRowId}`;
      newRow.classList.add('row-enter', 'container-row');
      newRow.innerHTML = `
        <td class="container-cell">
          <input type="text" name="container_number_${nextRowId}" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off" required>
        </td>
        <td class="container-cell">
          <select name="type_container_${nextRowId}" id="type_container_${nextRowId}" onchange="updateForm(${nextRowId})" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
            <option value="DRY">DRY</option>
            <option value="DG">DG</option>
            <option value="Reefer">Reefer</option>
            <option value="MTY">MTY</option>
          </select>
        </td>
        <td class="container-cell">
          <select name="size_container_${nextRowId}" id="size_container_${nextRowId}" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500">
            <option value="20'GP">20'GP</option>
            <option value="40'HC">40'HC</option>
            <option value="45'Ft">45'Ft</option>
          </select>
        </td>
        <td class="container-cell">
          <fieldset>
            <legend class="sr-only">Cetak Kartu for container ${nextRowId}</legend>
            <div class="radio-group flex flex-col gap-2">
              <label class="flex items-center"><input type="radio" name="cetak_kartu_${nextRowId}" value="sudah" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Sudah</span></label>
              <label class="flex items-center"><input type="radio" name="cetak_kartu_${nextRowId}" value="belum" class="mr-2 h-5 w-5"><span class="p-2 rounded-md">Belum</span></label>
            </div>
          </fieldset>
        </td>
        <td class="container-cell">
          <button type="button" onclick="removeRow(${nextRowId})" class="remove-row-button text-red-500 hover:text-red-700 active:text-red-800">Remove</button>
        </td>
      `;
      table.appendChild(newRow);

      const reeferRow = document.createElement('tr');
      reeferRow.id = `reefer_fields_${nextRowId}`;
      reeferRow.classList.add('hidden');
      reeferRow.innerHTML = `
        <td colspan="5" class="p-3 bg-gray-50">
          <label class="block text-sm font-medium text-gray-700">Set Temp</label>
          <input type="text" name="set_temp_${nextRowId}" placeholder="e.g., -18°C" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
        </td>
      `;
      table.appendChild(reeferRow);

      const dgRow = document.createElement('tr');
      dgRow.id = `dg_fields_${nextRowId}`;
      dgRow.classList.add('hidden');
      dgRow.innerHTML = `
        <td colspan="5" class="p-3 bg-gray-50">
          <div class="flex flex-col sm:flex-row gap-4">
            <div class="flex-1">
              <label class="block text-sm font-medium text-gray-700">IMO</label>
              <input type="text" name="imo_${nextRowId}" placeholder="e.g., 3" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
            </div>
            <div class="flex-1">
              <label class="block text-sm font-medium text-gray-700">UNNO</label>
              <input type="text" name="unno_${nextRowId}" placeholder="e.g., 1234" inputmode="numeric" class="w-full p-3 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500" autocomplete="off">
            </div>
          </div>
        </td>
      `;
      table.appendChild(dgRow);

      updateForm(nextRowId);
      updateRemoveButtonVisibility();

      const firstInputNewRow = newRow.querySelector('input[type="text"], select');
      if (firstInputNewRow) {
          firstInputNewRow.focus();
      }
    }

    function removeRow(rowIndex) {
      const rowToRemove = document.getElementById(`row_${rowIndex}`);
      const reeferFieldsToRemove = document.getElementById(`reefer_fields_${rowIndex}`);
      const dgFieldsToRemove = document.getElementById(`dg_fields_${rowIndex}`);

      if (rowToRemove) rowToRemove.remove();
      if (reeferFieldsToRemove) reeferFieldsToRemove.remove();
      if (dgFieldsToRemove) dgFieldsToRemove.remove();
      
      updateRemoveButtonVisibility();
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

      const tableBody = document.getElementById('container_table').getElementsByTagName('tbody')[0];
      const containerRows = tableBody.querySelectorAll('tr.container-row');

      containerRows.forEach(row => {
        const rowIdSuffix = row.id.split('_')[1]; // e.g., "row_1" -> "1"
        const container = {
          container_number: formData.get(`container_number_${rowIdSuffix}`),
          type_container: formData.get(`type_container_${rowIdSuffix}`),
          size_container: formData.get(`size_container_${rowIdSuffix}`),
          cetak_kartu: formData.get(`cetak_kartu_${rowIdSuffix}`) || ''
        };
        if (container.type_container === 'Reefer') {
          container.set_temp = formData.get(`set_temp_${rowIdSuffix}`) || '';
        }
        if (container.type_container === 'DG') {
          container.imo = formData.get(`imo_${rowIdSuffix}`) || '';
          container.unno = formData.get(`unno_${rowIdSuffix}`) || '';
        }
        data.containers.push(container);
      });


      try {
        const response = await fetch('https://script.google.com/macros/s/AKfycbzY6UIYG9OrwF78KNT1iWkHmgepaVApP2lyoAbzk_qb6YGkLhR1NPpus3JJoyGXnnA4/exec', {
          method: 'POST',
          mode: 'no-cors', // Client-side cannot read the response from Google Apps Script with no-cors
          headers: { 
            // 'Content-Type': 'application/json' // Not needed for FormData directly to GAS, unless GAS is specifically expecting JSON
            // For 'no-cors' and sending to GAS, it's often easier to let the browser set Content-Type or use 'text/plain' for simple proxying.
            // However, since we are constructing a JSON object `data` and then stringifying it, application/json is correct.
            // If your GAS doPost(e) expects e.postData.contents as JSON string, this is correct.
             'Content-Type': 'application/json'
          },
          body: JSON.stringify(data) // Sending the manually constructed JSON
        });
        // With no-cors, we can't check response.ok or response.status
        alert('Permintaan pengiriman data telah dikirim. Harap tunggu konfirmasi melalui email.');
        form.reset();

        // Clean up rows, leaving only the first one
        const allMainRows = Array.from(tableBody.querySelectorAll('tr.container-row'));
        const allReeferRows = Array.from(tableBody.querySelectorAll('tr[id^="reefer_fields_"]'));
        const allDgRows = Array.from(tableBody.querySelectorAll('tr[id^="dg_fields_"]'));

        for (let i = allMainRows.length - 1; i > 0; i--) {
            allMainRows[i].remove();
        }
        allReeferRows.forEach(row => { if(row.id !== "reefer_fields_1") row.remove(); });
        allDgRows.forEach(row => { if(row.id !== "dg_fields_1") row.remove(); });
        
        // Reset the state for the first row
        nextRowId = 1; // Reset counter for new rows to start from _1 (first row is _1)
        updateForm(1); // Reset type/size/conditional fields for the first row
        // Ensure radio buttons for the first row are also reset (form.reset() should handle this)
        const firstRowRadios = document.querySelectorAll('input[name="cetak_kartu_1"]');
        firstRowRadios.forEach(radio => radio.checked = false);
        
        updateRemoveButtonVisibility(); // Hide remove button if only one row remains

      } catch (error) {
        console.error('Error submitting data:', error);
        alert('Gagal mengirim data: ' + error.message + '. Silakan coba lagi.');
      }
    }

    document.addEventListener('DOMContentLoaded', () => {
        updateForm(1); // Initialize form for the first row
        updateRemoveButtonVisibility(); // Initialize remove button visibility
    });
  </script>
</body>
</html>

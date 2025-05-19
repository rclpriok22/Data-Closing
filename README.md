<script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
        </script><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Data Closing Time</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      background-image: url('https://raw.githubusercontent.com/username/data-closing-time/main/background.jpg');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-color: #1e3a8a; /* Fallback to blue-900 */
    }
    .form-container {
      background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
    }
  </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4 sm:p-6">
  <div class="form-container rounded-lg shadow-2xl p-4 sm:p-6 max-w-lg w-full">
    <h1 class="text-xl sm:text-2xl font-bold text-blue-900 mb-6 text-center">Data Closing Time</h1>
    <div id="form-container">
      <div class="mb-4" id="container-0">
        <label for="container-number-0" class="block text-sm font-medium text-gray-800">Container Number</label>
        <div class="flex items-end space-x-2">
          <input type="text" id="container-number-0" class="mt-1 flex-1 rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
          <button type="button" onclick="addContainerField()" class="bg-blue-600 text-white py-1.5 px-3 rounded-md hover:bg-blue-700 transition text-sm">Add</button>
        </div>
      </div>
      <div id="extra-containers"></div>
      <div class="mb-4">
        <label for="booking-number" class="block text-sm font-medium text-gray-800">Booking Number</label>
        <input type="text" id="booking-number" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
      </div>
      <div class="mb-4">
        <label for="pod" class="block text-sm font-medium text-gray-800">POD</label>
        <input type="text" id="pod" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
      </div>
      <div class="mb-4">
        <label for="vessel-name" class="block text-sm font-medium text-gray-800">Vessel Name</label>
        <input type="text" id="vessel-name" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
      </div>
      <div class="mb-4">
        <label for="opr" class="block text-sm font-medium text-gray-800">OPR</label>
        <input type="text" id="opr" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
      </div>
      <div class="mb-4">
        <label class="block text-sm font-medium text-gray-800">Cetak Kartu</label>
        <div class="mt-2 flex space-x-4">
          <label class="inline-flex items-center">
            <input type="radio" name="cetak-kartu" value="Sudah" class="form-radio text-blue-600 h-5 w-5" required>
            <span class="ml-2 text-gray-800 text-sm sm:text-base">Sudah Cetak Kartu</span>
          </label>
          <label class="inline-flex items-center">
            <input type="radio" name="cetak-kartu" value="Belum" class="form-radio text-blue-600 h-5 w-5">
            <span class="ml-2 text-gray-800 text-sm sm:text-base">Belum Cetak Kartu</span>
          </label>
        </div>
      </div>
      <button type="button" onclick="submitForm()" class="w-full bg-blue-900 text-white py-2.5 px-4 rounded-md hover:bg-blue-800 transition text-sm sm:text-base font-medium">Submit</button>
    </div>
    <p id="status" class="mt-4 text-center text-sm text-gray-800"></p>
  </div>

  <script>
    let containerCount = 1;

    function addContainerField() {
      const extraContainers = document.getElementById('extra-containers');
      const newField = document.createElement('div');
      newField.className = 'mb-4 flex items-end space-x-2';
      newField.id = `container-${containerCount}`;
      newField.innerHTML = `
        <div class="flex-1">
          <label for="container-number-${containerCount}" class="block text-sm font-medium text-gray-800">Container Number</label>
          <input type="text" id="container-number-${containerCount}" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring focus:ring-blue-500 focus:ring-opacity-50 text-sm sm:text-base py-2.5 px-3" required>
        </div>
        <button type="button" onclick="removeContainerField(${containerCount})" class="bg-red-600 text-white py-1.5 px-3 rounded-md hover:bg-red-700 transition text-sm">Hapus</button>
      `;
      extraContainers.appendChild(newField);
      containerCount++;
    }

    function removeContainerField(index) {
      const container = document.getElementById(`container-${index}`);
      if (container) {
        container.remove();
        const extraContainers = document.getElementById('extra-containers');
        const containers = extraContainers.children;
        containerCount = 1; // Reset to 1 since container-0 is always present
        for (let i = 0; i < containers.length; i++) {
          containers[i].id = `container-${containerCount}`;
          const input = containers[i].querySelector('input');
          const button = containers[i].querySelector('button');
          input.id = `container-number-${containerCount}`;
          button.setAttribute('onclick', `removeContainerField(${containerCount})`);
          containerCount++;
        }
      }
    }

    async function submitForm() {
      const containerNumbers = [];
      for (let i = 0; i < containerCount; i++) {
        const containerInput = document.getElementById(`container-number-${i}`);
        if (containerInput && containerInput.value) {
          containerNumbers.push(containerInput.value);
        }
      }
      const bookingNumber = document.getElementById('booking-number').value;
      const pod = document.getElementById('pod').value;
      const vesselName = document.getElementById('vessel-name').value;
      const opr = document.getElementById('opr').value;
      const cetakKartu = document.querySelector('input[name="cetak-kartu"]:checked')?.value;
      const status = document.getElementById('status');

      if (containerNumbers.length === 0 || !bookingNumber || !pod || !vesselName || !opr || !cetakKartu) {
        status.textContent = 'Please fill all fields';
        status.className = 'mt-4 text-center text-sm text-red-600';
        return;
      }

      const data = {
        containerNumbers,
        bookingNumber,
        pod,
        vesselName,
        opr,
        cetakKartu
      };

      try {
        const response = await fetch('https://script.google.com/macros/s/AKfycbxVtAqCma9MVvx61FRV-7NFHvXP4qvDutDG4DztqEzPuUJR3n2NXnTE5itvV9FfTslT/exec', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(data)
        });
        const result = await response.json();
        status.textContent = result.message;
        status.className = 'mt-4 text-center text-sm text-green-600';
        if (result.status === 'success') {
          document.getElementById('form-container').reset();
          document.getElementById('extra-containers').innerHTML = '';
          containerCount = 1;
        }
      } catch (error) {
        status.textContent = 'Error submitting form';
        status.className = 'mt-4 text-center text-sm text-red-600';
      }
    }
  </script>
</body>
</html>

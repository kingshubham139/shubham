<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Storage and Display</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
        }
    </style>
</head>
<body class="bg-gray-100">
    <nav class="bg-blue-600 p-4 text-white">
        <div class="container mx-auto flex justify-between items-center">
            <a href="#" class="text-2xl font-bold">DataStore</a>
            <ul class="flex space-x-4">
                <li><a href="#" class="hover:underline">Home</a></li>
                <li><a href="#" class="hover:underline">Add Data</a></li>
                <li><a href="#" class="hover:underline">View Data</a></li>
            </ul>
        </div>
    </nav>

    <div class="container mx-auto mt-8">
        <!-- Input Section -->
        <div class="bg-white p-6 rounded shadow-md">
            <h2 class="text-2xl font-bold mb-4">Add Data</h2>
            <form id="dataForm">
                <div class="mb-4">
                    <label for="name" class="block text-gray-700">Name</label>
                    <input type="text" id="name" name="name" class="mt-2 p-2 border rounded w-full" required>
                </div>
                <div class="mb-4">
                    <label for="email" class="block text-gray-700">Email</label>
                    <input type="email" id="email" name="email" class="mt-2 p-2 border rounded w-full" required>
                </div>
                <div class="mb-4">
                    <label for="description" class="block text-gray-700">Description</label>
                    <textarea id="description" name="description" class="mt-2 p-2 border rounded w-full" rows="3" required></textarea>
                </div>
                <button type="submit" class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">Add Data</button>
            </form>
        </div>

        <!-- Table Section -->
        <div class="bg-white p-6 rounded shadow-md mt-8">
            <h2 class="text-2xl font-bold mb-4">Stored Data</h2>
            <table class="table-auto w-full border-collapse border border-gray-300">
                <thead>
                    <tr class="bg-gray-200">
                        <th class="border border-gray-300 px-4 py-2">Name</th>
                        <th class="border border-gray-300 px-4 py-2">Email</th>
                        <th class="border border-gray-300 px-4 py-2">Description</th>
                        <th class="border border-gray-300 px-4 py-2">Actions</th>
                    </tr>
                </thead>
                <tbody id="dataTable">
                    <!-- Data will be dynamically inserted here -->
                </tbody>
            </table>
            <p id="emptyMessage" class="text-gray-500 text-center mt-4 hidden">No data available.</p>
        </div>
    </div>

    <footer class="bg-blue-600 text-white p-4 mt-8">
        <div class="container mx-auto text-center">
            <p>&copy; 2025 DataStore. All rights reserved.</p>
        </div>
    </footer>

    <script>
        const dataKey = 'dataStorage';

        const dataForm = document.getElementById('dataForm');
        const dataTable = document.getElementById('dataTable');
        const emptyMessage = document.getElementById('emptyMessage');

        // Load data from localStorage
        function loadData() {
            const data = localStorage.getItem(dataKey);
            return data ? JSON.parse(data) : [];
        }

        // Save data to localStorage
        function saveData(data) {
            localStorage.setItem(dataKey, JSON.stringify(data));
        }

        // Update the table
        function updateTable() {
            const dataStorage = loadData();

            // Clear existing table rows
            dataTable.innerHTML = '';

            if (dataStorage.length === 0) {
                emptyMessage.classList.remove('hidden');
            } else {
                emptyMessage.classList.add('hidden');

                // Populate table rows
                dataStorage.forEach((data, index) => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td class="border border-gray-300 px-4 py-2">${data.name}</td>
                        <td class="border border-gray-300 px-4 py-2">${data.email}</td>
                        <td class="border border-gray-300 px-4 py-2">${data.description}</td>
                        <td class="border border-gray-300 px-4 py-2 text-center">
                            <button class="bg-red-500 text-white px-2 py-1 rounded hover:bg-red-600" onclick="deleteData(${index})">Delete</button>
                        </td>
                    `;
                    dataTable.appendChild(row);
                });
            }
        }

        // Add data and save to localStorage
        dataForm.addEventListener('submit', (event) => {
            event.preventDefault();

            // Retrieve form data
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const description = document.getElementById('description').value;

            // Load existing data, add new entry, and save
            const dataStorage = loadData();
            dataStorage.push({ name, email, description });
            saveData(dataStorage);

            // Clear the form
            dataForm.reset();

            // Update the table
            updateTable();
        });

        // Delete data by index
        function deleteData(index) {
            const dataStorage = loadData();
            dataStorage.splice(index, 1);
            saveData(dataStorage);
            updateTable();
        }

        // Initialize table
        updateTable();
    </script>
</body>
</html>

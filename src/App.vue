<script setup>
import { ref } from "vue"; // Import ref if not already imported

const assets = ref({
  Printer: "HR",
  Monitor: "IT",
  "Barcode Scanner": "ACCOUNT",
});

const downloadCSV = () => {
  const csvContent =
    "data:text/csv;charset=utf-8," +
    "Asset Name,Department\n" + // Header row
    Object.entries(assets.value) // Now access assets.value
      .map(([asset, department]) => `${asset},${department}`)
      .join("\n");

  const encodedUri = encodeURI(csvContent);
  const link = document.createElement("a");
  link.setAttribute("href", encodedUri);
  link.setAttribute("download", "assets.csv");
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>
<template>
  <div class="table-container">
    <table class="asset-table">
      <thead>
        <tr>
          <th>Asset Name</th>
          <th>Department</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(department, asset) in assets" :key="asset">
          <td>{{ asset }}</td>
          <td>{{ department }}</td>
        </tr>
      </tbody>
    </table>
    <button class="button-download" @click="downloadCSV">Download CSV</button>
  </div>
</template>

<style scoped>
.table-container {
  overflow-x: auto;
}

.asset-table {
  width: 100%;
  border-collapse: collapse;
}

.asset-table th,
.asset-table td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.asset-table th {
  background-color: #f2f2f2;
  font-weight: bold;
}

.asset-table tbody tr:nth-child(even) {
  background-color: #f9f9f9;
}

.asset-table tbody tr:hover {
  background-color: #f2f2f2;
}

.button-download {
  margin-top: 24px;
  width: 100%;
  padding: 12px;
  box-shadow: 3px 7px 9px 0px rgba(65, 64, 64, 0.11);
  border: 1px solid rgba(39, 39, 39, 0.164);
  border-radius: 20px;
}
</style>

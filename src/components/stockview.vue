<template>
  <div class="app-container">
    <v-card class="main-card" outlined>
      <v-card-title class="card-header">
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search Medicines"
          single-line
          hide-details
          outlined
          dense
          class="search-bar"
        ></v-text-field>
      </v-card-title>

      <v-data-table
        :headers="headers"
        :items="newitemadd"
        :search="search"
        class="data-table"
        dense
      >
        <template v-slot:top>
          <div class="table-header">Medicine Inventory</div>
        </template>
        <template v-slot:[`item.medicinename`]="{ item }">
          <span class="item-highlight">{{ item.medicinename }}</span>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: "",
      medicine: this.$store.state.medicinemaster,
      stock: this.$store.state.stock,
      headers: [
        { text: "Medicine Name", align: "start", sortable: false, value: "medicinename" },
        { text: "Brand Name", value: "brandname" },
        { text: "Quantity", value: "quantity" },
        { text: "Unit Price", value: "amount" },
      ],
    };
  },
  computed: {
    newitemadd() {
      const newarr = [];
      for (const stockItem of this.stock) {
        for (const medicineItem of this.medicine) {
          if (stockItem.medicinename === medicineItem.medicinename) {
            const mergedObject = {
              medicinename: stockItem.medicinename,
              quantity: stockItem.quantity,
              amount: stockItem.amount,
              brandname: medicineItem.brandname,
            };
            newarr.push(mergedObject);
          }
        }
      }
      return newarr;
    },
  },
};
</script>

<style scoped>
.app-container {
  background-color: #f1faff;
  padding: 20px;
}

.main-card {
  background-color: #e9f7ef;
  border: 1px solid #d1e7dd;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card-header {
  background-color: #002d62;
  color: white;
  font-weight: bold;
  padding: 16px;
  border-radius: 4px 4px 0 0;
}

.search-bar {
  background-color: white;
  border-radius: 4px;
  max-width: 400px;
}

.data-table {
  background-color: #ffffff;
}

.table-header {
  font-size: 18px;
  font-weight: bold;
  color: #002d62;
  text-align: center;
  padding: 10px 0;
}

.v-data-table__wrapper {
  border: 1px solid #d1e7dd;
}

.v-data-table th {
  background-color: #e9f7ef;
  color: #002d62;
  font-weight: bold;
}

.v-data-table td {
  background-color: #ffffff;
}

.v-data-table tbody tr:nth-child(even) {
  background-color: #f0f8ff;
}

.item-highlight {
  font-weight: bold;
  color: #002d62;
}
</style>

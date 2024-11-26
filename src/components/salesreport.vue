<template>
  <div>
    <!-- Card Container -->
    <v-card width="100vw" class="main-card">
      <v-row class="p-4">
        <!-- Start Date Picker -->
        <v-col cols="12" sm="6" md="4">
          <v-menu
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="date1"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date1"
                label="Start Date"
                prepend-icon="mdi-calendar"
                readonly
                outlined
                dense
                class="date-picker"
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date1" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(date1)">OK</v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>

        <!-- End Date Picker -->
        <v-col cols="12" sm="6" md="4">
          <v-dialog
            ref="dialog"
            v-model="modal"
            :return-value.sync="date2"
            persistent
            width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date2"
                label="End Date"
                prepend-icon="mdi-calendar"
                readonly
                outlined
                dense
                class="date-picker"
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date2" scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="modal = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.dialog.save(date2)">OK</v-btn>
            </v-date-picker>
          </v-dialog>
        </v-col>

        <!-- Search Button -->
        <v-col cols="12" sm="6" md="4" class="d-flex align-center justify-center">
          <v-btn color="primary" class="search-btn" @click="search">Search</v-btn>
        </v-col>
      </v-row>
    </v-card>

    <!-- Data Table -->
    <v-data-table
      :headers="headers"
      :items="filterarr"
      :items-per-page="5"
      class="elevation-1 data-table mt-5"
    ></v-data-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      date2: new Date().toISOString().substr(0, 10),
      date1: new Date().toISOString().substr(0, 10),
      menu: false,
      modal: false,
      arr: [],
      billdetails: this.$store.state.billdetails,
      billmaster: this.$store.state.billmaster,
      headers: [
        { text: "Bill No", align: "start", sortable: false, value: "billno" },
        { text: "Bill Date", value: "billdate" },
        { text: "Medicine Name", value: "medicinename" },
        { text: "Quantity", value: "quantity" },
        { text: "Unit Price", value: "unitprice" },
        { text: "Amount", value: "amount" },
      ],
      newarr: [],
      filterarr: [],
    };
  },
  mounted() {
    for (let i in this.billdetails) {
      for (let j in this.billmaster) {
        if (this.billdetails[i].billno === this.billmaster[j].billno) {
          let mergedObject = {
            billno: this.billdetails[i].billno,
            medicinename: this.billdetails[i].medicinename,
            quantity: this.billdetails[i].qty,
            unitprice: this.billdetails[i].unitprice,
            amount: Number(this.billdetails[i].qty) * Number(this.billdetails[i].unitprice),
            billdate: this.billmaster[j].billdate,
          };
          this.newarr.push(mergedObject);
        }
      }
    }
  },
  methods: {
    search() {
      this.filterarr = [];
      let start = new Date(this.date1).toISOString();
      let stop = new Date(this.date2).toISOString();
      for (let i in this.newarr) {
        if (start <= this.newarr[i].billdate && stop >= this.newarr[i].billdate) {
          this.filterarr.push(this.newarr[i]);
        }
      }
    },
  },
};
</script>

<style scoped>
.main-card {
  background-color: #002b5b; 
  color: #ffffff;
  border-radius: 8px;
}

.date-picker {
  background-color: #ffffff;
  color: #002b5b;
}

.search-btn {
  background-color: #81d8d0; 
  color: #002b5b;
}

.data-table {
  background-color: #ffffff;
  border-radius: 8px;
  color: #002b5b;
}

.v-data-table-header {
  background-color: #81d8d0;
  color: #002b5b;
}
</style>

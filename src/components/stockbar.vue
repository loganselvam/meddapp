<template>
  <div class="app-container">
    <v-card class="main-card" outlined>
      <v-dialog v-model="dialog" max-width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="navy"
            dark
            class="add-btn"
            v-bind="attrs"
            v-on="on"
          >
            Add Medicine
          </v-btn>
        </template>
        <v-card class="dialog-card">
          <v-card-title class="dialog-title">
            Add New Medicine
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col cols="12" md="12">
                <v-text-field
                  label="Brand Name"
                  v-model="bname"
                  outlined
                  dense
                  color="navy"
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="12">
                <v-text-field
                  label="Medicine Name"
                  v-model="medicine"
                  outlined
                  dense
                  color="navy"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-card-text>
          <v-card-actions>
            <v-btn color="mint" dark @click="add">
              Add
            </v-btn>
            <v-btn text @click="dialog = false">Cancel</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-container>
        <v-row>
          <v-col cols="12" md="3">
            <v-select
              v-model="medicine"
              :items="medicinemaster"
              item-text="medicinename"
              label="Select Medicine"
              dense
              outlined
              color="navy"
            ></v-select>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="brandname1"
              label="Brand Name"
              dense
              outlined
              color="navy"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="qty"
              label="Quantity"
              dense
              outlined
              color="navy"
              type="number"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="uprice"
              label="Unit Price"
              dense
              outlined
              color="navy"
              type="number"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-btn
              class="update-btn"
              color="navy"
              dark
              block
              @click="update"
            >
              Update Stock
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialog: false,
      bname: "",
      medicine: "",
      qty: 0,
      brandname1: "",
      uprice: 0,
      medicinemaster: this.$store.state.medicinemaster,
      stock: this.$store.state.stock,
    };
  },
  methods: {
    add() {
      const newMedicine = {
        medicinename: this.medicine,
        brandname: this.bname,
      };
      this.medicinemaster.push(newMedicine);
      this.dialog = false;
      this.bname = "";
      this.medicine = "";
    },
    update() {
      const newStock = {
        medicinename: this.medicine,
        quantity: Number(this.qty),
        amount: Number(this.uprice),
      };

      const existingStock = this.stock.find(
        (item) => item.medicinename === this.medicine
      );
      if (existingStock) {
        existingStock.quantity += Number(this.qty);
        existingStock.amount = Number(this.uprice);
      } else {
        this.stock.push(newStock);
      }

      alert("Stock updated!");
    },
  },
  watch: {
    medicine(val) {
      const selectedMedicine = this.medicinemaster.find(
        (item) => item.medicinename === val
      );
      this.brandname1 = selectedMedicine ? selectedMedicine.brandname : "";
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
.add-btn {
  margin: 20px;
  background-color: #002d62;
}
.dialog-card {
  background-color: #ffffff;
}
.dialog-title {
  font-weight: bold;
  color: #002d62;
  text-align: center;
}
.update-btn {
  margin-top: 20px;
  background-color: #002d62;
}
.v-text-field__control {
  background-color: #ffffff !important;
}
.v-btn {
  border-radius: 4px;
}
</style>

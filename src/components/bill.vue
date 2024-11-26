<template>
    <div>
      <!-- Main Container with Background -->
      <v-sheet width="100vw" class="pa-5" style="background-color: #F1F9F7;">
        <!-- Expansion Panel for Item Entry -->
        <v-expansion-panels>
          <v-expansion-panel>
            <v-expansion-panel-header>
              <v-icon left>mdi-cart</v-icon> Items
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-card elevation="2" class="pa-4" style="background-color: #FFFFFF;">
                <v-row>
                  <v-col cols="12" md="4">
                    <v-autocomplete
                      v-model="medname"
                      :items="items"
                      item-text="medicinename"
                      dense
                      filled
                      label="Medicine"
                      class="mb-4"
                      style="background-color: #E3F8F0;"
                    ></v-autocomplete>
                  </v-col>
                  <v-col cols="12" md="4">
                    <v-text-field label="Quantity" v-model="qty" type="number" class="mb-4" style="background-color: #E3F8F0;"></v-text-field>
                  </v-col>
                  <v-col cols="12" md="4" class="d-flex align-end">
                    <v-btn @click="add" color="primary" large class="white--text" style="background-color: #003366;">
                      Add
                    </v-btn>
                  </v-col>
                </v-row>
              </v-card>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-sheet>
  
      <!-- Bill Summary Section -->
      <v-sheet width="100vw" class="d-flex" style="background-color: #4CAF50;">
        <v-container fluid>
          <v-row>
            <v-col cols="12" md="5">
              <!-- Preview Dialog -->
              <v-dialog transition="dialog-bottom-transition" max-width="600">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn @click="print" class="black white--text mr-5" v-bind="attrs" v-on="on" style="background-color: #003366;">
                    Preview
                  </v-btn>
                </template>
                <template v-slot:default="dialog">
                  <v-card>
                    <v-sheet>
                      <v-container style="background-color: #66BB6A;">
                        <v-row>
                          <v-col cols="12">
                            <h1 class="text-center white--text">Medical Shop Name</h1>
                          </v-col>
                          <v-divider></v-divider>
                          <v-col cols="12" class="white">
                            <v-row justify="space-between">
                              <p class="font-weight-bold">Medicine Name</p>
                              <p class="font-weight-bold">Qty</p>
                              <p class="font-weight-bold">Amount</p>
                            </v-row>
                            <v-row v-for="(value, i) in tempbill" :key="i" justify="space-between">
                              <p>{{ value.medicinename }}</p>
                              <p>{{ value.qty }}</p>
                              <p>{{ value.unitprice }}</p>
                            </v-row>
                            <v-row justify="end">
                              <v-col cols="12" md="4">
                                <v-card style="background-color: #66BB6A;">
                                  <h4>Total: {{ total }}</h4>
                                  <h4>GST: {{ gst }}</h4>
                                  <h4>NetPrice: {{ netpay }}</h4>
                                </v-card>
                              </v-col>
                            </v-row>
                          </v-col>
                        </v-row>
                      </v-container>
                    </v-sheet>
                    <v-btn text @click="dialog.value = false">Print</v-btn>
                  </v-card>
                </template>
              </v-dialog>
  
              <!-- Save Button -->
              <v-btn class="white--text" @click="save" style="background-color: #003366;">
                Save
              </v-btn>
            </v-col>
  
            <!-- Bill Info Section -->
            <v-col cols="12" md="12" class="d-flex justify-space-around my-4">
              <h4 style="color: #003366;">Bill No: {{ billnum }}</h4>
              <h4 style="color: #003366;">Date: {{ data }}</h4>
              <h4 style="color: #003366;">Total: {{ total }}</h4>
              <h4 style="color: #003366;">GST: {{ gst }}</h4>
              <h4 style="color: #003366;">Netpay: {{ netpay }}</h4>
            </v-col>
          </v-row>
  
          <!-- Data Table -->
          <v-data-table :headers="headers" :items="tempbill" :items-per-page="5" class="elevation-1" style="background-color: #FFFFFF; color: #003366;"></v-data-table>
        </v-container>
      </v-sheet>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        medname: '',
        qty: '',
        billno: 6,
        data: new Date().toLocaleDateString(),
        total: 0,
        gst: '18%',
        netpay: 0,
        bname: '',
        uprice: 0,
        stock: this.$store.state.stock,
        billdetails: this.$store.state.billdetails,
        items: this.$store.state.medicinemaster,
        billmaster: this.$store.state.billmaster,
        billnum: 6,
        tempbill: [],
        headers: [
          { text: 'Medicine Name', align: 'start', sortable: false, value: 'medicinename' },
          { text: 'Brand Name', value: 'brandname' },
          { text: 'Qty', value: 'qty' },
          { text: 'Unit Price', value: 'unitprice' }
        ]
      }
    },
    props: {
      currentuserid: String
    },
    methods: {
      add() {
        for (var i in this.items) {
          if (this.medname === this.items[i].medicinename) {
            this.bname = this.items[i].brandname
          }
        }
  
        for (var k in this.stock) {
          if (this.medname === this.stock[k].medicinename) {
            if (this.stock[k].quantity >= this.qty) {
              this.stock[k].quantity -= this.qty
              this.uprice = Number(this.stock[k].amount)
              this.total += Number(this.qty) * this.uprice
              this.netpay = this.total + (this.total * 18) / 100
              let newitem = {
                billno: this.billnum,
                medicinename: this.medname,
                brandname: this.bname,
                qty: Number(this.qty),
                unitprice: this.uprice
              }
              this.tempbill.push(newitem)
            } else {
              alert('Stock is not available')
            }
          }
        }
      },
      print() {},
      save() {
        let date = new Date().toLocaleDateString()
        let newbillmaster = {
          billno: this.billnum,
          billdate: date,
          billamount: this.total,
          billgst: this.gst,
          netprice: this.netpay,
          userid: this.currentuserid
        }
        this.billmaster.push(newbillmaster)
  
        for (var m in this.tempbill) {
          this.billdetails.push(this.tempbill[m])
        }
  
        this.billnum += 1
        this.tempbill = []
        this.netpay = 0
        this.total = 0
      }
    }
  }
  </script>
  
  <style scoped>
  .green {
    background-color: #66BB6A;
  }
  
  .green.lighten-2 {
    background-color: #4CAF50;
  }
  
  .green.lighten-1 {
    background-color: #81C784;
  }
  
  .primary {
    background-color: #003366;
  }
  </style>
  
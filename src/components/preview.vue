<template>
    <div>
      <v-sheet width="100%" class="main-bg">
        <v-container>
          <v-row class="header-row">
            <v-col cols="12">
              <h1 class="text-center shop-name">Medical Shop Name</h1>
            </v-col>
          </v-row>
          <v-divider></v-divider>
          <v-row class="details-header">
            <v-col cols="4" class="text-start">
              <h2 class="header-text">Medicine Name</h2>
            </v-col>
            <v-col cols="4" class="text-center">
              <h2 class="header-text">Quantity</h2>
            </v-col>
            <v-col cols="4" class="text-end">
              <h2 class="header-text">Amount</h2>
            </v-col>
          </v-row>
          <v-divider></v-divider>
          <v-row v-for="(item, index) in items" :key="index" class="medicine-row">
            <v-col cols="4" class="text-start">
              <p class="item-text">{{ item.name }}</p>
            </v-col>
            <v-col cols="4" class="text-center">
              <p class="item-text">{{ item.quantity }}</p>
            </v-col>
            <v-col cols="4" class="text-end">
              <p class="item-text">{{ item.amount }}</p>
            </v-col>
          </v-row>
        </v-container>
      </v-sheet>
    </div>
  </template>
  
  <script>
  export default {
    name: "MedicalShop",
    data() {
      return {
        items: [
          { name: "Paracetamol", quantity: 2, amount: "₹100" },
          { name: "Cough Syrup", quantity: 1, amount: "₹150" },
          { name: "Vitamin C", quantity: 3, amount: "₹90" },
        ],
      };
    },
  };
  </script>
  
  <style scoped>
  .main-bg {
    background-color: #002b5b; 
    color: #ffffff;
    padding: 20px;
    border-radius: 8px;
  }
  
  .header-row {
    margin-bottom: 20px;
  }
  .shop-name {
    font-family: "Arial", sans-serif;
    font-weight: 700;
    color: #ffffff;
  }
  
  .details-header {
    background-color: #81d8d0; 
    padding: 10px;
    border-radius: 4px;
  }
  .header-text {
    font-family: "Arial", sans-serif;
    font-size: 18px;
    font-weight: 600;
    color: #002b5b;
  }
  
  .medicine-row {
    margin-top: 10px;
    padding: 10px 0;
    border-bottom: 1px solid #ccc;
  }
  .item-text {
    font-size: 16px;
    color: #ffffff;
  }
  
  .v-divider {
    border-color: #ffffff;
    margin: 10px 0;
  }
  </style>
  
<template>
  <div>
    <v-container>
      <v-card class="elevation-3" style="background-color: #A8E6CF;">
        <v-data-table
          :headers="headers"
          :items="history"
          :items-per-page="5"
          class="elevation-1"
          style="border-radius: 10px;"
        >
          <template v-slot:[`column.userid`]="{ column }">
            <span class="text-h6" style="color: #003366;">{{ column.text }}</span>
          </template>
          <template v-slot:[`column.login`]="{ column }">
            <span class="text-h6" style="color: #003366;">{{ column.text }}</span>
          </template>
          <template v-slot:[`column.logout`]="{ column }">
            <span class="text-h6" style="color: #003366;">{{ column.text }}</span>
          </template>

          <template v-slot:item="{ item, index }">
            <tr :style="index % 2 === 0 ? 'background-color: #f2f8f2;' : 'background-color: #ffffff;'">
              <td>{{ item.userid }}</td>
              <td>{{ item.login }}</td>
              <td>{{ item.logout }}</td>
            </tr>
          </template>

        </v-data-table>
      </v-card>
    </v-container>
  </div>
</template>

<script>
export default {
  data () {
    return {
      history: this.$store.state.loginhistory,
      headers: [
        {
          text: 'User ID',
          align: 'start',
          sortable: false,
          value: 'userid',
        },
        { text: 'LogIn', value: 'login' },
        { text: 'Log Out', value: 'logout' },
      ],
    }
  },
}
</script>

<style scoped>
.v-data-table-header th {
  background-color: #003366; /* Navy Blue for the header */
  color: #ffffff; /* White text */
  font-weight: bold;
  text-align: left;
}

.v-data-table .v-data-table__mobile-row {
  background-color: #f2f8f2; /* Light Mint Green for even rows */
}

.v-data-table .v-data-table__row:hover {
  background-color: #e0f7fa; 
}

.v-card {
  border-radius: 12px;
  padding: 20px;
}

@media (max-width: 600px) {
  .v-data-table {
    font-size: 0.9rem;
  }
}
</style>

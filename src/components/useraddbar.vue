
import { RouterLink } from 'vue-router';
<template>
  <div class="app-container">
    <v-card class="main-card" width="1000">
      <v-expansion-panels focusable>
        <v-expansion-panel>
          <v-expansion-panel-header class="header-text">
            Add User
          </v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-form>
              <v-container>
                <v-row>
                  <v-col cols="12" md="3">
                    <v-text-field
                      v-model="userid"
                      :counter="20"
                      label="User ID"
                      required
                      outlined
                      class="custom-input"
                    ></v-text-field>
                  </v-col>

                  <v-col cols="12" md="3">
                    <v-text-field
                      v-model="password"
                      :counter="20"
                      label="Password"
                      required
                      outlined
                      type="password"
                      class="custom-input"
                    ></v-text-field>
                  </v-col>

                  <v-col cols="12" md="3">
                    <v-overflow-btn
                      class="custom-dropdown"
                      :items="dropdown_edit"
                      label="Role"
                      v-model="role"
                      outlined
                    ></v-overflow-btn>
                  </v-col>

                  <v-col cols="12" md="3">
                    <v-btn
                      color="primary"
                      class="custom-btn"
                      @click="add"
                    >
                      Add User
                    </v-btn>
                  </v-col>
                </v-row>
              </v-container>
            </v-form>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </v-card>
  </div>
</template>

<script>
export default {
  name: '',
  data() {
    return {
      dropdown_edit: [
        { text: 'Biller' },
        { text: 'Manage' },
        { text: 'System Admin' },
        { text: 'Inventory' },
      ],
      userid: '',
      password: '',
      role: '',
    };
  },
  methods: {
    add() {
      const newuser = { username: this.userid, password: this.password, role: this.role };
      this.$store.state.login.push(newuser);
      alert('User added');
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

.header-text {
  background-color: #002d62;
  color: white;
  font-weight: bold;
  padding: 12px 16px;
  font-size: 16px;
}

.custom-input {
  background-color: #ffffff;
  border-radius: 4px;
}

.custom-dropdown {
  background-color: #ffffff;
  border: 1px solid #d1e7dd;
  border-radius: 4px;
}

.custom-btn {
  background-color: #002d62;
  color: white;
  font-weight: bold;
  text-transform: uppercase;
  padding: 8px 16px;
}

.custom-btn:hover {
  background-color: #004080;
}

.v-expansion-panel-header {
  border-bottom: 1px solid #d1e7dd;
}

.v-btn.primary {
  background-color: #002d62 !important;
  color: #fff !important;
}

.v-btn.primary:hover {
  background-color: #003f8a !important;
}

.v-expansion-panel-content {
  background-color: #e9f7ef;
}
</style>

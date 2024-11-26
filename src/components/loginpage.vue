<template>
    <div>
        <v-app-bar color="blue darken-2" elevation="4" app>
            <v-container>
                <v-row justify="space-between" align="center">
                    <v-col cols="6">
                        <v-icon large color="white">mdi-hospital</v-icon>
                        <span class="text-h5 white--text ml-2">Hospital Login Portal</span>
                    </v-col>
                    <v-col cols="6" class="text-right">
                        <v-dialog transition="dialog-top-transition" scrollable max-width="500">
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn color="white black--text" v-bind="attrs" v-on="on">Login</v-btn>
                            </template>
                            
                            <template>
                                <v-card>
                                    <v-card-title class="justify-center">User Login</v-card-title>
                                    <v-divider></v-divider>
                                    <v-card-text>
                                        <v-container>
                                            <v-row>
                                                <v-col cols="12">
                                                    <v-text-field
                                                        v-model="userid"
                                                        label="Username"
                                                        prepend-inner-icon="mdi-account"
                                                        outlined
                                                        dense
                                                    ></v-text-field>
                                                </v-col>
                                                <v-col cols="12">
                                                    <v-text-field
                                                        v-model="password"
                                                        type="password"
                                                        label="Password"
                                                        prepend-inner-icon="mdi-lock"
                                                        outlined
                                                        dense
                                                    ></v-text-field>
                                                </v-col>
                                                <v-col cols="12" class="text-center">
                                                    <v-btn
                                                        :disabled="!userid || !password"
                                                        color="blue darken-2"
                                                        dark
                                                        @click="login"
                                                    >
                                                        <v-icon left>mdi-login</v-icon> Login
                                                    </v-btn>
                                                </v-col>
                                            </v-row>
                                        </v-container>
                                    </v-card-text>
                                    <v-divider></v-divider>
                                    <v-card-actions>
                                        <v-spacer></v-spacer>
                                        <v-btn text @click="$emit('close')">Close</v-btn>
                                    </v-card-actions>
                                </v-card>
                            </template>
                        </v-dialog>
                    </v-col>
                </v-row>
            </v-container>
        </v-app-bar>
        <template>
  <div class="fullscreen-image">
    <v-img
      src="../assets/img1.jpg"
      cover
      alt="Full Screen Background"
    ></v-img>
  </div>
</template>
    </div>
    
</template>

<script>
export default {
    name: 'LoginPage',
    data() {
        return {
            userid: '',
            password: '',
            data: this.$store.state.login,
            date: '',
            userrole: '',
            logout: 'notyet',
        };
    },
    methods: {
        login() {
            let user = this.userid;
            let passw = this.password;
            this.date = new Date().toLocaleString();
            const userEntry = this.data.find((userData) => userData.username === user);
            if (userEntry && userEntry.password === passw) {
                this.userrole = userEntry.role;
                // alert(`Logged in as ${this.userrole}`);
                this.$store.state.loginhistory.push({
                    userid: user,
                    login: this.date,
                    logout: this.logout,
                });
                this.$router.push('/medlogin');
            } else {
                alert('Invalid Username or Password!');
            }
        },
    },
};
</script>

<style scoped>
.v-btn {
    font-weight: bold;
}
.v-card-title {
    font-size: 1.2rem;
    font-weight: bold;
    color: #1976d2;
}
.v-divider {
    margin: 10px 0;
}
.v-app-bar {
    position: fixed;
}
.fullscreen-image{
    height: 70vh;
}


</style>

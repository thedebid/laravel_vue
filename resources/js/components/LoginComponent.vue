<template>
<v-app id="inspire">
    <v-content>
        <v-container class="fill-height" fluid>
            <v-row align="center" justify="center">
                <v-col cols="12" sm="8" md="4">
                    <v-card class="elevation-12">
                        <v-toolbar color="primary" dark flat>
                            <v-toolbar-title>Login form</v-toolbar-title>

                        </v-toolbar>
                        <v-card-text>

                            <v-form ref="form" v-model="valid">

                                <v-progress-linear :active="loading" :indeterminate="loading" absolute bottom color="green accent-4"></v-progress-linear>

                                <v-text-field label="Email" name="email" v-model="email" :rules="emailRules" prepend-icon="mdi-account-circle" type="email" required />

                                <v-text-field id="password" label="Password" name="password" :append-icon="value ? 'mdi-eye-outline' : 'mdi-eye-off-outline'"
    @click:append="() => (value = !value)"
    :type="value ? 'password' : 'text'" v-model="password" :rules="passwordRules" prepend-icon="mdi-lock-outline" required />
                            </v-form>
                        </v-card-text>
                        <v-card-actions>
                            <v-spacer />
                            <v-btn color="primary" :disabled="!valid" block @click="login">Login</v-btn>
                        </v-card-actions>
                    </v-card>
                    <v-snackbar v-model="snackbar">
                        {{ text }}
                        <v-btn color="pink" text @click="snackbar = false">
                            Close
                        </v-btn>
                    </v-snackbar>

                </v-col>
            </v-row>
        </v-container>
    </v-content>
</v-app>
</template>

<script>
export default {
    data() {
        return {
            value: String,
            valid: true,
            email: '',
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
            ],
            password: '',
            passwordRules: [
                v => !!v || 'Password is required',

            ],
            loading: false,
            snackbar: false,
            text: '',
        }
    },
    methods: {
        login: function () {

            // Add a request interceptor
            axios.interceptors.request.use((config) => {
                // Do something before request is sent
                this.loading = true;
                return config;
            }, (error) => {
                this.loading = false;
                return Promise.reject(error);
            });

            // Add a response interceptor
            axios.interceptors.response.use((response) => {
                this.loading = false;
                return response;
            }, (error) => {
                this.loading = false;
                return Promise.reject(error);
            });

            axios.post('/api/login', {
                'email': this.email,
                'password': this.password
            }).then(res => {
                localStorage.setItem('token', res.data.token)
                localStorage.setItem('LoggedIn', 'true')
                this.$router.push('/admin')
                    .then(res => console.log('Logged In Successfully'))
                    .catch(err => console.log(err))
            }).catch(err => {
                this.text = err.response.data.status, this.snackbar = true
            });
        }
    }
}
</script>

<style scoped>

</style>

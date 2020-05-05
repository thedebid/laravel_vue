<template>
<v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app clipped>
        <v-list dense>
            <v-list-item v-for="item in items" :key="item.text" link :to="item.action">
                <v-list-item-action>
                    <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                    <v-list-item-title>
                        {{ item.text }}
                    </v-list-item-title>
                </v-list-item-content>
            </v-list-item>
            <v-subheader class="mt-4 grey--text text--darken-1">REPORTS</v-subheader>
            <v-list>
                <v-list-item v-for="item in items2" :key="item.text" link>
                    <v-list-item-avatar>
                        <img :src="`https://randomuser.me/api/portraits/men/${item.picture}.jpg`" alt="">
                    </v-list-item-avatar>
                    <v-list-item-title v-text="item.text" />
                </v-list-item>
            </v-list>
            <v-list-item class="mt-4">
                <v-list-item-title class="grey--text text--darken-1">  <v-switch v-model="theme" class="ma-4" label="Switch Theme"></v-switch></v-list-item-title>
            </v-list-item>
            <v-list-item link @click="logout">
                <v-list-item-action>
                    <v-icon color="grey darken-1">mdi-logout</v-icon>
                </v-list-item-action>
                <v-list-item-title class="grey--text text--darken-1">Logout</v-list-item-title>
            </v-list-item>
        </v-list>
    </v-navigation-drawer>

    <v-app-bar app clipped-left color="primary" dense>
        <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
        <v-btn text to="/admin" left>
            <v-icon class="mx-4" large>
                mdi-bug
            </v-icon>
            <v-toolbar-title class="mr-12 align-center">
                <span class="title">Admin Panel</span>
            </v-toolbar-title>
        </v-btn>
        <v-spacer />
        <v-row align="center" style="max-width: 250px">
            <v-text-field :append-icon-cb="() => {}" placeholder="Search..." single-line append-icon="mdi-magnify" color="white" hide-details />
        </v-row>
    </v-app-bar>

    <v-content>
        <v-container class="">
            <v-row justify="center" align="center">
                <v-col>
                    <router-view></router-view>
                    <v-snackbar v-model="snackbar">
                        Logged In Successfully
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
    props: {
        source: String,

    },
    data: () => ({
        snackbar: false,
        drawer: null,
        theme:true,
        items: [{
                icon: 'mdi-account',
                text: 'Users'
            },
            {
                icon: 'mdi-post-outline',
                text: 'Posts'
            },
            {
                icon: 'mdi-circle-edit-outline',
                text: 'Pages'
            },
            {
                icon: 'mdi-briefcase-edit-outline',
                text: 'Categories'
            },
            {
                icon: 'mdi-account-badge-outline',
                text: 'Roles',
                action: '/admin/roles'
            },
        ],
        items2: [{
                picture: 28,
                text: 'Joseph'
            },
            {
                picture: 38,
                text: 'Apple'
            },
            {
                picture: 48,
                text: 'Xbox Ahoy'
            },
            {
                picture: 58,
                text: 'Nokia'
            },
            {
                picture: 78,
                text: 'MKBHD'
            },
        ],
    }),
    methods: {
        logout: function () {
            localStorage.removeItem('token');
            this.$router.push('/login')
                .then(res => {
                    this.text = "You  are logged out successfully";
                    this.snackbar = true;
                })
                .catch(err => console.log(err))
        }
    },
    created() {
        this.$vuetify.theme.dark = true

    },
        watch:{
        theme: function (old, newval) {
            this.$vuetify.theme.dark = old;
        }
    },
    mounted() {
        this.snackbar = localStorage.getItem('LoggedIn') ? true : false;
        localStorage.removeItem('LoggedIn')
    }
}
</script>

<style scoped>

</style>

<template>
<fragment>
    <h1>Sessions</h1>
    <v-card class="mt-2">
        <v-app-bar flat color="transparent">
            <v-toolbar-title></v-toolbar-title>
        </v-app-bar>
        <v-divider></v-divider>
        <v-card-text class="pa-0">
            <v-data-table :headers="headers" :items="$store.getters['sessions/list']" item-key="uid" :sort-by="['started_at']" :sort-desc="[true]" disable-pagination hide-default-footer>
                <template v-slot:item.device="{ item }">
                    <v-chip>
                        {{ item.device }}
                        <v-icon small right @click.stop v-clipboard="item.uid" v-clipboard:success="showCopySnack">mdi-content-copy</v-icon>
                    </v-chip>
                </template>

                <template v-slot:item.ip_address="{ item }">
                    <code>{{ item.ip_address }}</code>
                </template>

                <template v-slot:item.active="{ item }">
                    <v-icon color="success" v-if="item.active">check_circle</v-icon>
                    <v-tooltip bottom v-else>
                        <template #activator="{ on }">
                            <v-icon v-on="on">check_circle</v-icon>
                        </template>
                        <span>active {{ item.last_seen | moment("from", "now") }}</span>
                    </v-tooltip>
                </template>
            </v-data-table>
        </v-card-text>
    </v-card>
    <v-snackbar v-model="copySnack" :timeout=3000>Device UID copied to clipboard</v-snackbar>
</fragment>
</template>

<script>
export default {
    methods: {
        showCopySnack() {
            this.copySnack = true;
        }
    },

    data() {
        return {
            copySnack: false,

            headers: [{
                    text: "Device",
                    value: "device"
                },
                {
                    text: "Username",
                    value: "username"
                },
                {
                    text: "IP Address",
                    value: "ip_address"
                },
                {
                    text: "Active",
                    value: "active"
                },
            ]
        };
    },

    created() {
        this.$store.dispatch("sessions/fetch");
    }
};
</script>

<template>
  <div>
    <v-menu offset-y top>
      <template v-slot:activator="{ on: menu }">
        <v-btn class="ml-2" icon :dark="!light" v-on="menu">
          <v-icon>mdi-settings</v-icon>
        </v-btn>
      </template>
      <v-list two-line subheader>
        <v-subheader class="heading h2 white--text primary py-0 px-2">Pilot Options</v-subheader>
        <v-list-item @click="$refs.printDialog.show()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon>mdi-printer</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Print</v-list-item-title>
            <v-list-item-subtitle>
              Print tabletop-ready character and mech sheets
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item @click="$refs.statblockDialog.show()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon>mdi-file-document-box</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Generate Statblock</v-list-item-title>
            <v-list-item-subtitle>
              Get a plaintext representation of this character's build
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item @click="$refs.exportDialog.show()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon>mdi-export-variant</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Export Pilot</v-list-item-title>
            <v-list-item-subtitle>Open the pilot export menu</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item @click="$refs.cloudDialog.show()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon>mdi-cloud</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Cloud Settings</v-list-item-title>
            <v-list-item-subtitle>
              Manage this pilot's cloud data and share codes
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-list-item @click="$refs.cloud.sync()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon>mdi-cloud-sync</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>Quick Sync</v-list-item-title>
            <v-list-item-subtitle v-if="!pilot.CloudID && !pilot.IsUserOwned">
              Sync cloud with pilot (creates new cloud data)
            </v-list-item-subtitle>
            <v-list-item-subtitle v-else-if="!pilot.CloudID || pilot.IsUserOwned">
              Sync cloud with pilot (overwrites cloud data)
            </v-list-item-subtitle>
            <v-list-item-subtitle v-else>
              Sync pilot with cloud (overwrites local data)
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider />
        <v-list-item @click="$refs.deleteDialog.show()">
          <v-list-item-icon class="ma-0 mr-2 mt-3">
            <v-icon color="error">mdi-delete</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title class="error--text">Delete Pilot</v-list-item-title>
            <v-list-item-subtitle class="error--text">
              Remove this pilot from the roster
            </v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-menu>
    <print-dialog ref="printDialog" :pilot="pilot" />
    <export-dialog ref="exportDialog" :pilot="pilot" />
    <statblock-dialog ref="statblockDialog" :pilot="pilot" />
    <cloud-dialog ref="cloudDialog" :pilot="pilot" />
    <delete-dialog ref="deleteDialog" :pilot="pilot" @delete="deletePilot()" />
    <cloud-manager ref="cloud" :pilot="pilot" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

import CloudManager from './CloudManager.vue'
import CloudDialog from './CloudDialog.vue'
import StatblockDialog from './StatblockDialog.vue'
import ExportDialog from './ExportDialog.vue'
import PrintDialog from './PrintDialog.vue'
import DeleteDialog from './DeletePilotDialog.vue'

import { getModule } from 'vuex-module-decorators'
import { PilotManagementStore } from '@/store'

export default Vue.extend({
  name: 'edit-menu',
  components: {
    CloudManager,
    CloudDialog,
    StatblockDialog,
    ExportDialog,
    PrintDialog,
    DeleteDialog,
  },
  props: {
    pilot: {
      type: Object,
      required: true,
    },
    light: {
      type: Boolean,
    },
  },
  methods: {
    deletePilot() {
      this.$router.push('/pilot_management')
      const store = getModule(PilotManagementStore, this.$store)
      store.deletePilot(this.pilot)
    },
  },
})
</script>

<template>
  <v-container fluid>
    <v-sheet class="pa-5">
      <div></div>
      <v-sheet
        :elevation="6"
        :rounded="true"
        class="mx-auto"
        height="350"
        width="350"
      >
        <v-card class="mx-auto" max-width="500">
          <v-card-title
            class="text-h6 font-weight-regular justify-space-between"
          >
            <span>{{ currentTitle }}</span>
            <v-avatar color="primary" size="24" v-text="state.step"></v-avatar>
          </v-card-title>

          <v-window v-model="state.step">
            <v-window-item :value="1">
              <v-card-text>
                <v-text-field
                  v-model="db.username"
                  label="username"
                  placeholder="libretime"
                ></v-text-field>
                <v-text-field
                  v-model="db.password"
                  label="password"
                  placeholder="libretime"
                ></v-text-field>
                <v-text-field
                  v-model="db.dbName"
                  label="Database Name"
                  placeholder="libretime"
                ></v-text-field>
                <v-text-field
                  v-model="db.host"
                  label="password"
                  placeholder="localhost"
                ></v-text-field>
                <span class="text-caption grey--text text--darken-1">
                  Enter your Libretime database settings here.
                </span>
              </v-card-text>
            </v-window-item>

            <v-window-item :value="2">
              <v-card-text>
                <v-text-field label="Password" type="password"></v-text-field>
                <v-text-field
                  label="Confirm Password"
                  type="password"
                ></v-text-field>
                <span class="text-caption grey--text text--darken-1">
                  Please enter a password for your account
                </span>
              </v-card-text>
            </v-window-item>

            <v-window-item :value="3">
              <v-text-field
                v-model="general.host"
                label="Webserver Host"
                placeholder="localhost"
              ></v-text-field>
              <v-text-field
                v-model="general.port"
                label="Webserver Port"
                placeholder="80"
              ></v-text-field>
              <span class="text-caption grey--text text--darken-1">
                These settings are automatically pulled from your web server. In
                most circumstances you won't need to change them.
              </span>
            </v-window-item>

            <v-window-item :value="4">
              <v-text-field
                v-model="media.folder"
                label="Media Folder"
              ></v-text-field>
              <span class="text-caption grey--text text--darken-1">
                Here you can set your default media directory for Libretime.
              </span>
            </v-window-item>

            <v-window-item :value="5">
              <div class="pa-4 text-center">
                <v-img
                  class="mb-4"
                  contain
                  height="128"
                  src="/logo.svg"
                ></v-img>
                <h3 class="text-h6 font-weight-light mb-2">
                  Welcome to Libretime
                </h3>
                <span class="text-caption grey--text"
                  >Let's get you on the air!</span
                >
              </div>
            </v-window-item>
          </v-window>

          <v-divider></v-divider>

          <v-card-actions>
            <v-btn v-if="state.step > 1" text @click="prevPage"> Back </v-btn>
            <v-spacer></v-spacer>
            <v-btn
              v-if="state.step < 5"
              color="primary"
              depressed
              @click="nextPage"
            >
              Next
            </v-btn>
            <router-link to="/">
              <v-btn v-if="state.step == 5" color="primary" depressed> Login </v-btn>
            </router-link>
          </v-card-actions>
        </v-card>
      </v-sheet>
    </v-sheet>
  </v-container>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from "vue";

export default defineComponent({
  name: "SetupWizard",
  setup() {
    const state = reactive({ step: 1 });

    function nextPage() {
      state.step++;
    }

    function prevPage() {
      state.step--;
    }

    const currentTitle = computed(() => {
      switch (state.step) {
        case 1:
          return "Database Settings";
        case 2:
          return "RabbitMQ Settings";
        case 3:
          return "General Settings";
        case 4:
          return "Media Settings";
        default:
          return "Manual Step: Start Libretime Services";
      }
    });

    const db = reactive({
      username: "libretime",
      password: "libretime",
      dbName: "libretime",
      host: "localhost",
    });

    const general = reactive({
      host: "localhost",
      port: "80",
    });

    const media = reactive({
      folder: "",
    });

    return { state, nextPage, prevPage, currentTitle, db, general, media };
  },
});
</script>

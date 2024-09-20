<template>
    <v-container fluid class="pa-5">
      <v-row>
        <!-- Header Section -->
        <v-col cols="12">
          <v-card class="elevation-26" outlined>
            <v-card-title class="text-center">
              <v-img src="#" height="100" contain>
                <v-container fill-height>
                  <v-row align="center" justify="center">
                    <v-col class="text-center">
                      <h1 class="display-2 blue--text">Welcome to Your Dashboard</h1>
                      <v-btn color="blue" @click="navigateTo('overview')" class="mt-4">Get Started</v-btn>
                    </v-col>
                  </v-row>
                </v-container>
              </v-img>
            </v-card-title>
            <v-card-subtitle class="text-center">
              This page is about Dashboard to connect MQTTX and DATA for IoT
            </v-card-subtitle>
          </v-card>
        </v-col>
      </v-row>
  
      <!-- Quick Links Section -->
      <v-row class="mt-4">
        <v-col cols="12">
          <v-card class="elevation-26">
            <v-card>
              <v-card-title>
                <v-icon left>mdi-view-dashboard</v-icon> <!-- Dashboard icon for the title -->
                <span class="headline">Quick to Dashboard</span>
              </v-card-title>
              <v-card-actions>
                <v-btn @click="navigateTo('dashboard1')" class="ma-2">
                  <v-icon left>mdi-arrow-right-bold</v-icon> <!-- Arrow icon for the button -->
                  Go to Dashboard
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-card>
        </v-col>
      </v-row>
  
      <!-- Image Section -->
      <v-row class="mt-4">
        <v-col cols="12">
          <v-card class="elevation-26">
            <v-card-title class="headline">Iot & MQTTX</v-card-title>
            <v-card-subtitle class="text-center">
              
            </v-card-subtitle>
            <v-card-actions>
              <v-img
                src="data:image/png;base64,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"
                height="200"
                class="mx-auto my-2"
                contain
              ></v-img>
              <v-img
                src="data:image/png;base64,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"
                height="200"
                class="mx-auto my-2"
                contain
              ></v-img>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    methods: {
      navigateTo(page) {
        this.$router.push({ name: page });
      }
    }
  }
  </script>
  
  <style scoped>
  .v-card {
    border-radius: 10px;
    overflow: hidden;
  }
  
  .v-img {
    border-bottom: 1px solid #e0e0e0;
  }
  
  .display-2 {
    font-size: 3rem;
    font-weight: 700;
  }
  
  .white--text {
    color: #ffffff;
  }
  
  .mt-4 {
    margin-top: 1rem;
  }
  
  .pa-5 {
    padding: 2rem;
  }
  
  .headline {
    font-size: 1.5rem;
    font-weight: 500;
  }
  
  .elevation-26 {
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .mx-auto {
    margin-left: auto;
    margin-right: auto;
  }
  
  .my-2 {
    margin-top: 0.5rem;
    margin-bottom: 0.5rem;
  }
  </style>
  
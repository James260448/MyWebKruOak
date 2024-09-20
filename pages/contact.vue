<template>
    <v-container>
      <v-row justify="center">
        <v-col cols="12" md="8">
          <v-card>
            <v-card-title class="headline">Contact Us</v-card-title>
            <v-card-text>
              <v-form>
                <v-text-field
                  v-model="name"
                  label="Name"
                  outlined
                  required
                ></v-text-field>
  
                <v-text-field
                  v-model="email"
                  label="Email"
                  type="email"
                  outlined
                  required
                ></v-text-field>
  
                <v-text-field
                  v-model="subject"
                  label="Subject"
                  outlined
                  required
                ></v-text-field>
  
                <v-textarea
                  v-model="message"
                  label="Message"
                  outlined
                  required
                ></v-textarea>
  
                <v-btn color="primary" @click="submitForm">Send Message</v-btn>
              </v-form>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        name: '',
        email: '',
        subject: '',
        message: ''
      };
    },
    methods: {
      submitForm() {
        // การส่งข้อมูล form อาจใช้ axios หรือ fetch API
        if (this.name && this.email && this.subject && this.message) {
          alert('Message sent successfully!');
        } else {
          alert('Please fill in all fields.');
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .v-card {
    margin-top: 50px;
  }
  .v-btn {
    margin-top: 20px;
  }
  </style>
  

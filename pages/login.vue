<template>
  <div>
    <v-img
      class="mx-auto my-6"
      max-width="150"
      src="data:image/png;base64,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"
    ></v-img>

    <v-card
      class="mx-auto pa-12 pb-8"
      elevation="8"
      max-width="448"
      rounded="lg"
    >
      <div class="text-subtitle-1 text-medium-emphasis">Account</div>

      <v-text-field
        v-model="email"
        density="compact"
        placeholder="Email address"
        prepend-inner-icon="mdi-email-outline"
        variant="outlined"
      ></v-text-field>

      

      <v-text-field
        v-model="passwd"
        :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'"
        :type="visible ? 'text' : 'password'"
        density="compact"
        placeholder="Enter your password"
        prepend-inner-icon="mdi-lock-outline"
        variant="outlined"
        @click:append-inner="visible = !visible"
      ></v-text-field>

      

      <v-btn
        class="mb-8"
        color="blue"
        size="large"
        variant="tonal"
        block
        @click="doLogin"
      >
        Log In
      </v-btn>

      <v-btn
        class="mb-8"
        color="blue"
        size="large"
        variant="tonal"
        block
        @click="goToRegister"
      >
        Register
      </v-btn>

      <v-card-text class="text-center">
        <a
          class="text-blue text-decoration-none"
          href="reg"
          rel="noopener noreferrer"
          target="_blank"
        >
          Sign up now <v-icon icon="mdi-chevron-right"></v-icon>
        </a>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
definePageMeta({
  layout: "#",
});
import axios from 'axios';

export default {
  data: () => ({
    visible: false,
    email: '',
    passwd: ''
  }),
  methods: {
    async doLogin() {
      let forms = {
        email: this.email,
        passwd: this.passwd
      };
      console.log(forms);
      const response = await axios.post('http://localhost:7000/login', forms);
      const data = response.data;
      console.log(data.status);
      if (data.status == 'success') {
        console.log(data.row.email);
        window.sessionStorage.setItem('token', JSON.stringify(data.token));
        this.$router.replace('/home1');
      } else {
        this.$router.replace('/login');
      }
    },
    goToRegister() {
      this.$router.push('/reg');
    }
  }
};

</script>
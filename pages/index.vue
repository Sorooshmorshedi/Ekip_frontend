<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card
        class="mx-auto mt-16 mb-16"
        width="800"
        height="800"
      >
        <v-img
          class="white--text align-end"
          src="https://i.pinimg.com/originals/fd/c7/80/fdc7807c0efb23bdf00a78e32e9daf9a.gif"
        >
        </v-img>
        <v-card-title class="text--blue">Dongeto</v-card-title>
        <v-card-subtitle class="pb-0">
          by Artech
        </v-card-subtitle>

        <v-card-text class="text--primary">
          <div>Create your ekip & add your friends</div>
          <div>have fun & spend safely then we estimate all the dutches</div>
        </v-card-text>

        <v-card-actions>

          <v-btn
            class="ml-3"
            @click="Login"
            color="green"
            dark
          >
            Login
          </v-btn>

          <v-btn
            class="ml-2"
            @click="Sign"
            color="lime"
            text
          >
            dont have a account?
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {}
  },

  methods: {
    Login() {
      window.location.href = "http://127.0.0.1:8000/login"
    },
    Sign() {
      window.location.href = "http://127.0.0.1:8000/signup"
    }


  }
}


</script>

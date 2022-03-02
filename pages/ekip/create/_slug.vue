<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card
        class="mx-auto mt-16 mb-16"
      >
        <v-toolbar
          flat
          color=green
        >
          <v-icon>mdi-account</v-icon>
          <v-toolbar-title class="font-weight-light">
            create ekip
          </v-toolbar-title>
        </v-toolbar>


        <v-card-text>
          <v-text-field
            v-model="name"
            color="white"
            label="ekip name"
          ></v-text-field>
        </v-card-text>

        <v-card-text>
          <v-textarea
            v-model="about"
            filled
            auto-grow
            label="about"
            rows="4"
            row-height="40"
            shaped
          ></v-textarea>
        </v-card-text>


        <v-card-actions>
          <v-spacer></v-spacer>
          <nuxt-link :to="{ path: '/account/' + this.slug }">
            <v-btn
              class="ma-2"
              color=red
            >
              cancell
            </v-btn>
          </nuxt-link>

          <v-btn
            class="ma-2"
            color=#59981A
            @click="createekip"
          >
            create ekip
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>

</template>


<script>
export default {
  name: 'create account',
  data() {
    return {
      slug: this.$route.params.slug,
      name: '',
      about: '',
      data: '',
      file: null,
      member: [this.$route.params.slug]


    }
  },

  methods: {
    createekip() {
      this.$axios.$post('http://127.0.0.1:8000/ekip', {
        name: this.name,
        about: this.about,
        member: this.member,
        creater: this.slug
      })
        .then(response => {
          console.log(response)
          window.alert('ekip created')
          window.location.href = "http://127.0.0.1:3000/account/" + this.$route.params.slug + "?acid=" + this.$route.query.acid;
        }).catch(response => {
        window.alert('!!!!')
      })
    }
  }
}
</script>
<style>

</style>

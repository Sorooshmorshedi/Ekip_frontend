<template>

  <v-container>
    <div class="ma-5">
      <v-toolbar
        color=withe
        dense
        rounded
        floating
      >
        <v-text-field
          v-model="token"
          label="add ekip invite"
          hide-details
          prepend-icon="mdi-account"
          single-line
        ></v-text-field>
        <v-btn icon @click="addekip()">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-toolbar>
    </div>


    <v-row justify="center">
      <v-col
        v-for="ekip in ekips"
        :key="ekip.id"
        cols="auto"
      >
        <v-card
          :elevation="n - 1"
          height='auto'
          width="550"
          class="green"
        >
          <v-card-title class="pa-1 light-green darken-4">
            <v-col>
              <v-dialog
                transition="dialog-bottom-transition"
                max-width="600"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="green"
                    v-bind="attrs"
                    v-on="on"
                  >ekip invite token
                  </v-btn>
                </template>
                <template v-slot:default="dialog">
                  <v-card>
                    <v-toolbar
                      color="green"
                      dark
                    >invite token
                    </v-toolbar>
                    <v-card-text>
                      <div class="text-h2 pa-12">{{ ekip.token }}</div>
                    </v-card-text>
                    <v-card-actions class="justify-end">
                      <v-btn
                        text
                        @click="dialog.value = false"
                      >Close
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </template>
              </v-dialog>


            </v-col>
            <h3 class="text-h5 font-weight-bold text-right mr-4 grow">
              {{ ekip.name }}
            </h3>
            <v-avatar>
              <v-img
                src="https://play-lh.googleusercontent.com/j5Wm3LrSwN1TO7FxcFxhCl-ho0Z6PJz_t67527SrAVhtt84Gf5wKZUu7Gez3mI0otCG8"></v-img>
            </v-avatar>
          </v-card-title>
          <v-card-text class=" mx-auto text-center">
            <v-card-text class="text-h5">{{ ekip.about }}</v-card-text>
            <v-card
              class="mx-auto"
              max-width="300"
            >
              <v-img
                src="https://i.pinimg.com/originals/2d/35/31/2d3531e5c7ca3bf17aa8c61dce02fe2c.gif"
                height="100"
              ></v-img>
              <v-col justify="center">
                <v-card-subtitle>
                  created by :
                  {{ ekip.creater_name }}
                </v-card-subtitle>
                <v-card-subtitle>
                  unpaid :
                  <v-icon>mdi-cash</v-icon>
                  {{ ekip.unpaid }}
                </v-card-subtitle>
                <v-card-subtitle>
                  total expense :
                  <v-icon>mdi-cash</v-icon>
                  {{ ekip.expense_sum }}
                </v-card-subtitle>

              </v-col>


              <v-card-actions class="justify-center">
                <nuxt-link :to="{ path: '/ekip/expense/create/'+ slug + '?ekip=' + ekip.id}">
                  <v-btn

                    class="ml-16"
                    color="lime accent-4">
                    new expense
                  </v-btn>
                </nuxt-link>

                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-card-text>
          <v-card-actions>
            <nuxt-link :to="{ path: '/ekip/'+ slug + '/' + '?ekid=' + ekip.id}">
              <v-btn
                class="mr-8 black--text"
                color='yellow'
              >details
              </v-btn>
            </nuxt-link>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>

export default {

  data() {
    return {
      token: '',
      show: false,
      dialog: false,
      slug: this.$route.params.slug,
      ekipss: '',
    }
  },

  mounted() {

  },

  async asyncData({$axios, params}) {
    const ekips = await $axios.$get('http://127.0.0.1:8000/account/ekip/' + params.slug)
    console.log(ekips);
    return {ekips}
  },


  methods: {
    deleteEkip(ekip) {
      this.$axios.$delete('http://127.0.0.1:8000/ekip/' + ekip.id)
        .then(response => {
          console.log(response)
          window.alert('ekip deleted')
          window.location.href = 'http://127.0.0.1:3000/ekip/all/' + this.slug
        })
    },
    addekip() {
      this.$axios.$get('http://127.0.0.1:8000/invite/ekip/' + this.slug + '/' + this.token)
        .then(response => {
          console.log(response)
          window.alert('ekip added')
          window.location.href = 'http://127.0.0.1:3000/ekip/all/' + this.slug
        })
    }

  }
}


</script>
<style scoped>
h3 {
  color: snow;
  padding-top: 10px;
}

</style>

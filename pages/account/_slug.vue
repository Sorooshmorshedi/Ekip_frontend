<template>
  <div>
    <v-card
      class="mx-auto mt-16 mb-16"
      width="850"
    >
      <v-card
        dark
        flat
      >
        <v-card-title class="pa-2 light-green darken-4">
          <v-col>
            <nuxt-link :to="{ path: '/account/edit/' + this.slug}">
              <v-btn
                fab
                small
                color="green darken-2"
              >
                <v-icon>mdi-account-edit-outline</v-icon>
              </v-btn>
            </nuxt-link>
            <v-btn
              color="red darken-4"
              fab
              small
              dark
              @click.stop="dialog = true"
            >
              <v-icon>mdi-account-remove-outline</v-icon>
            </v-btn>

            <v-dialog
              v-model="dialog"
              max-width="290"
            >
              <v-card color='withe'>
                <v-card-title class="text-h5">
                  Delete this account?
                </v-card-title>

                <v-card-text>
                  all your information will be lost but
                  your user will not be deleted and you can come back any time, login and create another profile
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>

                  <v-btn
                    color="green darken-1"
                    text
                    @click="dialog = false"
                  >
                    cancell
                  </v-btn>

                  <v-btn
                    color="red darken-1"
                    text
                    @click="dialog = false , deleteAcc(accounts[0])"
                  >
                    delete account
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>


          </v-col>
          <h3 class="text-h5 font-weight-bold text-right mr-4 grow">
            {{ accounts[0].first_name }}
          </h3>
          <v-avatar>
            <v-img :src=accounts[0].profile_picture></v-img>
          </v-avatar>
        </v-card-title>
        <v-container>
          <v-row>

            <v-col key="1">
              <v-img
                height="400"
                width="400"
                class="ma-1"
                :src=accounts[0].profile_picture
              >
              </v-img>
            </v-col>
            <v-col key="2">
              <v-card-text class="py-0">
                <v-card-title>
                  {{ accounts[0].first_name }} {{ accounts[0].last_name }}
                </v-card-title>
                <v-card-text>{{ accounts[0].status }}</v-card-text>
                <v-row>
                  <v-col key="1">
                    <v-card style="background-color: darkseagreen" class="pa-1">
                      <v-toolbar>
                        <nuxt-link :to="{ path: '/account/shares/' + this.slug }">
                          <v-toolbar-title>Your shares</v-toolbar-title>
                        </nuxt-link>
                      </v-toolbar>
                      <v-sheet
                        v-for="share in shares"
                        rounded
                        class=" ma-1 pa-1  align-center justify-center"
                        height="auto"
                        color=#223800
                        width="auto"
                      >
                        <h4>
                          <v-avatar color="green" size="30" class="mr-2">
                            <v-img
                              lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                              :src=share.creditor_pic
                              class="white--text align-center ma-4"
                              height=auto
                              width=auto
                            ></v-img>
                          </v-avatar>
                          {{ share.debtor }} $ to {{ share.creditor_name }}
                        </h4>

                      </v-sheet>
                    </v-card>
                  </v-col>
                  <v-col key="2">
                    <v-card style="background-color: darkseagreen" class="pa-1">
                      <v-toolbar>
                        <nuxt-link :to="{ path: '/account/creditor/' + this.slug}">
                          <v-toolbar-title>Your creditor</v-toolbar-title>
                        </nuxt-link>
                      </v-toolbar>
                      <v-sheet
                        v-for="share in shares1"
                        rounded
                        class=" ma-1 pa-1  align-center justify-center"
                        height="auto"
                        color=#223800
                        width="auto"
                      >
                        <h4>
                          <v-avatar color="green" size="30" class="mr-2">
                            <v-img
                              lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                              :src=share.account_pic
                              class="white--text align-center ma-4"
                              height=auto
                              width=auto
                            ></v-img>
                          </v-avatar>
                          {{ share.account_name }} : {{ share.debtor }} $
                        </h4>

                      </v-sheet>
                    </v-card>

                  </v-col>
                </v-row>

              </v-card-text>

            </v-col>
          </v-row>

        </v-container>


      </v-card>
      <v-card-actions class="pa-5">
        <nuxt-link :to="{ path: '/ekip/create/' + this.slug}">
          <v-btn
            class="mr-8 green--text text--darken-4"
            color=#EEFF41
          >
            create a ekip
          </v-btn>
        </nuxt-link>
        <nuxt-link :to="{ path: '/ekip/all/' + this.slug}">
          <v-btn
            class="mr-8"
            color=#43A047
          >
            your ekips
          </v-btn>
        </nuxt-link>

        </nuxt-link>
        <nuxt-link :to="{ path: '/account/count/' + this.slug }">
          <v-btn
            class="mr-8 green--text text--darken-4"
            color="lime"
          >
            counting
          </v-btn>
        </nuxt-link>



      </v-card-actions>

    </v-card>
  </div>
</template>


<script>
export default {
  data() {
    return {
      dialog: false,
      slug: this.$route.params.slug,
      shares: '',
      shares1: '',
    }
  },

  mounted() {
    console.log(this.slug);
    console.log(this.$route);
    this.$axios.$get('http://127.0.0.1:8000/account/share/' + this.slug)
      .then(response => {
        console.log(response)
        this.shares = response
      });
    this.$axios.$get('http://127.0.0.1:8000/account/creditor/' + this.slug)
      .then(response => {
        console.log(response)
        this.shares1 = response
      })


  },

  async asyncData({$axios, params}) {
    const accounts = await $axios.$get('http://127.0.0.1:8000/account/' + params.slug)
    console.log(accounts);
    return {accounts}
  },


  methods: {
    deleteAcc(account) {
      this.$axios.$delete('http://127.0.0.1:8000/account/' + account.id)
        .then(response => {
          console.log(response)
          window.alert('account deleted')
          window.location.href = 'http://127.0.0.1:3000'
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

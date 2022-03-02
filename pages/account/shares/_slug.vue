<template>
  <div>
    <v-row class="mt-md-auto">
      <v-col
        cols="12"
        md="4"
        class=" order-1 order-md-0 flex-row flex-md-column  justify-md-center  "
      >
      </v-col>
      <v-card class="mt-16 mb-16 " color=#beff59 height="auto" width="350px">
        <v-toolbar
          color=#3f6602
          dark
        >
          <nuxt-link :to="{ path: '/account/'+ slug}">
            <v-btn icon>
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </nuxt-link>
          <v-spacer></v-spacer>

          <v-toolbar-title>Your shares</v-toolbar-title>

        </v-toolbar>

        <v-sheet
          v-for="share in shares"
          rounded
          class=" ma-3 pa-2  align-center justify-center"
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
            you debtor {{ share.debtor }} $ to {{share.creditor_name}}
          </h4>
          <h4 style="color:red">
            <v-avatar color=#223800 size="50" class="ma-2">
              <v-img
                lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                src="https://cdn.dribbble.com/users/356550/screenshots/4085010/bettr-send-money.gif"
                class="white--text align-center ma-4"
                height=auto
                width=auto
              ></v-img>
            </v-avatar>
            <v-btn
              class="ml-1"
              color="primary"
              small
              @click="payShare(share)"

            >Pay Share
            </v-btn>
            <v-icon class="ml-5" color="red">mdi-account-group</v-icon>
            {{ share.ekip_name }}


          </h4>

        </v-sheet>

      </v-card>
      </v-col>
    </v-row>

  </div>
</template>


<script>
export default {
  data() {
    return {
      show: false,
      dialog: false,
      slug: this.$route.params.slug,
      myid: this.$route.params.slug,
      shares: '',

    }
  },


  mounted() {
    console.log(this.id)
    this.$axios.$get('http://127.0.0.1:8000/account/share/' + this.slug)
      .then(response => {
        console.log(response)
        this.shares = response
      })
  },


  methods: {
    payShare(share) {
      this.$axios.$delete('http://127.0.0.1:8000/share/' + share.id)
        .then(response => {
          console.log(response)
          window.alert('pay done')
          window.location.href = 'http://127.0.0.1:3000/account/shares/' + this.slug
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

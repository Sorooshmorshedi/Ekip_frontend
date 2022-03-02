<template>
  <div>
    <v-row class="mt-md-auto">
      <v-col
        cols="12"
        md="4"
        class=" order-1 order-md-0 flex-row flex-md-column  justify-md-center  "
      >
      </v-col>
      <v-card class="mt-16 mb-16 " color=#beff59 height="auto" width="400px">
        <v-toolbar
          color=#3f6602
          dark
        >
          <nuxt-link :to="{ path: '/ekip/'+ slug + '?acid=' + myid + '&ekid=' + ekid}">
            <v-btn icon v-if="ekid">
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </nuxt-link>
          <nuxt-link :to="{ path: '/account/friends/'+ slug + '?acid=' + myid}">
            <v-btn icon v-if="!ekid">
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </nuxt-link>

          <v-spacer></v-spacer>

          <v-toolbar-title>friend obligor to you </v-toolbar-title>

        </v-toolbar>

        <v-sheet
          v-if="shares"
          rounded
          class=" ma-3 pa-2  align-center justify-center"
          height="auto"
          color=#223800
          width="auto"
        >
          <h4>
            <v-avatar color=#3f6602 size="30" class="mr-2">
              <v-img
                lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                src="https://d1x98t9ot91xc.cloudfront.net/wp-content/uploads/2021/03/rh_cash_on_visit.png?x79430"
                class="white--text align-center ma-4"
                height=auto
                width=auto
              ></v-img>
            </v-avatar>
            you debtor {{ shares.debtor }} $ to {{shares.creditor}}
          </h4>
          <h4 style="color:crimson">
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
              small
              @click="payShare(share)"
              color="primary"
            >pay share
            </v-btn>



          </h4>

        </v-sheet>
        <v-sheet
          v-if="!shares"
          rounded
          class=" ma-3 pa-2  align-center justify-center"
          height="auto"
          color=#223800
          width="auto"
        >
          <h4>
            <v-avatar color=#3f6602 size="30" class="mr-2">
              <v-img
                lazy-src="127.0.0.1:3000/account/friends/creditor/3gh5gh0ms6ms2ac6bb8bb1ac9ms0ms9aa2gh7bb0sm8aa5sm4ms9ms?acid=1&fid=2"
                src="https://d1x98t9ot91xc.cloudfront.net/wp-content/uploads/2021/03/rh_cash_on_visit.png?x79430"
                class="white--text align-center ma-4"
                height=auto
                width=auto
              ></v-img>
            </v-avatar>
            {{shares1.name}} debtor {{ shares1.debtor }} $  to {{shares1.creditor}}
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
      myid: this.$route.query.acid,
      fid: this.$route.query.fid,
      ekid: this.$route.query.ekid,

      shares: '',
      shares1: '',

    }
  },


  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/obligor/' + this.slug + '/' + this.fid)
      .then(response => {
        console.log(response)
        this.shares = response
      });
    this.$axios.$get('http://127.0.0.1:8000/creditor/' + this.slug + '/' + this.fid)
      .then(response => {
        console.log(response)
        this.shares1 = response
      });

  },


  methods: {
    payShare(share) {
      this.$axios.$get('http://127.0.0.1:8000/debtor/pay/' + this.slug + '/' + this.fid)
        .then(response => {
          console.log(response)
          window.alert('pay done')
          window.location.href = 'http://127.0.0.1:3000/account/shares/' + slug + '/?acid=' + myid
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

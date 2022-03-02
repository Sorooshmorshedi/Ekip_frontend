<template>
  <div>
    <v-row class="mt-md-auto">
      <v-col
        cols="12"
        md="4"
        class=" order-1 order-md-0 flex-row flex-md-column  justify-md-center  "
      >
      </v-col>
      <v-card class="mt-16 mb-16 " color=#beff59 height="auto" width="auto">
        <v-toolbar
          color=#3f6602
          dark
        >
          <nuxt-link :to="{ path: '/ekip/all/'+ slug + '?acid=' + myid}">
            <v-btn icon>
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </nuxt-link>
          <v-spacer></v-spacer>

          <v-toolbar-title>Ekip members</v-toolbar-title>

        </v-toolbar>

        <v-sheet
          v-for="share in shares"
          rounded
          class=" ma-3 pl-3  align-center justify-center"
          height="auto"
          color=#223800
          width="auto"
        >
          <h3 class="mr-2 ml-2">
            <v-avatar color=#223800>
              <v-img
                lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                :src=share.account_pic
                class="white--text align-center ma-4"
                height=auto
                width=auto
              ></v-img>
            </v-avatar>
            {{ share.account_name }} debtor {{share.debtor}} $ to {{share.creditor_name}}
          </h3>
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
      ekid: this.$route.query.ekip,
      shares: '',

    }
  },


  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/ekip/share/' + this.ekid)
      .then(response => {
        console.log(response)
        this.shares = response
      })
  },


  methods: {
    deleteEkip(ekip) {
      this.$axios.$delete('http://127.0.0.1:8000/ekip/' + ekip.id)
        .then(response => {
          console.log(response)
          window.alert('ekip deleted')
          window.location.href = 'http://127.0.0.1:3000/account/' + slug
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

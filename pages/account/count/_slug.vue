<template>

  <v-container>


    <v-row justify="center">
      <v-col
        v-for="count in counts"
        :key="count.id"
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

            </v-col>
            <h3 class="text-h5 font-weight-bold text-right mr-4 grow">
              {{ count.name }}
            </h3>
            <v-avatar>
              <v-img
                src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTjHW38nn6dzQxSxYw0BoYmxtyqI3eeH1h9h78VmtZb_SoYqy0KD3Ub0r_aQ8QUT5E90ok&usqp=CAU"></v-img>
            </v-avatar>
          </v-card-title>
          <v-card-text class=" mx-auto text-center">
            <v-card-text class="text-h5">
              <v-avatar>
                <v-img
                  src="https://i.pinimg.com/originals/37/6c/d7/376cd78f534efc70562acd902db2752c.gif">

                </v-img>
              </v-avatar>
              {{ count.name }} debtor
              <v-icon>mdi-cash</v-icon>
              {{ count.debtor }}
              <v-icon>mdi-cash</v-icon>
              to {{ count.creditor }}
            </v-card-text>
          </v-card-text>
          <v-card-actions>
            <v-btn
              v-if="count.id == slug"
              @click="payShare(count)"
              class="mr-8 black--text"
              color='primary'
            >pay share
            </v-btn>
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
    const counts = await $axios.$get('http://127.0.0.1:8000/creditor/' + params.slug)
    console.log(counts);
    return {counts}
  },


  methods: {
    payShare(count) {
      this.$axios.$get('http://127.0.0.1:8000/debtor/pay/' + this.slug + '/' + count.fid)
        .then(response => {
          console.log(response)
          window.alert('pay done')
          window.location.href = 'http://127.0.0.1:3000/account/shares/' + slug
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

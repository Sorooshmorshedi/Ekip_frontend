<template>
  <div>
    <div class="ma-5">
      <v-toolbar
        color=withe
        dense
        rounded
        floating
      >
        <v-text-field
          v-model="searched"
          hide-details
          prepend-icon="mdi-account"
          single-line
        ></v-text-field>
        <v-btn icon @click="searchacc()">
          <v-icon>mdi-magnify</v-icon>
        </v-btn>
      </v-toolbar>
    </div>

    <v-card
      v-for="account in accounts"
      class="mx-auto mt-16 mb-16"
      max-width="650"
    >
      <v-card
        dark
        flat
      >
        <v-card-title class="pa-2 light-green darken-4">

          <v-col>
            <v-btn
              @click="addFriend(account)"
              fab
              small
              color="green darken-2"
            >
              <v-icon>mdi-plus</v-icon>
            </v-btn>

          </v-col>

          <h3 class="text-h5 font-weight-bold text-right mr-4 grow">
            {{ account.first_name }}
          </h3>
          <v-avatar>
            <v-img :src=account.profile_picture></v-img>
          </v-avatar>
        </v-card-title>
        <v-img
          class="ma-1"
          :src=account.profile_picture
        >
        </v-img>
      </v-card>
      <v-card-text class="py-0">
        <v-card-title>
          {{ account.first_name }} {{ account.last_name }}
        </v-card-title>
        <v-card-text>{{ account.status }}</v-card-text>
      </v-card-text>

    </v-card>
  </div>
</template>


<script>
export default {
  data() {
    return {
      searched: '',
      slug: this.$route.params.slug,
      myid: this.$route.query.acid,
      accounts: '',

    }
  },

  methods: {
    searchacc() {
      this.$axios.$get('http://127.0.0.1:8000/search/' + this.searched)
        .then(response => {
          console.log(response)
          this.accounts = response
        })
    },
    addFriend(account) {
      this.$axios.post('http://127.0.0.1:8000/friends', {
        account: this.myid,
        friends: account.id
      })
        .then(response => {
          console.log(response)
          window.alert('friend added')
        }).catch(response => {
        this.unFriend(account)
      })
    },
    unFriend(account) {
      this.$axios.$delete('http://127.0.0.1:8000/unfriend/' + account.id + '/' + this.myid)
        .then(response => {
          console.log(response)
          window.alert('unfriend')
        }).catch(response => {
        this.addFriend(account)
      })
    },


  },
}


</script>
<style scoped>
h3 {
  color: snow;
  padding-top: 10px;
}

</style>

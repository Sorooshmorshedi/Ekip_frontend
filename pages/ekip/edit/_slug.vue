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
            edit ekip
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
          <v-card-title> ekip members</v-card-title>

          <v-autocomplete
            class="ml-10 mr-10 mt-5 mb-15"
            v-model="accounts"
            :items="account"
            item-text="name"
            item-value="id"
            deletable-chips
            filled
            chips
            multiple
            rounded
          >
            <template v-slot:selection="data">
              <v-chip
                v-bind="data.attrs"
                :input-value="data.selected"
                close
              >
                <v-avatar left>
                  <v-img :src="data.item.pic"></v-img>
                </v-avatar>
                {{ data.item.name }} {{ data.item.lname }}
              </v-chip>
            </template>
            <template v-slot:item="data">
              <template>
                <v-list-item-avatar>
                  <img :src="data.item.pic">
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title v-html="data.item.name"></v-list-item-title>
                  <v-list-item-subtitle v-html="data.item.lname"></v-list-item-subtitle>
                </v-list-item-content>
              </template>
            </template>
          </v-autocomplete>

        </v-card-text>


        <v-card-actions>
          <v-spacer></v-spacer>
          <nuxt-link :to="{ path: '/ekip/' + this.slug + '/?ekid=' + this.$route.query.ekip}">
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
            @click="editekip"
          >
            edit ekip
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>

</template>


<script>
export default {
  data() {
    return {
      accounts: [],
      slug: this.$route.params.slug,
      account: [],
      account_name: [],
      ekipid: this.$route.query.ekip,
      name: '',
      about: '',
      data: '',
      file: null,


    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/account')
      .then(response => {
        for (let ac in response) {
          this.account.push({
            'name': response[ac].first_name,
            'lname': response[ac].last_name,
            'id': response[ac].id,
            'pic': response[ac].profile_picture
          })
          this.account_name.push(response[ac].first_name)

        }
      })
  },


  methods: {
    editekip() {
      this.$axios.$put('http://127.0.0.1:8000/ekip/' + this.ekipid + '/', {
        name: this.name,
        about: this.about,
        member: this.accounts,
      })
        .then(response => {
          console.log(response)
          window.alert('ekip edited')
          window.location.href = "http://127.0.0.1:3000/ekip/" + this.$route.params.slug + "?ekid=" + this.$route.query.ekip;
        }).catch(response => {
        window.alert('!!!!')
      })
    }
  }
}
</script>
<style>

</style>


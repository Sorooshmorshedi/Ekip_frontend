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
            Add a Expense
          </v-toolbar-title>
        </v-toolbar>

        <v-card-text>
          <v-text-field
            v-model="title"
            color="white"
            label="title"
          ></v-text-field>
        </v-card-text>

        <v-card-text>
          <v-text-field
            v-model="amount"
            color="white"
            label="amount"
          ></v-text-field>
        </v-card-text>


        <v-card-text>
          <v-textarea
            v-model="description"
            filled
            auto-grow
            label="description"
            rows="4"
            row-height="40"
            rounded
          ></v-textarea>
        </v-card-text>
        <v-checkbox
          class="ml-10"
          v-model="checkbox1"
          :label="`equal share?`"
        ></v-checkbox>
        <v-card-title> who buy ?</v-card-title>


        <v-autocomplete
          class="ml-15 mr-15 mt-5 mb-15"
          v-model="account"
          :items="accounts"
          item-text="item.id"
          item-value="id"
          deletable-chips
          filled
          chips
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
              {{ data.item.name }} {{data.item.lname}}
            </v-chip>
          </template>
          <template v-slot:item="data">
            <template >
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
            @click="postExpene"
          >
            Add expense
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
      account: '',
      slug: this.$route.params.slug,
      ekid: this.$route.query.ekip,
      title: '',
      description: '',
      amount: '',
      account_name: [],
      accounts: [],
      checkbox1: true,


    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/ekip/member/' + this.ekid)
      .then(response => {

        for (let ac in response) {
          this.accounts.push({
            'name': response[ac].first_name,
            'lname': response[ac].last_name,
            'id': response[ac].id,
            'pic': response[ac].profile_picture
          })
          this.account_name.push(response[ac].first_name)
        }
      })
    console.log(this.accounts)
  },


  methods: {
    postExpene() {
      this.$axios.$post('http://127.0.0.1:8000/expense', {
        title: this.title,
        ekip: this.ekid,
        description: this.description,
        amount: this.amount,
        equal_share: this.checkbox1,
        payer: this.account
      })
        .then(response => {
          console.log(response)
          window.alert('expense added')
          if (this.checkbox1 == true) {
            window.location.href = "http://127.0.0.1:3000/ekip/" + this.$route.params.slug + '/?ekid=' + this.ekid };
          if (this.checkbox1 == false) {
            window.location.href = "http://127.0.0.1:3000/ekip/expense/shares/" + this.$route.params.slug + '/?ekid=' + this.ekid + '&exid=' + response.id + '&am=' + response.amount };


        }).catch(response => {
        window.alert(response)
      })
    }
  }
}
</script>
<style>

</style>

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
            Add share of members
          </v-toolbar-title>
        </v-toolbar>
        <h4 style="color: #85f7ba ; margin: 30px"> expense amount = {{am}} $ </h4>
        <h4 style="color: #3daf5a  ; margin: 30px" v-if="leftover , leftover>= 0">left = {{leftover}} $</h4>
        <h4 style="color: #f03838  ; margin: 30px" v-if="leftover < 0">shares is more than amount</h4>


        <v-simple-table class="ma-10">
          <template v-slot:default>
            <thead>
            <tr>
              <th class="text-left">
                Name
              </th>
              <th class="text-left">
                share
              </th>
            </tr>
            </thead>
            <tbody>
            <tr
              v-for="(account, index) in accs"
              :key="account.name"
            >
              <td>{{ account.first_name }} {{ account.last_name }}</td>
              <td>
                <v-text-field
                  v-model="values[account.id]"
                  @change="change"
                  color="white"
                  label="share amount"
                ></v-text-field>
              </td>
              <td>
                <v-btn @click="show">a</v-btn>
              </td>

            </tr>
            </tbody>
          </template>
        </v-simple-table>


        <v-card-actions>
          <v-spacer></v-spacer>
          <nuxt-link :to="{ path: '/ekip/' + this.slug + '/?ekid=' + this.$route.query.ekid}">
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
            Add shares
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
      values: [],
      account: '',
      slug: this.$route.params.slug,
      exid: this.$route.query.exid,
      ekid: this.$route.query.ekid,
      am: this.$route.query.am,
      title: '',
      description: '',
      amount: 'a',
      account_name: [],
      accounts: [],
      checkbox1: true,
      accs: '',
      leftover: '',


    }
  },
  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/ekip/member/' + this.ekid)
      .then(response => {
        this.accs = response

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
    console.log(this.accounts.length)

    console.log(this.accounts)
  },


  methods: {
    show() {
      for (let ac in this.accs) {
        console.log(this.values[this.accs[ac].id])

      }
    },
    change() {
      var over = this.am
      for (let ac in this.accs) {
        if(this.values[this.accs[ac].id]) {
          over -= this.values[this.accs[ac].id]
        }
        else {
          continue
        }
        this.leftover = over
      }
    },

    postExpene() {
      for (let ac in this.accs){
        this.$axios.$post('http://127.0.0.1:8000/membershare', {
          amount: this.values[this.accs[ac].id],
          account: this.accs[ac].id,
          expense: this.exid,
        })
          .then(response => {
            console.log(response)
            window.alert('share added')
            window.location.href = "http://127.0.0.1:3000/ekip/" + this.$route.params.slug + '/?ekid=' + this.ekid;
          }).catch(response => {
          window.alert(response)
        })
      }

    },
    postAndContinue() {
      this.$axios.$post('http://127.0.0.1:8000/membershare', {
        amount: this.amount,
        account: this.account,
        expense: this.exid,
      })
        .then(response => {
          console.log(response)
          window.alert('share added')
          window.location.href = "http://127.0.0.1:3000/ekip/expense/shares/" + this.$route.params.slug + '/?ekid=' + this.ekid + '&exid=' + this.exid;
        }).catch(response => {
        window.alert(response)
      })
    }

  }
}
</script>
<style>

</style>

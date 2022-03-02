<template>
  <div id="app">
    <nuxt-link :to="{ path: '/ekip/all/' + slug}">
      <v-btn
        fab
        smallv
        style="margin: 20px"
        color="red"
      >
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
    </nuxt-link>

    <v-card
      v-for="ekip in ekips"
      class="mx-auto mt-16 mb-16"
      width="1300"
    >
      <v-card
        dark
        flat
      >
        <v-card-title class="pa-2 light-green darken-4">
          <v-col>
            <nuxt-link :to="{ path: '/ekip/edit/' + slug +'?ekip=' + ekip.id}">
              <v-btn
                v-if="ekip.creater == slug"
                fab
                smallv
                color="green darken-2"
              >
                <v-icon>mdi-wrench</v-icon>
              </v-btn>
            </nuxt-link>
            <v-btn
              v-if="ekip.creater == slug"
              color="red darken-4"
              fab
              small
              dark
              @click="deleteEkip(ekip)"
            >
              <v-icon>mdi-delete</v-icon>
            </v-btn>



          </v-col>
          <h3 class="text-h5 font-weight-bold text-right mr-4 grow">
            {{ ekip.name }}
          </h3>
          <v-avatar>
            <v-img
              src="https://play-lh.googleusercontent.com/j5Wm3LrSwN1TO7FxcFxhCl-ho0Z6PJz_t67527SrAVhtt84Gf5wKZUu7Gez3mI0otCG8"></v-img>
          </v-avatar>
        </v-card-title>
      </v-card>
      <v-dialog
        transition="dialog-bottom-transition"
        max-width="600"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            style="margin: 20px"
            color="green"
            v-bind="attrs"
            v-on="on"
          >invite link
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
              <div class="text-h5 pa-12">
                <nuxt-link to=""
                <a>http://127.0.0.1:8000/invite/{{ ekip.token }}</a>
              </div>
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

      <v-card-text class=" mx-auto text-center">
        <v-sheet style="background-color: darkgreen; margin-left: 200px; margin-right: 200px;">
          <v-card-text class="text-h6">{{ ekip.about }}</v-card-text>
          <v-card-text class="text-h7"> created by : {{ ekip.creater_name }}</v-card-text>
          <v-card-text class="text-h6"> total expense : {{ ekip.expense_sum }} $
            <v-icon>mdi-cash</v-icon>
            Upaid : {{ ekip.unpaid }} $
          </v-card-text>
        </v-sheet>
        <v-container>
          <v-row>
            <v-col key="1">
              <v-card
                style="background-color: darkgreen"
                class="pa-1 ma-1"
                width="350"
                outlined
                rounded
              >
                <v-toolbar
                  color=#3f6602
                  dark
                >
                  <v-toolbar-title>Ekip members</v-toolbar-title>
                </v-toolbar>
                <v-sheet
                  v-for="member in members"
                  rounded
                  class=" ma-2 pl-2  "
                  height="60"
                  color=#223800
                  width="320"
                >
                  <nuxt-link
                    :to="{ path: '/account/friends/obligor/'+ slug + '?acid=' + myid + '&fid=' + member.id + '&ekid=' + ekip.id}">

                    <h3>
                      <v-avatar color=#223800>
                        <v-img
                          lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
                          :src=member.profile_picture
                          class="white--text"
                          height=auto
                          width=auto
                        ></v-img>
                      </v-avatar>
                      {{ member.first_name }} {{ member.last_name }}
                    </h3>
                  </nuxt-link>

                </v-sheet>

              </v-card>
            </v-col>
            <v-col key="2">

              <v-card
                width="400"
                style="background-color: darkgreen"
                class="pa-1 ma-1"
                outlined
                rounded
              >
                <v-toolbar
                  color=#3f6602
                  dark
                >
                  <nuxt-link :to="{ path: '/ekip/expense/create/'+ slug + '/?ekip=' + ekid}">
                    <v-btn fab small>
                      <v-icon>mdi-plus</v-icon>
                    </v-btn>
                  </nuxt-link>
                  <v-spacer></v-spacer>

                  <v-toolbar-title>Ekip Expenses</v-toolbar-title>

                </v-toolbar>

                <v-sheet
                  v-for="expense in expenses"
                  rounded
                  class=" ma-2 pa-1 "
                  height="auto"
                  color=#223800
                  width="auto"
                >
                  <v-btn @click="deleteExpense(expense)" fab x-small color="red" class="float-right ma-1">
                    <v-icon>mdi-delete-outline</v-icon>
                  </v-btn>
                  <v-dialog
                    v-model="dialog"
                    persistent
                    max-width="600px"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn fab x-small color="orange" class="float-right ma-1" v-bind="attrs" v-on="on"
                             @click="setitem(expense)">
                        <v-icon>mdi-pencil</v-icon>
                      </v-btn>
                    </template>
                    <v-card>
                      <v-card-title>
                        <span class="text-h5">edit expense</span>
                      </v-card-title>
                      <v-card-text>
                        <v-container>
                          <v-row>
                            <v-col
                              cols="12"
                              sm="6"
                              md="4"
                            >
                              <v-text-field
                                label="name"
                                v-model="put_name"
                                required
                              ></v-text-field>
                            </v-col>
                            <v-col
                              cols="12"
                              sm="6"
                              md="4"
                            >
                              <v-text-field
                                label="description"
                                v-model="put_des"
                              ></v-text-field>
                            </v-col>
                            <v-col
                              cols="12"
                              sm="6"
                              md="4"
                            >
                              <v-text-field
                                label="amount"
                                v-model="put_amount"
                                required
                              ></v-text-field>
                            </v-col>

                            <v-card-text>payer</v-card-text>
                            <v-autocomplete
                              v-model="put_payer"
                              :items="accounts"
                              item-text="name"
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
                          </v-row>
                        </v-container>
                      </v-card-text>
                      <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn
                          text
                          @click="dialog = false"
                        >
                          Close
                        </v-btn>
                        <v-btn
                          v-if="equal != false"
                          color="green darken-1"
                          text
                          @click=" editExpense(expense); dialog = false"
                        >
                          Save
                        </v-btn>
                        <v-btn
                          v-if="equal == false"
                          color="green darken-1"
                          text
                          @click=" editExpense1(expense) ; dialog = false"
                        >
                          Save
                        </v-btn>

                      </v-card-actions>
                    </v-card>
                  </v-dialog>

                  <h3>
                    <v-avatar color=#223800>
                      <v-img
                        lazy-src="https://img.favpng.com/0/20/6/money-management-expense-png-favpng-PSgAcbjbNz110h35T61vVRPdt.jpg"
                        src="https://img.favpng.com/0/20/6/money-management-expense-png-favpng-PSgAcbjbNz110h35T61vVRPdt.jpg"
                        class="white--text  ma-1"
                      ></v-img>
                    </v-avatar>
                    {{ expense.amount }}$ for {{ expense.title }}
                  </h3>
                  <h5 style="margin-left: 50px">{{ expense.description }}</h5>
                  <h5 style="margin-left: 50px">by : {{ expense.payer_name }} {{ expense.payer_lname }}</h5>
                  <h6 style="margin-left: 150px">{{ expense.date }}</h6>
                </v-sheet>

              </v-card>
            </v-col>
            <v-col key="3">

              <v-card
                style="background-color: darkgreen"
                class="pa-1 ma-1"
                outlined
                rounded
              >
                <v-toolbar
                  color=#3f6602
                  dark
                >
                  <v-toolbar-title>Ekip shares</v-toolbar-title>
                </v-toolbar>

                <v-sheet
                  v-for="share in shares"
                  rounded
                  class=" ma-3 pl-3  align-center justify-center"
                  height="auto"
                  color=#223800
                  width="auto"
                >
                  <v-btn
                    small
                    v-if="share.account == slug"
                    class="ma-1 float-right"
                    @click="payShare(share)"
                    color="primary"
                  >pay
                  </v-btn>

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
                    {{ share.account_name }} debtor {{ share.debtor }} $ to {{ share.creditor_name }}
                  </h3>
                </v-sheet>

              </v-card>

            </v-col>
          </v-row>
        </v-container>

      </v-card-text>
    </v-card>
  </div>

</template>


<script>

export default {

  data() {
    return {
      token: '',
      show: false,
      dialog: false,
      slug: this.$route.params.slug,
      ekips: '',
      ekid: this.$route.query.ekid,
      expenses: '',
      members: '',
      shares: '',
      put_payer: '',
      put_ex: '',
      put_id: '',
      put_ekip: '',
      put_amount: '',
      put_name: '',
      put_des: '',
      accounts: [],
      account_name: [],
      equal: true,
    }
  },


  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/ekip/' + this.ekid)
      .then(response => {
        this.ekips = response
        console.log(response)
      });
    this.$axios.$get('http://127.0.0.1:8000/expense/ekip/' + this.ekid)
      .then(response => {
        console.log(response)
        this.expenses = response
      });
    this.$axios.$get('http://127.0.0.1:8000/ekip/member/' + this.ekid)
      .then(response => {
        console.log(response)
        this.members = response
      });
    this.$axios.$get('http://127.0.0.1:8000/ekip/share/' + this.ekid)
      .then(response => {
        console.log(response)
        this.shares = response
      });
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
      });
    console.log(this.accounts)


  },


  methods: {
    setitem(expense) {
      this.put_ekip = expense.ekip
      this.put_payer = expense.payer
      this.put_id = expense.id
      this.equal = expense.equal_share
      console.log(this.put_id);

    },
    deleteEkip(ekip) {
      this.$axios.$delete('http://127.0.0.1:8000/ekip/' + ekip.id)
        .then(response => {
          console.log(response)
          window.alert('ekip deleted')
          window.location.href = 'http://127.0.0.1:3000/ekip/all/' + this.slug
        })
    },
    payShare(share) {
      this.$axios.$delete('http://127.0.0.1:8000/share/' + share.id)
        .then(response => {
          console.log(response)
          window.alert(' share payed !')
          window.location.href = 'http://127.0.0.1:3000/ekip/' + this.slug + '/?ekid=' + this.ekid
        })
    },
    deleteExpense(expense) {
      this.$axios.$delete('http://127.0.0.1:8000/expense/' + expense.id)
        .then(response => {
          console.log(response)
          window.alert('expense deleted')
          window.location.href = 'http://127.0.0.1:3000/ekip/' + this.slug + '/?acid=' + this.myid + '&ekid=' + this.ekid
        })
    },
    editExpense() {
      this.$axios.$put('http://127.0.0.1:8000/expense/' + this.put_id + '/', {
        name: this.put_name,
        description: this.put_des,
        payer: this.put_payer,
        ekip: this.put_ekip,
        amount: parseInt(this.put_amount),

      })
        .then(response => {
          console.log(response)
          console.log(this.put_id)
          window.alert('expense edited')
          window.location.href = 'http://127.0.0.1:3000/ekip/' + this.slug + '/?acid=' + this.myid + '&ekid=' + this.ekid
        }).catch(response => {
        console.log(response)
      })
    },
    editExpense1() {
      this.$axios.$put('http://127.0.0.1:8000/expense/' + this.put_id + '/', {
        name: this.put_name,
        description: this.put_des,
        payer: this.put_payer,
        ekip: this.put_ekip,
        amount: parseInt(this.put_amount),

      })
        .then(response => {
          console.log(response)
          console.log(this.put_id)
          window.alert('expense edited')
          window.location.href = window.location.href = 'http://127.0.0.1:3000/ekip/expense/shares/' + this.slug + '/?ekid=' + this.ekid + '&exid=' + this.put_id
        }).catch(response => {
        console.log(response)
      })
    },


  }
}


</script>
<style scoped>
h3 {
  color: snow;
  padding-top: 10px;
}

</style>

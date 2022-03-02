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
          <nuxt-link :to="{ path: '/ekip/all/'+ slug}">
            <v-btn icon>
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </nuxt-link>
          <v-spacer></v-spacer>

          <v-toolbar-title>Ekip Expense</v-toolbar-title>

        </v-toolbar>

        <v-sheet
          v-for="expense in expenses"
          rounded
          class=" ma-3 pl-3  align-center justify-center"
          height="auto"
          color=#223800
          width="320"
        >
          <h3>
            <v-avatar color=#223800>
              <v-img
                lazy-src="https://img.favpng.com/0/20/6/money-management-expense-png-favpng-PSgAcbjbNz110h35T61vVRPdt.jpg"
                src="https://img.favpng.com/0/20/6/money-management-expense-png-favpng-PSgAcbjbNz110h35T61vVRPdt.jpg"
                class="white--text align-center ma-4"
              ></v-img>
            </v-avatar>
            {{expense.amount}}$ for {{ expense.title }}
          </h3>
          <h5 style="margin-left: 50px">{{expense.description}}</h5>
          <h5 style="margin-left: 50px">by : {{expense.payer_name}} {{expense.payer_lname}}</h5>
          <h6 style="margin-left: 150px">{{expense.date}}</h6>


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
      ekid: this.$route.query.ekip,
      expenses: '',

    }
  },


  mounted() {
    this.$axios.$get('http://127.0.0.1:8000/expense/ekip/' + this.ekid)
      .then(response => {
        console.log(response)
        this.expenses = response
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

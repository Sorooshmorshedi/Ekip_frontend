<template>
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
        <nuxt-link :to="{ path: '/account/all/'+ slug + '?acid=' + myid}">
          <v-btn color=#223800>
            add friend
          </v-btn>
        </nuxt-link>
        <v-spacer></v-spacer>

        <v-toolbar-title>Friends</v-toolbar-title>

      </v-toolbar>

      <v-sheet
        v-for="friend in friends"
        v-if="friend.friends != myid"
        rounded
        class=" ma-3 pl-3  align-center justify-center"
        height="auto"
        color=#223800
        width="320"
      >
        <h3>
          <v-avatar color=#223800>
            <v-img
              lazy-src="https://images.assetsdelivery.com/compings_v2/yehorlisnyi/yehorlisnyi2104/yehorlisnyi210400016.jpg"
              :src=friend.friend_pic
              class="white--text align-center ma-4"
              height=auto
              width=auto
            ></v-img>
          </v-avatar>
          {{ friend.friend_name }} {{ friend.friend_lname }}
        </h3>
        <p style="color: palegreen; margin-left: 100px; ">{{ friend.friend_status }}</p>
        <nuxt-link :to="{ path: '/account/friends/obligor/' + slug + '?acid=' + myid + '&fid=' + friend.friends}">
          <v-btn class="mb-2 yellow black--text">debtors</v-btn>
        </nuxt-link>
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
      slug: this.$route.params.slug,
      myid: this.$route.query.acid,
      ekid: this.$route.query.ekip,
      friends: '',

    }
  },


  mounted() {
    console.log(this.id)
    this.$axios.$get('http://127.0.0.1:8000/account/friends/' + this.slug)
      .then(response => {
        console.log(response)
        this.friends = response
      })
  },


  methods: {}
}


</script>
<style scoped>
h3 {
  color: snow;
  padding-top: 10px;
}

</style>

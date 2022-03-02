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
            Profile Setting
          </v-toolbar-title>
        </v-toolbar>

        <v-card-text>
          <v-card-title> profile picture</v-card-title>
          <input id="image-upload" type="file" ref="file" @change="uploadFile"/>
        </v-card-text>

        <v-card-text>
          <v-text-field
            v-model="username"
            color="white"
            label="user name"
          ></v-text-field>
        </v-card-text>
        <v-card-text>

        <v-text-field
            v-model="f_name"
            color="white"
            label="First name"
          ></v-text-field>
        </v-card-text>
        <v-card-text>

          <v-text-field
            v-model="l_name"
            color="white"
            label="Last name"
          ></v-text-field>
        </v-card-text>

        <v-card-text>
          <v-textarea
            v-model="sts"
            filled
            auto-grow
            label="status"
            rows="4"
            row-height="40"
            shaped
          ></v-textarea>
        </v-card-text>
        <v-text-field
          class="ml-16 mr-16 mt-5 mb-15"
          v-model="password"
          :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
          :rules="[rules.required, rules.min]"
          :type="show1 ? 'text' : 'password'"
          name="input-10-1"
          label="password"

          hint="At least 8 characters"
          counter
          @click:append="show1 = !show1"
        ></v-text-field>


        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color=#59981A
            @click="editAcc"
          >
            create account
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
      show1: false,
      password: '',
      rules: {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters',
      },
      user: this.$route.params.slug,
      f_name: '',
      l_name: '',
      pic: null,
      sts: '',
      data: '',
      file: null,
      username: null,


    }
  },
  methods: {
    uploadFile() {
      this.file = this.$refs.file.files[0];
      console.log(this.file)
      console.log(this.$refs.file.files[0])
    },

    editAcc() {
      const formData = new FormData()
      formData.append("first_name", this.f_name);
      formData.append("username", this.username);
      formData.append("last_name", this.l_name);
      formData.append("password", this.password);
      formData.append('profile_picture', this.file)
      formData.append("status", this.sts);
      this.data = formData
      console.log(this.data)
      this.$axios.$put('http://127.0.0.1:8000/account/' + this.user + '/', this.data)
        .then(response => {
          console.log(response)
          window.alert('account edited')
          window.location.href = "http://127.0.0.1:3000/account/" + response.id ;
        }).catch(response => {
        window.alert('pick a image for your profile')
      })
    }
  }
}
</script>
<style>

</style>

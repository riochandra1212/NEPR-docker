<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-form v-model="valid" ref="form">
          <v-card-title class="headline">
            Login
          </v-card-title>
          <v-card-text>
            <v-alert v-if="error" :type="error.type" dismissible>
              {{ error.message }}
            </v-alert>
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="Email"
              required
            />
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="Password"
              required
              type="password"
            />
          </v-card-text>
          <v-card-actions>
            <nuxt-link to="/">Kembali ke Home</nuxt-link>
            <v-spacer />
            <v-btn
              color="primary"
              nuxt
              @click="validate"
              :loading="isLoading"
            >
              Login
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>

export default {
  auth: 'guest',
  data () {
    return {
      valid: false,
      email: '',
      password: '',
      emailRules: [
        v => !!v || 'Email harus diisi',
        v => /.+@.+/.test(v) || 'Eamil tidak valid'
      ],
      passwordRules: [
        v => !!v || 'Password harus diisi'
      ],
      error: null,
      isLoading: false
    }
  },
  methods: {
    async validate () {
      if (this.$refs.form.validate()) {
        this.isLoading = true
        try {
          await this.$auth.loginWith('local', {
            data: {
              email: this.email,
              password: this.password
            }
          })
          console.log('berhasil login')
          console.log(this.$auth.user);
          this.$router.push('/')
        } catch (error) {
          console.log(error)
          this.error = {
            type: 'error',
            message: error.response.data.error.message ?? "Error tidak diketahui"
          }
          setTimeout(() => {
            this.error = null
          }, 3000);
        }
        this.isLoading = false
      }else{
        console.log('not validate');
      }
    }
  }
}
</script>

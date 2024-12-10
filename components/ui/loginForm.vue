<template>
  <v-card flat elevation="0" width="400" color="#444444">
    <v-card-title>
      <p class="text-center" style="color: #d4af37; width: 100%;">
        I am All of me
      </p>
    </v-card-title>
    <v-card-text>
      <v-row style="width: 100% !important;">
        <v-col cols="4" align-self="center">
          <v-img
            :src="require('@/assets/images/shadowitsmedevil.png')"
            style="width: 80%;"
          />
        </v-col>
        <v-col>
          <form>
            <v-row>
              <v-text-field
                v-model="form.usuario"
                outlined
                dense
              />
            </v-row>
            <v-row>
              <v-text-field
                v-model="form.password"
                type="password"
                outlined
                dense
              />
            </v-row>
            <v-row v-if="errorMessage" class="error">
              {{ errorMessage }}
            </v-row>
          </form>
        </v-col>
      </v-row>
    </v-card-text>
    <v-card-actions>
      <v-row align="center" justify="center" class="ma-3">
        <v-btn color="#d4af37" @click="gotoSignUp">
          <span style="txt-transform: none; color: black;">
            Registrarse
          </span>
        </v-btn>
        <v-btn color="#DA0037" class="ml-3" @click="login">
          <span style="txt-transform: none; color: black;">
            Iniciar Sesion
          </span>
        </v-btn>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data () {
    return {
      form: {
        usuario: '',
        password: ''
      },
      errorMessage: ''
    }
  },
  methods: {
    async login () {
      try {
        const res = await this.$auth.loginWith('local', {
          data: this.form
        })
        console.log('@@ res => ', res)
        if (res && res.data && res.data.token) {
          this.$router.push('/dashboard')
        }
      } catch (error) {
        this.errorMessage = error
      }
    },
    gotoSignUp () {
      this.$router.push('/signup')
    }
  }
}
</script>

<style scoped>
  .error {
    color: blue;
  }
</style>

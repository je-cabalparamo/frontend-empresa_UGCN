<template>
  <v-card
    flat
    elevation="0"
    width="1000"
    color="#444444"
  >
    <v-card-title>
      <p class="text-center" style="color: #d4af37; width: 100% !important;">
        Team Dark Register
      </p>
    </v-card-title>
    <v-card-text>
      <v-form>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.nombre"
              dense
              outlined
              label="Nombre"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.apaterno"
              dense
              outlined
              label="Apellido Paterno"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.amaterno"
              dense
              outlined
              label="Apellido Materno"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.direccion"
              dense
              outlined
              label="Direccion"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.ciudad"
              dense
              outlined
              label="Ciudad"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.estado"
              dense
              outlined
              label="Estado"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.telefono"
              dense
              outlined
              label="Telefono"
            />
          </v-col>
          <v-col cols="4">
            <v-combobox
              v-model="usuario.rol"
              :items="['Admin', 'Compras']"
              dense
              outlined
            />
          </v-col>
          <v-col cols="4">
            <v-file-input
              v-model="usuario.imagen"
              dense
              outlined
              label="Imagen"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.usuario"
              dense
              outlined
              label="Usuario"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="usuario.password"
              type="password"
              dense
              outlined
              label="Password"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="confirmPassword"
              type="password"
              dense
              outlined
              label="Confirmar Password"
            />
          </v-col>
        </v-row>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-row align="center" justify="center" class="ma-3">
        <v-btn color="#d4af37" @click="gotoLogin">
          <span style="txt-transform: none; color: black;">
            Cancelar
          </span>
        </v-btn>
        <v-btn color="#DA0037" class="ml-3" @click="registrarUsuario">
          <span style="txt-transform: none; color: black;">
            {{ inside && update ? 'Actualizar' : inside && !update ? 'Crear Nuevo' : 'Registrar' }}
          </span>
        </v-btn>
      </v-row>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    inside: { type: Boolean, default: false },
    update: { type: Boolean, default: false },
    empleadoUpdate: { type: Object, default: null }
  },
  data () {
    return {
      usuario: {},
      confirmPassword: ''
    }
  },
  mounted () {
    if (this.update && this.empleadoUpdate) {
      this.usuario = this.empleadoUpdate
      this.usuario.password = ''
    }
  },
  methods: {
    gotoLogin () {
      if (this.inside) {
        this.$emit('click-cancel')
      } else {
        this.$router.push('/')
      }
    },
    async registrarUsuario () {
      if (this.usuario.password === this.confirmPassword) {
        const formData = new FormData()
        for (const key in this.usuario) {
          if (key === 'imagen') {
            if (this.usuario.imagen) {
              formData.append(key, this.usuario.imagen[0])
            }
          } else {
            formData.append(key, this.usuario[key])
          }
        }
        let response
        if (this.update) {
          response = await this.$axios.put(`/empleados/update/${this.usuario.id}`, formData)
        } else {
          response = await this.$axios.post('/empleados/create', formData)
        }
        console.log('@@@ response => ', response)
        if (response.data.success) {
          this.usuario = {}
          if (this.inside) {
            if (this.update) {
              this.$store.commit('setType', 'warning')
              this.$store.commit('setColor', 'warning')
              this.$store.commit('setMensaje', 'El usuario fue Actualizado Exitosamente')
              this.$store.commit('setShowAlert', true)
            } else {
              this.$store.commit('setType', 'success')
              this.$store.commit('setColor', 'green')
              this.$store.commit('setMensaje', 'El usuario fue Creado Exitosamente')
              this.$store.commit('setShowAlert', true)
            }
            this.$emit('guardado')
          } else {
            this.$router.push('/')
          }
        } else {
          this.$store.commit('setType', 'error')
          this.$store.commit('setColor', 'red')
          this.$store.commit('setMensaje', 'Dang, error Found')
          this.$store.commit('setShowAlert', true)
        }
      }
    }
  }
}

</script>

<style scoped>
</style>

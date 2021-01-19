<template lang="pug">
div
  h1 Candidatos
  v-data-table(
    :headers="headers"
    :items="candidatos"
  )
    template( v-slot:top )
      v-toolbar( flat )
        v-toolbar-title Candidatos
        v-spacer
        v-dialog( v-model="dialogNuevo" max-width="500px" )
          template( v-slot:activator="{on, attrs}" )
            v-btn(@click="agregarCandidato()" color="primary" v-bind="attrs" v-on="on") Nuevo
          v-card
            v-card-text
              v-container
                v-row
                  v-col(class="d-flex flex-column")
                    v-text-field(v-model="nuevoCandidato.nombre" label ="Nombre")
                    v-text-field(v-model.number="nuevoCandidato.telefono" label="Número" type="number" )
                    v-text-field(v-model="nuevoCandidato.comentarios" label="Comentarios")
            v-card-actions
              v-spacer
              v-btn(@click="() => dialogNuevo = false" color="primary") Cerrar
              v-btn(@click="() => dialogNuevo = false" color="primary") Agregar
    template(v-slot:item.acciones="{ item }")
      v-btn(@click="eliminar" color="primary")
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data() {
    return {
      nuevoCandidato: {},
      headers: [
        {
          text: 'Nombre',
          value: 'nombre'
        },
        {
          text: 'Teléfono',
          value: 'telefono'
        },
        {
          text: 'Comentarios',
          value: 'comentarios'
        }
      ],
      candidatos: [],
      dialogNuevo: false
    }
  },
  mounted() {
    this.getCandidatos()
  },
  methods: {
    agregarCandidato() {

    },
    getCandidatos() {
      console.log(this.$fire)
    }
  }
}
</script>

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

<script lang="ts">
import Vue from 'vue'

interface Candidato {
  name: string,
  vacant: string,
  call_datetime?: string,
  appointment_datetime?: string,
  evaluation_score: string,
  annotations: string
}

interface CandidatoCliente {
  name: string,
  vacant: string,
  call_date: string,
  call_time: string,
  appointment_date: string,
  appointment_time: string,
  evaluation_score: string,
  annotations: string
}

export default Vue.extend({
  data() {
    return {
      nuevoCandidato: {},
      headers: [
        {
          text: 'Nombre',
          value: 'name'
        },
        {
          text: 'Vacante',
          value: 'vacant'
        },
        {
          text: 'Fecha de llamada',
          value: 'call_date'
        },
        {
          text: 'Hora de llamada',
          value: 'call_time'
        },
        {
          text: 'Fecha de cita',
          value: 'appointment_date'
        },
        {
          text: 'Hora de cita',
          value: 'appointment_time'
        },
        {
          text: 'Puntaje de evaluación',
          value: 'evaluation_score'
        },
        {
          text: 'Anotaciones',
          value: 'annotations'
        }
      ],
      candidatos: [] as any,
      dialogNuevo: false
    }
  },
  mounted() {
    this.getCandidates()
  },
  methods: {
    agregarCandidato() {

    },
    async getCandidates() {
      let candidatos = (await this.$fire.firestore.collection('candidatos_gibran').get()).docs.map(doc => doc.data())
      console.log( this.candidatos.map( (candidato: any) => {
        return {
          name: candidato.name,
          vacant: candidato.vacant,
          call_date: this.$moment(candidato.call_datetime).format('LL'),
          call_time: this.$moment(candidato.call_datetime).format('h:mm A'),
          appointment_date: this.$moment(candidato.appointment_datetime).format('LL'),
          appointment_time: this.$moment(candidato.appointment_datetime).format('h:mm A'),
          evaluation_score: candidato.evaluation_score,
          annotations: candidato.annotations
        }
      }))
    }
  }  
})
</script>

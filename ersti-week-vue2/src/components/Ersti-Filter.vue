<script>
export default {
  emits: ['update:selectedDegree','update:ukrainian', 'update:flinta', 'update:international'],
  data: () => ({
    degrees: ['bachelor', 'master'],
    b_courses: ['Wirtschaftsinformatik', 'Elektrotechnik', 'Technische Informatik', 'Automotive Systems'],
    m_courses: ['Computer Science'],
    internalSelectedDegree: null,
    selected_course: null,
    internalUkrainian: true,
    internalFlinta: true,
    internalInternational: true,
  }),
  props: {
    selected_degree: {
      type: String,
      default: null,
      validator: function(value){
        return ['bachelor', 'master', null].includes(value);
      }
    },
    ukrainian: {
      type: Boolean,
      default: true
    },
    flinta: {
      type: Boolean,
      default: true
    },
    international: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    courses() {
      return this.selected_degree === 'Bachelor' ? this.b_courses : this.m_courses;
    },
  },
  watch: {
      selectedDegree(newVal){
        this.internalSelectedDegree = newVal;
      },
    ukrainian(newVal){
      this.internalUkrainian = newVal;
    },
    flinta(newVal){
      this.internalFlinta = newVal;
    },
    international(newVal){
      this.internalInternational = newVal;
    }
  },
  methods: {
    handleDegreeChange(){
      this.$emit('update:selectedDegree', this.internalSelectedDegree)
    },
    handleUkrainianChange(){
      this.$emit('update:ukrainian', this.internalUkrainian);
    },
    handleFlintaChange(){
      this.$emit('update:flinta', this.internalFlinta);
    },
    handleInternationalChange(){
      this.$emit('update:international', this.internalInternational);
    }
  }

}
</script>

<template>

    <v-container fluid>
      <v-select
          v-model="internalSelectedDegree"
          v-on:change="handleDegreeChange"
          :items="degrees"
          label="Angestrebter Studienabschluss"

      ></v-select>
      <v-switch
          v-model="internalUkrainian"
          @change="handleUkrainianChange"
          color="#0057B7"
          :label="`Ukrainian`"
      ></v-switch>
      <v-switch
          v-model="internalFlinta"
          @change="handleFlintaChange"
          color="green"
          :label="`Flinta`"
      ></v-switch>
      <v-switch
          v-model="internalInternational"
          @change="handleInternationalChange"
          color="purple"
          :label="`International`"
      ></v-switch>
    </v-container>


</template>

<style scoped>

</style>
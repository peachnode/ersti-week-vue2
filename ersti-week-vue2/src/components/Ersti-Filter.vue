<script>

export default {
  emits: ['update:selectedDegree', 'update:selectedCourse','update:ukrainian', 'update:flinta', 'update:international'],
  data: () => ({
    degrees: ['bachelor', 'master'],
    internalSelectedDegree: null,
    internalSelectedCourse: null,
    internalUkrainian: null,
    internalFlinta: null,
    internalInternational: null,
  }),
  props: {
    coursesDict: {
      type: Object
    }, //TODO add more
    selected_degree: {
      type: String,
      default: null,
      validator: function(value){
        return ['bachelor', 'master', null].includes(value);
      }
    },
    selected_course: { //TODO add validator
      type: String,
      default: null,
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
      let degreePrefix = this.internalSelectedDegree === 'bachelor' ? 'b_' : 'm_';
      let keys = Object.keys(this.coursesDict).filter(key => key.startsWith(degreePrefix));
      return keys.map(key => {
        return { text: this.coursesDict[key], value: key };
      });
    },
  },

  watch: {
      selectedDegree(newVal){
        this.internalSelectedDegree = newVal;
      },
    selectedCourse(newVal){
      this.internalSelectedCourse = newVal;
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
      this.$emit('update:selectedDegree', this.internalSelectedDegree);
      this.internalSelectedCourse = null; // Set the selected course to null
      this.$emit('update:selectedCourse', null); // Emit the course change
    },
    handleCourseChange(){
      this.$emit('update:selectedCourse', this.internalSelectedCourse)
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
  },
  created() {
    this.internalSelectedDegree = this.selected_degree;
    this.internalSelectedCourse = this.selected_course;
    this.internalUkrainian = this.ukrainian;
    this.internalFlinta = this.flinta;
    this.internalInternational = this.international;
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
      <v-select
          v-if="selected_degree"
          :items="courses"
          v-model="internalSelectedCourse"
          v-on:change="handleCourseChange"
          item-text="text"
          item-value="value"
          label="Studiengang"
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
<script>
export default {
  data: () => ({
    focus: '',
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    events: [
      {
        name: "Begrüßung Bachelor & How TU Studium",
        start: "2022-10-06T10:00:00",
        end: "2022-10-06T12:00:00",
        color: "red",
        details: "Hybrid, H0104 /n https://tubcloud.tu-berlin.de/s/A8bTb8iTdptgywS"
      },
      {
        name: "Begrüßung Master & How TU Studium",
        start: "2022-10-06T13:00:00",
        end: "2022-10-06T15:00:00",
        color: "orange",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführungsveranstaltung für internationale Studierende",
        start: "2022-10-06T08:00:00",
        end: "2022-10-06T18:00:00",
        color: "purple",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Wirtschaftsinformatik",
        start: "2022-10-07T11:00:00",
        end: "2022-10-07T12:00:00",
        color: "red",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Elektrotechnik",
        start: "2022-10-07T11:15:00",
        end: "2022-10-07T12:15:00",
        color: "red",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Technische Informatik",
        start: "2022-10-07T14:00:00",
        end: "2022-10-07T15:00:00",
        color: "red",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Automotive Systems",
        start: "2022-10-07T10:00:00",
        end: "2022-10-07T11:00:00",
        color: "orange",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Elektrotechnik",
        start: "2022-10-07T10:00:00",
        end: "2022-10-07T11:00:00",
        color: "orange",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung ICT Innovation",
        start: "2022-10-07T10:00:00",
        end: "2022-10-07T12:00:00",
        color: "orange",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführung Computer Engineering",
        start: "2022-10-07T15:00:00",
        end: "2022-10-07T16:00:00",
        color: "orange",
        details: "Hybrid, H0104"
      },
      {
        name: "Einführungsveranstaltung für internationale Studierende",
        start: "2022-10-07T08:00:00",
        end: "2022-10-07T18:00:00",
        color: "purple",
        details: "Hybrid, H0104"
      },
      {
        name: "Уні в Німеччині vs. Уні в Україні - Uni in Deutschland vs Uni in der Ukraine ",
        start: "2022-10-08T11:00:00",
        end: "2022-10-08T13:00:00",
        color: "#0057B7",
      },
      {
        name: "Schnitzeljagd durch Berlin",
        start: "2022-10-09T14:00:00",
        end: "2022-10-09T17:00:00",
        color: "blue",
      },
    ]
  }),
  mounted () {
    this.$refs.calendar.checkChange()
  },
  methods: {
    viewDay ({ date }) {
      this.focus = date
      this.type = 'day'
    },
    getEventColor (event) {
      return event.color
    },
    setToday () {
      this.focus = ''
    },
    prev () {
      this.$refs.calendar.prev()
    },
    next () {
      this.$refs.calendar.next()
    },
    showEvent ({ nativeEvent, event }) {
      const open = () => {
        this.selectedEvent = event
        this.selectedElement = nativeEvent.target
        requestAnimationFrame(() => requestAnimationFrame(() => this.selectedOpen = true))
      }

      if (this.selectedOpen) {
        this.selectedOpen = false
        requestAnimationFrame(() => requestAnimationFrame(() => open()))
      } else {
        open()
      }

      nativeEvent.stopPropagation()
    }
  },
}
</script>

<template>

    <v-row class="fill-height">
      <v-col>
        <v-sheet height="64">
          <v-toolbar
              flat
          >
            <v-btn
                fab
                text
                small
                color="grey darken-2"
                @click="prev"
            >
              <v-icon small>
                mdi-chevron-left
              </v-icon>
            </v-btn>
            <v-btn
                fab
                text
                small
                color="grey darken-2"
                @click="next"
            >
              <v-icon small>
                mdi-chevron-right
              </v-icon>
            </v-btn>
            <v-toolbar-title v-if="$refs.calendar">
              {{ $refs.calendar.title }}
            </v-toolbar-title>
            </v-toolbar>
        </v-sheet>
        <v-sheet height="600">
          <v-calendar
              ref="calendar"
              v-model="focus"
              color="primary"
              :events="events"
              :event-color="getEventColor"
              type="week"
              @click:event="showEvent"
              @click:more="viewDay"
              @click:date="viewDay"
              start="2022-10-06"
              first-time="7"
              interval-count="14"
          ></v-calendar>
          <v-menu
              v-model="selectedOpen"
              :close-on-content-click="false"
              :activator="selectedElement"
              offset-x
          >
            <v-card
                color="grey lighten-4"
                min-width="350px"
                flat
            >
              <v-toolbar
                  :color="selectedEvent.color"
                  dark
              >

                <v-toolbar-title v-html="selectedEvent.name"></v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon>
                  <v-icon>mdi-heart</v-icon>
                </v-btn>
              </v-toolbar>
              <v-card-text>
                <span v-html="selectedEvent.details"></span>
              </v-card-text>
              <v-card-actions>
                <v-btn
                    text
                    color="secondary"
                    @click="selectedOpen = false"
                >
                  Cancel
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-menu>
        </v-sheet>
      </v-col>
    </v-row>

</template>

<style scoped>

</style>
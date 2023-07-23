<script>
import events from "../assets/events.json"

export default {
  data: () => ({
    focus: '',
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    events: events,
     }),
  mounted () {
    this.$refs.calendar.checkChange()
  },
  computed: {
    filteredEvents(){
      return this.events.filter(event =>
          !(this.international && event.parallel) &&
          (!event.international || this.international) &&
          (this.degree === event.degree || !this.degree || !event.degree) &&
          (!event.flinta || this.flinta) &&
          (!event.ukrainian || this.ukrainian) &&
          (this.course === event.course || !this.course || !event.course));
    }
  },

  props: {

    degree: {
      type: String,
      default: null
    },
    course: {
      type:String,
      default: null
    },
    flinta: {
      type: Boolean,
      default: false
    },
    ukrainian: {
      type: Boolean,
      default: true
    },
    international: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    viewDay ({ date }) {
      this.focus = date
      this.type = 'day'
    },
    getEventColor(event) {
        return event.color; // return the normal color if not international or not parallel

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
              :events="filteredEvents"
              :event-color="getEventColor"
              type="week"
              @click:event="showEvent"
              @click:more="viewDay"
              @click:date="viewDay"
              start="2022-10-06"
              first-time="7"
              interval-count="14"
          ><template v-slot:event="{ event }">
            <div v-if="event.ukrainian" class="ukr-event">
              {{ event.name }}
            </div>
            <div v-else>
              {{ event.name }}
            </div>
          </template></v-calendar>
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
.ukr-event {
  background: #0057B7;
  color: white;
  border-radius: 4px;
  padding: 2px;
}
</style>


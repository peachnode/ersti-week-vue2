<script>
import events from "../assets/events.json"

export default {
  data: () => ({
    focus: '',
    selectedEvent: {},
    selectedElement: null,
    selectedOpen: false,
    events: events,
    colorNameToHex: {
      'black': '#000000',
      'white': '#FFFFFF',
      'red': '#FF0000',
      'lime': '#00FF00',
      'blue': '#0000FF',
      'yellow': '#FFFF00',
      'cyan': '#00FFFF',
      'magenta': '#FF00FF',
      'silver': '#C0C0C0',
      'gray': '#808080',
      'maroon': '#800000',
      'olive': '#808000',
      'green': '#008000',
      'purple': '#800080',
      'teal': '#008080',
      'navy': '#000080',
      'orange': '#FFA500',
    },
  }),
  mounted () {
    this.$refs.calendar.checkChange()
  },
  computed: {
    filteredEvents(){
      return this.events.filter(event =>
          (!event.international || this.international) &&
          (this.degree === event.degree || !this.degree || !event.degree) &&
          (!event.flinta || this.flinta) &&
          (!event.ukrainian || this.ukrainian));
    }
  },
  props: {
    degree: {
      type: String,
      default: null,
      validator: function(value){
        return ['bachelor', 'master', null].includes(value);
      }
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
    rgbaColor(color, alpha) {
      // If color is a name, convert it to hex
      if (!color.startsWith('#')) {
        color = this.colorNameToHex[color.toLowerCase()] || '#000000'; // Default to black if color name is not found
      }

      color = color.slice(1); // remove '#' from the start of the color

      const r = parseInt(color.slice(0, 2), 16);
      const g = parseInt(color.slice(2, 4), 16);
      const b = parseInt(color.slice(4, 6), 16);

      return `rgba(${r},${g},${b},${alpha})`;
    },
    getEventColor(event) {
      if(this.international && event.parallel) {
        return this.rgbaColor(event.color, 0.2); // return a semi-transparent version of the event color
      } else {
        return event.color; // return the normal color if not international or not parallel
      }
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

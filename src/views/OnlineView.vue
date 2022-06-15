<template>
<v-app>
  <v-main>
    <head>
      <title>Home</title>
    </head>
    <template>
      <div>
    <p class="subtitle_text" align="center">Select the type of events you are looking for</p>
    <div class="text-center">
    <v-menu
      open-on-hover
      top
      offset-y
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="teal"
          dark
          v-bind="attrs"
          v-on="on"
        >
          FILTER
        </v-btn>
      </template>

      <v-list>
        <v-list-item>
          <v-list-item-title><a href="/online_events" style="text-decoration: none;">Online Events</a></v-list-item-title>
        </v-list-item>
        <v-list-item>
          <v-list-item-title><a href="/presential_events" style="text-decoration: none;">Presential Events</a></v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
        <v-container>
         <v-row justify="center">
          <v-col cols="4" v-for="event in events"
            :key="event.id"
          >
        <myEvent
          :event="event"
        />
      </v-col>
    </v-row>
    </v-container>
    <v-footer
    color="teal"
    padless
  >
    <v-row
      justify="center"
      no-gutters
    >
      <v-btn
        v-for="link in links"
        :key="link"
        color="white"
        text
        rounded
        class="my-2"
      >
        {{ link }}
      </v-btn>
      <v-col
        class="primary lighten-2 py-4 text-center white--text"
        cols="12"
      >
        {{ new Date().getFullYear() }} — <strong>Iker Pacheco</strong>
      </v-col>
    </v-row>
  </v-footer>
      </div>
    </template>
  </v-main>
</v-app>
</template>

<script>
import myEvent from '../components/myEventOnline'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    myEvent
  },
  data () {
    return {
      events: [],
      loading: false,
      items: [],
      search: null,
      select: null,
      states: ["Madrid", "San francisco"],
      displayed: []
    }
  },
  watch: {
    search (val) {
      val && val !== this.select && this.querySelections(val)
    },
    filterCity(newVal, oldVal) {
      console.log(oldVal, newVal);
      this.filterCities(newVal);
    }
  },
  methods: {
    filterCities(val) {
      this.displayed = this.items.filter((item) => item.title.includes(val));
    },
    querySelections (v) {
      this.loading = true
      // Simulated ajax query
      setTimeout(() => {
        this.items = this.states.filter(e => {
          return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
        })
        this.loading = false
      }, 500)
    },
  },

  async created() {
    axios.defaults.headers.common['Authorization'] = `Bearer ${"2T26GZVOEBDSFGA32Z2M"}`
    const result = await axios.get("https://www.eventbriteapi.com/v3/organizations/1006260659593/events/?expand=venue");
    this.events = result.data.events;
    localStorage.setItem("events", JSON.stringify(this.events));
    console.log(this.events)
    this.displayed = this.states;
  }
}
</script>
<style>

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'poppins', sans-serif;
  }

  .subtitle_text {
    margin-bottom: -30px;
    margin-top: 20px;
  }
</style>


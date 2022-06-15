<template>
<v-app>
  <body>
  <v-container>
    <v-img height="500" :src="eventView.logo.url"></v-img>
  <v-main>
  <div class="event">
    <h1 class="event_name" align="center">{{eventView.name.text}}</h1><hr/>
    <h2 class="event_section_title" align="left">Event description:</h2>
    <p class="event_description" align="left">{{eventView.description.text}}</p>
    <h2 class="event_section_title" align="left">Address:</h2>
    <p class="event_description" align="left">{{eventView.venue.address.address_1}}</p>
    <h2 class="event_section_title" align="left">Starting date and ending date:</h2>
    <p class="event_description" align="left">The event starts the day {{start_date}} at {{start_hour}} and finishes the day {{end_date}} at {{end_hour}}</p>

    <v-btn 
      class="book_button"
      @click="ShareData"
      :to="`/event/${eventView.id}/tickets`"
      rounded
      color="primary"
      dark
    >
      Buy Tickets
    </v-btn>
    <v-btn align="left"><a href="../../" style="text-decoration: none">RETURN</a></v-btn>
  </div>
  </v-main>
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
  </body>
</v-app>
</template>

<script>
export default {
  data: () => ({
    eventView: {},
    name:"eventView",
    start_date: "",
    end_date: "",
    start_hour: "",
    end_hour: ""
  }),
  methods: {
    async get_hours(event) {
      let str1 = event.start.local.split('T');
      let str2 = event.end.local.split('T');

      this.start_date = str1[0];
      this.end_date = str2[0];
      this.start_hour = str1[1];
      this.end_hour = str2[1];
    },
  },
  async created() {
    const result = JSON.parse(localStorage.getItem("events"));
    console.log(result);
    this.eventView = result.find(event => event.id == this.$route.params.id)
    this.get_hours(this.eventView);
  }
}
</script>
<style>

  body {
    font-family: 'montseratt', sans-serif;
  }

  .event_name
  {
    margin: 50px 10px;
    font-size: 65px;
  }
  .event_section_title
  {
    margin: 40px 10px;
    font-size: 30px;
  }
  .event_description
  {
    margin: 10px 10px;
    font-size: 20px;
  }
  .book_button
  {
    margin: 30px 10px;
  }

  main {
    width: 100vw;
    min-height: 20vh;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
</style>
<template>
<v-app>
  <v-main>
  <v-body class="tickets">
    <v-container>
      <v-rows>
        <v-cols cols="2">
    <v-hover v-slot="{ hover }">
          <v-card
            class="mx-auto"
            color="teal"
            max-width="600"
          >
            <v-img
              :aspect-ratio="16/9"
              src="https://st2.depositphotos.com/4215343/6595/i/950/depositphotos_65955407-stock-photo-green-tickets.jpg"
            >
              <v-expand-transition>
                <div
                  v-if="hover"
                  class="d-flex transition-fast-in-fast-out white darken-2 v-card--reveal text-h2 teal--text"
                  style="height: 100%;"
                >
                <strong>{{ticketCost[0].cost.major_value}} {{ticketCost[0].cost.currency}}</strong>
                </div>
              </v-expand-transition>
            </v-img>
            <v-card-text
              class="pt-6"
              style="position: relative;"
            >
              <v-btn
                absolute
                color="teal"
                class="white--text"
                fab
                large
                right
                top
              >
                <v-icon>mdi-cart</v-icon>
              </v-btn>
              <div class="font-weight-light white--text text-h6 mb-2">
                PAID ENTRY - {{ticketCost[0].on_sale_status}} until {{end_sale}} at {{end_hour}}
              </div>
              <h3 class="text-h4 font-weight-light white--text mb-2">
                <strong>{{ticketCost[0].display_name}}</strong>
              </h3>
              <div class="font-weight-light text-h6 mb-2">
                {{ticketCost[0].description}}
              </div>
            </v-card-text>
          </v-card>
        </v-hover>
        <v-hover v-slot="{ hover }">
          <v-card
            class="mx-auto"
            color="teal"
            max-width="600"
          >
            <v-img
              :aspect-ratio="16/9"
              src="https://mpng.subpng.com/20181214/akz/kisspng-event-tickets-clip-art-vector-graphics-royalty-fre-download-clip-art-royalty-free-stock-you-could-win-5c13aad241e927.78403279154479278627.jpg"
            >
              <v-expand-transition>
                <div
                  v-if="hover"
                  class="d-flex transition-fast-in-fast-out white darken-2 v-card--reveal text-h2 teal--text"
                  style="height: 100%;"
                >
                <strong>FREE</strong>
                </div>
              </v-expand-transition>
            </v-img>
            <v-card-text
              class="pt-6"
              style="position: relative;"
            >
              <v-btn
                absolute
                color="teal"
                class="white--text"
                fab
                large
                right
                top
              >
                <v-icon>mdi-cart</v-icon>
              </v-btn>
              <div class="font-weight-light white--text text-h6 mb-2">
                FREE ENTRY - {{ticketCost[1].on_sale_status}} until {{end_sale}} at {{end_hour}}
              </div>
              <h3 class="text-h4 font-weight-light white--text mb-2">
                <strong>{{ticketCost[1].display_name}}</strong>
              </h3>
              <div class="font-weight-light text-h6 mb-2">
                {{ticketCost[1].description}}
              </div>
            </v-card-text>
          </v-card>
        </v-hover>
        </v-cols>
      </v-rows>
      </v-container>
  </v-body>
    <v-btn to="./">Return</v-btn>
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
  </v-main>
</v-app>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    ticketView: {},
    ticketCost: {},
    end_sale: "",
    end_hour: ""
  }),
  methods: {
  },
  async created() {
    axios.defaults.headers.common['Authorization'] = `Bearer ${"2T26GZVOEBDSFGA32Z2M"}`
    const tickets = await axios.get("https://www.eventbriteapi.com/v3/events/" + this.$route.params.id + "/ticket_classes/");
    this.ticketCost = tickets.data.ticket_classes;
    console.log(this.ticketCost);
    this.ticketView = this.$route.params.data;
    console.log(this.ticketView);

    let array = this.ticketCost[0].sales_end.split('T');
    this.end_sale = array[0];
    this.end_hour = array[1].slice(0, 8);
  }
}
</script>
<style>

  body {
    font-family: 'montseratt', sans-serif;
  }

  main {
    width: 100vw;
    min-height: 100vh;
    overflow: hidden;
    background-color: #EEE;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .tickets {
    display:flex;
    max-width: 1280px;
    padding: 25px;
    margin: 0 auto;
  }
  .v-card--reveal {
  color: teal;
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: .7;
  position: absolute;
  width: 100%;
  }
  
</style>

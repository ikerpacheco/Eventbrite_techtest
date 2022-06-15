<template>
  <v-hover>
    <template v-slot:default="{ hover }">
  <v-card v-if="event.online_event === true"
    :loading="loading"
    class="mx-auto my-12"
    max-width="600"
  >
    <template slot="progress">
      <v-progress-linear
        color="deep-purple"
        height="10"
        indeterminate
      ></v-progress-linear>
    </template>

    <v-img
      height="250"
      :src="event.logo.url"
    ></v-img>

    <v-card-title>{{event.name.text}}</v-card-title>

    <v-card-text>
      <v-row
        align="center"
        class="mx-0"
      >
        <v-rating
          :value="5"
          color="amber"
          dense
          half-increments
          readonly
          size="14"
        ></v-rating>

        <div class="grey--text ms-4">
        </div>
      </v-row>

      <div class="my-4 text-subtitle-1">
        {{event.venue.address.address_1}}
      </div>

      <div>{{event.description.text}}</div>
    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    <v-card-title>Date and event duration</v-card-title>

    <v-card-text>
      <v-chip-group
        v-model="selection"
        active-class="deep-purple accent-4 white--text"
        column
      >
        <v-chip>{{start_date}} - {{start_hour}} / {{end_date}} - {{end_hour}}</v-chip>
      </v-chip-group>
    </v-card-text>
    <!-- <v-card-actions>
      <v-btn
        color="deep-purple lighten-2"
        text
        @click="reserve"
      >
        <a href="http://localhost:8080/event" style="text-decoration: none">Reserve</a>
      </v-btn>
    </v-card-actions> -->
    <v-fade-transition>
      <v-overlay
        v-if="hover"
        absolute
        color="#036358"
      >
        <v-btn
          @click="reserve"
        >
        <router-link :to="`/online_events/event/${event.id}`">See more info</router-link>
        </v-btn>
      </v-overlay>
    </v-fade-transition>
  </v-card></template>
  </v-hover>
</template>

<script>
export default {
  props: {
      event: Object
  },
  data: () => ({
      loading: false,
      selection: 1,
      overlay: false,
      start_date: "",
      end_date: "",
      start_hour: "",
      end_hour: ""
    }),
    methods: {
      reserve () {
        this.loading = true

        setTimeout(() => (this.loading = false), 2000)
        this.ShareData()
      },

      async get_hours(event) {
      let str1 = event.start.local.split('T');
      let str2 = event.end.local.split('T');

      this.start_date = str1[0];
      this.end_date = str2[0];
      this.start_hour = str1[1];
      this.end_hour = str2[1];
      },
      ShareData () {
        this.$router.push({name:"event", params:{data:this.event}})
      }
    },

    async created() {
      this.get_hours(this.event);
    }
}
</script>
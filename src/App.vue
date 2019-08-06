<template>
  <div id="app">
    <calendar-view
      :show-date="showDate"
      :events="events"
      class="theme-default holiday-us-traditional holiday-us-official"
    >
      <calendar-view-header
        slot="header"
        slot-scope="t"
        :header-props="t.headerProps"
        @input="setShowDate"
      />
    </calendar-view>
  </div>
</template>

<script>
import axios from 'axios'
import { CalendarView, CalendarViewHeader } from "vue-simple-calendar";
// The next two lines are processed by webpack. If you're using the component without webpack compilation,
// you should just create <link> elements for these. Both are optional, you can create your own theme if you prefer.
require("vue-simple-calendar/static/css/default.css");
require("vue-simple-calendar/static/css/holidays-us.css");

export default {
  name: "app",
  data: function() {
    return {
      showDate: new Date(),
      events: [
				{
          startDate: "2019-08-06",
          endDate: "2019-08-06",
          title: "GSW Philadelphia",
          classes: "gsw"
        },
        {
          startDate: "2019-01-01",
          endDate: "2019-08-22",
          title: "RADICAVA HCP Overhaul Initial Dev",
          classes: "radicava"
        },
        {
          startDate: "2019-01-01",
          endDate: "2019-08-22",
          title: "XPD Patient Overhaul Initial Dev",
          classes: "xpd"
        },
        {
          startDate: "2019-01-01",
          endDate: "2019-08-22",
          title: "Stendra Consumer Landing Page Initial Dev",
          classes: "stendra"
        },
        {
          startDate: "2019-08-17",
          endDate: "2019-08-22",
          title: "Investor Relations (WP) Initial Dev",
          classes: "stendra"
        },
      ]
    }
  },
  components: {
    CalendarView,
    CalendarViewHeader
  },
  methods: {
    setShowDate(d) {
      this.showDate = d;
    }
  },
  mounted () {
    axios
      .get('https://sheetsu.com/apis/v1.0su/ac2b4327279a')
      .then(response => (
        console.log(response.data)
      ))
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #2c3e50;
  height: 95vh;
  width: 90vw;
  margin-left: auto;
  margin-right: auto;
}

.cv-event.gsw {
  background-color: yellow;
  color: black;
}

.cv-event.radicava {
  background-color: blue;
  color: black;
}

.cv-event.xpd {
  background-color: orange;
  color: black;
}

.cv-event.stendra {
  background-color: teal;
  color: black;
}
</style>

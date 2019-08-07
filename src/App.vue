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
      datafeed: null,
      events: []
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
  watch: {
    datafeed: function (data) {
      /* Set a watcher on the raw data-feed that lets you
       * process it as needed into its components.
       */
      this.events = data.reduce((acc, item) => {
        const event = {}
        event.startDate = item.inital_dev_start
        event.endDate = item.initial_dev_end
        event.title = item.brand + item.project
        event.classes = item.brand.toLowerCase()
        /* If we don't have a start date, we skip this one.
         * This can be expanded to make a more robust check.
         */
        if (event.startDate) {
          acc.push(event)
        }
        return acc
      }, [])
    }
  },
  mounted () {
    axios
      .get('https://sheetsu.com/apis/v1.0su/ac2b4327279a')
      .then(response => {
        /* Simplified data request and stored the whole thing,
         * unaltered. You might want to use it for some other
         * purpose later in addition to the events subset.
         */
        this.datafeed = response.data
      })
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

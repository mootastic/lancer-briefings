<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Mission Log</h1>
      </div>
      <div class="section-content-container">
        <h3>Current Assignment</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Mission List</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';

export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },

  data() {
    return {
      "mission_slug": "001",
      "current_md": "",
      "events": "",
      "missions": [
        {
          "slug": "001",
          "name": "Operation: Solstice Rain",
          "status": "start"
        },
        /*
        {
        "slug": "002",
        "name": "Operation: Daybreak",
        "status": "start"
        }
*/
      ],
      "pilots": [
        {
          "callsign": "Cheshire",
          "alias": "Aeva",
          "code": "PILOT/IDref/3e584dd2-9f2b-4473-85c9-414647f3798c",
          "corpro": "GMS",
          "frame": "CHOMOLUNGMA",
          "mech": "Ethereal and Unquestionable"
        },
        {
          "callsign": "Hammer",
          "alias": "Tohrel Jones",
          "code": "PILOT/IDref/99bdb35e-f826-4a22-af57-d00fb59c1362",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Yeti"
        },
        /*{
          "callsign": "El Diablo",
          "alias": "Ricardo Roberts",
          "code": "PILOT/IDref/a895c370-9978-419d-8e85-c944dc755e4f",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Whole-Ass One Thing"
        }, */
        {
          "callsign": "Buzzard",
          "alias": "Anton Madler",
          "code": "PILOT/IDref/4a67acd5-5ee3-4e5a-bb44-8d8480b26003",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Grey Area"
        },
        {
          "callsign": "Ambassassin",
          "alias": "Dax Morrow",
          "code": "PILOT/IDref/3bddbb10-cf0d-4d9b-b37d-5809be68be91",
          "corpro": "GMS",
          "frame": "Everest",
          "mech": "Mister Misunderstanding"
        }
      ],
      "header": {
        "planet": "Nov Elysia",
        "year": "5016u",
        "system": "Cressidium",
        "gate": "Rainier",
        "ring": "Cascade Line",
        "headerTitle": "Union Naval Department",
        "headerSubtitle": "",
        "subheaderTitle": "UNS-CV Rio Grande ",
        "subheaderSubtitle": "Strike Group Delta",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },

  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },

  computed: {

  },

  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";

      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }

      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }

}
</script>


<style lang="scss">
#app {
  width: 1902px;
  height: 910px;
  overflow: hidden;
}
</style>

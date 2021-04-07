<template>
  <div>
    <b-container>
      <h1>Redmind Code test</h1>
      <h4>My Willander</h4>
      <div class="m-3">
        <input type="text" id="textarea" placeholder="Search for a character" />
        <b-button
          variant="warning"
          type="button"
          v-on:click="searchfunction()"
          class="ml-2"
          >Search</b-button
        >
      </div>
      <hr />

      <div v-if="found.length != 0">
        <div v-for="info in found" v-bind:key="info">
          <div class="accordion" role="tablist">
            <b-card no-body class="mb-1">
              <b-card-header header-tag="header" class="p-1" role="tab">
                <b-button
                  block
                  v-b-toggle="'accordion-' + info.url"
                  variant="dark"
                  >{{ info.name }}</b-button
                >
              </b-card-header>
              <b-collapse
                v-bind:id="'accordion-' + info.url"
                accordion="my-accordion"
                role="tabpanel"
              >
                <b-card-body>
                  <b-card-text>
                    <ul>
                      <li>Height: {{ info.height }}</li>
                      <li>Mass: {{ info.mass }}</li>
                      <li>Hair color: {{ info.hair_color }}</li>
                      <li>Eye color: {{ info.skin_color }}</li>
                      <li>Birth Year: {{ info.birth_year }}</li>
                      <li>Gender: {{ info.gender }}</li>
                    </ul>
                  </b-card-text>
                </b-card-body>
              </b-collapse>
            </b-card>
          </div>
        </div>
      </div>
    </b-container>
  </div>
</template>

  

<script>
import axios from "axios";
//GET all pages of people from SWAPI
let one = "https://swapi.dev/api/people/?page=1";
let two = "https://swapi.dev/api/people/?page=2";
let three = "https://swapi.dev/api/people/?page=3";
let four = "https://swapi.dev/api/people/?page=4";
let five = "https://swapi.dev/api/people/?page=5";
let six = "https://swapi.dev/api/people/?page=6";
let seven = "https://swapi.dev/api/people/?page=7";
let eight = "https://swapi.dev/api/people/?page=8";
let nine = "https://swapi.dev/api/people/?page=9";

const requestOne = axios.get(one);
const requestTwo = axios.get(two);
const requestThree = axios.get(three);
const requestFour = axios.get(four);
const requestFive = axios.get(five);
const requestSix = axios.get(six);
const requestSeven = axios.get(seven);
const requestEight = axios.get(eight);
const requestNine = axios.get(nine);

export default {
  name: "Start",
  data() {
    //create two arrays swpeople which includes all characters and found which is dependent on search
    return { SWpeople: [], found: [] };
  },
  mounted() {
    //runs the Get requests on load
    this.getSW();
  },
  methods: {
    //Gets all pages and combines into SWpeople
    getSW() {
      axios
        .all([
          requestOne,
          requestTwo,
          requestThree,
          requestFour,
          requestFive,
          requestSix,
          requestSeven,
          requestEight,
          requestNine,
        ])
        .then(
          axios.spread((...responses) => {
            for (var i = 1; i < 9; i++) {
              responses[0].data.results = responses[0].data.results.concat(
                responses[i].data.results
              );
            }
            this.SWpeople = responses[0].data.results;
            //initially want to display all characters
            this.found = this.SWpeople;
          })
        );
    },

    searchfunction() {
      var x = document.getElementById("textarea").value.toLowerCase();
      //empty found
      this.found = [];
      //checks for each entry in SWpeople if the textarea is a substring of the name
      for (var i = 0; i < this.SWpeople.length; i++) {
        if (this.SWpeople[i].name.toLowerCase().includes(x)) {
          this.found = this.found.concat(this.SWpeople[i]);
        }
      }
    },
  },
};
</script>

<style scoped>
.accordion {
  width: 20em;
  margin: auto;
}
</style>

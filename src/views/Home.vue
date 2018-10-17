<template>
  <div class="home">

    <h4>Total number of people: {{ people.length }}</h4>

    <div>
      Name: <input v-model="newPerson.name">
      Bio: <input v-model="newPerson.bio">
      <button v-on:click="addPerson()">Add Person</button>
    </div>

    <div v-for="person in people">
      <h2 v-on:click="toggleBioVisible(person)">{{ person.name }}</h2> 
      <h3 v-bind:class="{strike: !person.bioVisible}">{{ person.bio }}</h3>
      <button v-on:click="deletePerson(person)">Delete</button>
    </div>

  </div>
</template>

<style>
  .strike {
    text-decoration: line-through;
  }
</style>

<script>
var axios = require('axios');
export default {
  data: function() {
    return {
      people: [],
      newPerson: {name: "", bio: "", bioVisible: true}
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/people").then(function(response){
      console.log(response.data);
      this.people = response.data;
    }.bind(this));
  },
  methods: {
    addPerson: function() {
      var params = {
        name: this.newPerson.name, 
        bio: this.newPerson.bio
      }
      axios.post("http://localhost:3000/api/people", params).then(function(response){
        this.people.push(response.data);
        this.newPerson.name = "";
        this.newPerson.bio = "";
      }.bind(this));
    },
    deletePerson: function(person) {
      // find the index of this person in the people array
      var index = this.people.indexOf(person);
      this.people.splice(index, 1);
    },
    toggleBioVisible: function(person) {
      // sets bioVisible equal to the opposite of what it was
      person.bioVisible = !person.bioVisible;
    }
  },
  computed: {}
};
</script>

















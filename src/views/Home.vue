<template>
  <div class="home">

    <div class="container">
      <h4>Total number of people: {{ people.length }}</h4>

      <div class="row">
        <div class="col-md-8 col-md-push-1 col-sm-12 col-sm-push-0 col-xs-12 col-xs-push-0">
          <div class="row">
            <div class="col-md-12">
              <div class="form-group">
                <input v-model="newPerson.name" class="form-control" placeholder="Name" type="text">
              </div>
            </div>
            <div class="col-md-12">
              <div class="form-group">
                <textarea v-model="newPerson.bio" name="" class="form-control" id="" cols="30" rows="7" placeholder="Bio"></textarea>
              </div>
            </div>
            <div class="col-md-12">
              <div class="form-group">
                <input v-on:click="addPerson()" value="Add Person" class="btn btn-primary" type="submit">
              </div>
            </div>
          </div>
        </div>
      </div>
    
      <div>
        <li v-for="error in errors">{{ error }}</li>
      </div>

      <div class="row">
        <div class="col-md-6 col-md-offset-3 text-center fh5co-heading animate-box">
          <h2>Interesting People</h2>
        </div>
        <div v-for="person in filterBy(people, this.$parent.nameFilter, 'name', 'bio')" class="col-md-4 text-center item-block">
          <h3 v-on:click="toggleBioVisible(person)">{{ person.name }}</h3>
          <p v-bind:class="{strike: !person.bioVisible}">{{ person.bio }}</p>
          <p><a v-on:click="deletePerson(person)" class="btn btn-primary btn-outline with-arrow">Delete <i class="icon-arrow-right"></i></a></p>
        </div>
      </div>

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
      newPerson: {name: "", bio: ""},
      errors: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/people").then(response => {
      console.log(response.data);
      this.people = response.data;
    });
  },
  methods: {
    addPerson: function() {
      var params = {
        name: this.newPerson.name, 
        bio: this.newPerson.bio
      }
      axios.post("http://localhost:3000/api/people", params).then(response => {
        this.people.push(response.data);
        this.newPerson.name = "";
        this.newPerson.bio = "";
      }).catch(error => {
        this.errors = error.response.data.errors;
      });
    },
    deletePerson: function(person) {
      axios.delete("http://localhost:3000/api/people/" + person.id).then(response => {
        // find the index of this person in the people array
        var index = this.people.indexOf(person);
        this.people.splice(index, 1);
      });
    },
    toggleBioVisible: function(person) {
      // sets bioVisible equal to the opposite of what it was
      person.bioVisible = !person.bioVisible;
    }
  },
  computed: {}
};
</script>

















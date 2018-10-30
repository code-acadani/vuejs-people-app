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
                <input class="form-control" type="file" v-on:change="setFile($event)" ref="fileInput">
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
        <div class="col-md-4">
          <div class="form-group">
            <input v-model="nameFilter" list="names" class="form-control" placeholder="Search people" type="text">
          </div>
        </div>

        <div>
          <button v-on:click="setSortAttribute('name')" class="btn btn-primary">Sort by name 
            <span v-if="sortAttribute === 'name' && sortAscending === 1"><i class="icon-arrow-up"></i></span>
            <span v-else-if="sortAttribute === 'name' && sortAscending === -1"><i class="icon-arrow-down"></i></span>
          </button>

          <button v-on:click="setSortAttribute('bio')" class="btn btn-primary">Sort by bio 
            <span v-if="sortAttribute === 'bio' && sortAscending === 1"><i class="icon-arrow-up"></i></span>
            <span v-else-if="sortAttribute === 'bio' && sortAscending === -1"><i class="icon-arrow-down"></i></span>
          </button>
        </div>
      </div>

      <datalist id="names">
        <option v-for="person in people">{{ person.name }}</option>
      </datalist>

      <div class="row">
        <div class="col-md-6 col-md-offset-3 text-center fh5co-heading animate-box">
          <h2>Interesting People</h2>
        </div>

        <transition-group name="slide-right">
          <div v-for="person in orderBy(filterBy(people, nameFilter, 'name'), sortAttribute, sortAscending)" class="col-md-4 text-center item-block" v-bind:key="person.id">
            <h3 v-on:click="toggleBioVisible(person)">{{ person.name }}</h3>
            <p v-bind:class="{strike: !person.bioVisible}">{{ person.bio }}</p>
            <img :src="person.avatar_url" alt="">
            <p><a v-on:click="deletePerson(person)" class="btn btn-primary btn-outline with-arrow">Delete <i class="icon-arrow-right"></i></a></p>
          </div>
        </transition-group>

      </div>

    </div>

  </div>
</template>

<style>
  .strike {
    text-decoration: line-through;
  }
  /* Vue.js fade */
  .fade-enter-active, .fade-leave-active {
    transition: opacity 3s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }

  /* Vue.js slide-right */
  .slide-right-enter-active {
    transition: all 3s ease;
  }
  .slide-right-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-right-enter, .slide-right-leave-to {
    transform: translateX(10px);
    opacity: 0;
  }

  /* Vue.js slide-left */
  .slide-left-enter-active {
    transition: all .3s ease;
  }
  .slide-left-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-left-enter, .slide-left-leave-to {
    transform: translateX(-10px);
    opacity: 0;
  }
</style>

<script>
var axios = require('axios');
export default {
  data: function() {
    return {
      people: [],
      newPerson: {name: "", bio: ""},
      errors: [],
      nameFilter: "",
      sortAttribute: "name",
      sortAscending: 1,
      avatar: ""
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/people").then(response => {
      console.log(response.data);
      this.people = response.data;
    });
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.avatar = event.target.files[0];
      }
    },
    addPerson: function() {
      var formData = new FormData();
      formData.append("name", this.newPerson.name);
      formData.append("bio", this.newPerson.bio);
      formData.append("avatar", this.avatar);

      axios.post("http://localhost:3000/api/people", formData).then(response => {
        this.people.push(response.data);
        this.newPerson.name = "";
        this.newPerson.bio = "";
        this.$refs.fileInput.value = "";
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
    },
    setSortAttribute: function(attribute) {
      if(this.sortAttribute === attribute) {
        this.sortAscending = this.sortAscending * -1;
      } else {
        this.sortAscending = 1;
      }
      this.sortAttribute = attribute;
    }
  },
  computed: {}
};
</script>

















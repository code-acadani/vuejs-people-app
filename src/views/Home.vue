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
export default {
  data: function() {
    return {
      people: [
        {
          name: "Bob",
          bio: "Small batch salvia Marfa chillwave delectus, odio forage art party laborum street art minim fixie locavore hoodie mollit.",
          bioVisible: true
        },
        {
          name: "Alice",
          bio: "Tattooed letterpress gluten-free ugh, adipisicing scenester church-key gentrify tousled gastropub pour-over Shoreditch asymmetrical lomo High Life.",
          bioVisible: true
        },
        {
          name: "Tibor",
          bio: "Incididunt photo booth ethical reprehenderit adipisicing. Echo Park readymade Bushwick distillery Tonx. +1 semiotics qui duis literally.",
          bioVisible: true
        },
        {
          name: "Živa",
          bio: "Excepteur shabby chic semiotics Marfa, quinoa try-hard polaroid pariatur banh mi selfies incididunt brunch trust fund. Ethical dolor PBR&B Tumblr.",
          bioVisible: true
        }
      ],
      newPerson: {name: "", bio: "", bioVisible: true}
    };
  },
  created: function() {},
  methods: {
    addPerson: function() {
      // push the newPerson object into the people array
      this.people.push(this.newPerson);
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

















<template>
  <div class="flex justify-center items-center">
    <img src="./assets/rick_and_morty.png" class="w-28 h-28 m-8" />
    <h1 class="font-black text-2xl underline decoration-red-700">
      Welcome on our site of Rick and Morty
    </h1>
  </div>
  <div class="bg-red-500 flex justify-center items-center">
    You search a character(s) of Rick and Morty, fill out the form !
  </div>
  <div class="flex items-center justify-center">
    <form class="flex items-center justify-start m-8 flex-col bg-slate-200 w-1/3 p-5 rounded-xl" v-on:submit.prevent="update">
      <h2 class="underline text-2xl">Formulaire :</h2>
      <div class="h-36 flex flex-col justify-around">
        <div>
          <label class="mr-1" for="nameInput">Character Name</label>
          <input
            class="text-center"
            v-model="name"
            type="text"
            name="nameCharacter"
            id="nameInput"
            placeholder="Name Character"
          />
        </div>
        <div>
          <label class="mr-1" id="status">Status</label>
          <select v-model="status" name="status" id="status">
            <option value="">Défault</option>
            <option value="alive">Alive</option>
            <option value="dead">Dead</option>
            <option value="unknown">Unknown</option>
          </select>
        </div>
        <div>
          <label class="mr-1" id="gender">Gender</label>
          <select v-model="gender" name="gender" id="gender">
            <option value="">Défault</option>
            <option value="male">Male</option>
            <option value="female">Female</option>
            <option value="genderless">Genderless</option>
            <option value="unknown">Unknown</option>
          </select>
        </div>
      </div>
      <button @click="searchCharacters()" class=" bg-sky-600 rounded-lg p-3">
        Find Characters
      </button>
    </form>
  </div>
  <Character msg="Welcome to Your Vue.js App" />
</template>

<script>
import Character from "./components/Character.vue";
import "./assets/tailwind.css";
import { ApolloClient, InMemoryCache, gql } from "@apollo/client";

export default {
  name: "App",
  components: {
    Character,
  },
  data: function () {
    return {
      name: "",
      status: "",
      gender: "",
    };
  },
  methods: {
    searchCharacters() {
      const client = new ApolloClient({
        uri: "https://rickandmortyapi.com/graphql",
        cache: new InMemoryCache(),
      });
      client
              .query({
                query: gql`
                  query Query {
                    characters(filter: {name:"${this.name}", status:"${this.status}", gender:"${this.gender}"}) {
                      results {
                        id
                        name
                        gender
                        status
                        image
                        species
                      }
                    }
                  }
                `,
              })
              .then((result) => {
                this.$store.state.characters=result.data.characters.results;
                console.log(result.data.characters.results)
                }
              );
          },
        },
};
</script>

<style></style>

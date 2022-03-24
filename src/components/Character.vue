<template>
  <div class="">
    <div class="underline text-lg font-bold text-center">
      <span class="bg-orange-500 p-2 rounded-lg">Résults :</span>
    </div>
    <div v-if="this.$store.state.characters">
      <div class="text-center mt-5">
        Results numbers : {{ $store.state.info.count }} characters
      </div>
      <div class="flex flex-wrap justify-center align-center">
        <div
          v-for="character in $store.state.characters"
          :key="character.id"
          class="bg-slate-700 m-4 p-4 flex flex-col justify-center items-center text-white"
        >
          <div class="underline">{{ character.name }}</div>
          <img :src="character.image" class="w-28 h-28 mb-2 mt-2" />
          <div class="text-sm">
            <span class="underline">Gender</span> :{{ character.gender }}
          </div>
          <div class="text-sm">
            <span class="underline">Species</span> :{{ character.species }}
          </div>
          <div class="text-sm">
            <span class="underline">Status</span> :{{ character.status }}
          </div>
        </div>
      </div>
      <div class="flex justify-center space-x-2">
        <button
          @click="previousSearch()"
          class="bg-slate-400 hover:bg-slate-500 rounded-lg p-1 text-sm"
        >
          Previous
        </button>
        <button
          @click="nextSearch()"
          class="bg-slate-400 hover:bg-slate-500 rounded-lg p-1 text-sm"
        >
          Next
        </button>
      </div>
    </div>
    <div class="text-center mt-10" v-else>Not results</div>
  </div>
</template>

<script>
import { ApolloClient, InMemoryCache, gql } from "@apollo/client";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  methods: {
    previousSearch() {
      const client = new ApolloClient({
        uri: "https://rickandmortyapi.com/graphql",
        cache: new InMemoryCache(),
      });
      if (this.$store.state.info.prev != null) {
        client
          .query({
            query: gql`
                    query Query {
                      characters(page: ${this.$store.state.info.prev},filter: {name:"${this.$store.state.name}", status:"${this.$store.state.status}", gender:"${this.$store.state.gender}"}) {
                        results {
                          id
                          name
                          gender
                          status
                          image
                          species
                        }
                        info{
                          count
                          prev
                          next
                        }
                      }
                    }
                  `,
          })
          .then((result) => {
            this.$store.state.characters = result.data.characters.results;
            this.$store.state.info = result.data.characters.info;
            console.log(result.data.characters);
          });
      }
    },
    nextSearch() {
      const client = new ApolloClient({
        uri: "https://rickandmortyapi.com/graphql",
        cache: new InMemoryCache(),
      });
      if (this.$store.state.info.next != null) {
        client
          .query({
            query: gql`
                  query Query {
                    characters(page: ${this.$store.state.info.next},filter: {name:"${this.$store.state.name}", status:"${this.$store.state.status}", gender:"${this.$store.state.gender}"}) {
                      results {
                        id
                        name
                        gender
                        status
                        image
                        species
                      }
                      info{
                        count
                        prev
                        next
                      }
                    }
                  }
                `,
          })
          .then((result) => {
            this.$store.state.characters = result.data.characters.results;
            this.$store.state.info = result.data.characters.info;
            console.log(result.data.characters);
          });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

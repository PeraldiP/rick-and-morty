<template>
  <div class="">
    <div class="underline text-lg font-bold text-center"><span class="bg-orange-700 p-2 rounded-lg">Résults :</span></div>
    <div v-if="this.$store.state.characters" class="flex flex-wrap justify-center align-center">
      <div  v-for="character in $store.state.characters" :key="character.id" class="bg-slate-700 m-4 p-4 w-52 flex flex-col justify-center items-center text-white">
        <div class="underline">{{character.name}}</div>
        <img :src="character.image" class="w-28 h-28 mb-2 mt-2">
        <div><span class="underline">Gender</span> :{{character.gender}}</div>
        <div><span class="underline">Species</span> :{{character.species}}</div>
        <div class="text-sm"><span class="underline ">Status</span> :{{character.status}}</div>
      </div>    
    </div> 
    <div class="text-center mt-10" v-else> Not results</div> 
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
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

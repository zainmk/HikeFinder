<template>
  <div id="app">
    <main>
      <h1> Hike Finder </h1>
      <br>
      <p style="font-size:20px"> Input your latitude and longitude coordiantes to search for hikes within a 200 mile radius </p>
      <br> 
      <div class="search-box-lat">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Input Latitude..."
          v-model="query_lat"
        />
      </div>

       <div class="search-box-lon">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Input Longitude..."
          v-model="query_lon"
        />
      </div>

      <button class="search-button" v-on:click="fetchTrail">Find trails nearby... </button>
      <br>
      <br>

      <div class="trail-wrap" v-if="typeof trail.trails != 'undefined'">
        <div class="name-box">
          <ul>
            <li v-for="name in names" v-bind:key="name.id"> <p style="font-size:30px; font-style:italic; font-weight:500"> {{ name }} : </p>  <p> {{name_and_sum[name]}} </p> </li>
          </ul>
        </div>
      </div>

    </main>
  </div>
</template>


<script>
export default {
  name:'app',

  data () {
    return {
      api_key: '200847193-8e9e680997981ef802ae640231c68e1f',
      url_base: 'https://www.hikingproject.com/data/',
      query_lat: '',
      query_lon: '',
      trail: {},
      name_and_sum: {},
      names: []
    }
  },

  methods: {
    fetchTrail () {
        this.names = [];
        this.names_and_sum = {};
        var APIcall = `${this.url_base}get-trails?lat=${this.query_lat}&lon=${this.query_lon}&maxDistance=200&key=${this.api_key}`;

        fetch(APIcall).then(res => {return res.json();}).then(this.setResults);
    },

    setResults (results){
      this.trail = results;
      var numberOf = this.trail.trails.length;

      for(var x = 0; x < numberOf; x++){
        this.name_and_sum[this.trail.trails[x].name] = this.trail.trails[x].summary;
        this.names.push(this.trail.trails[x].name);
      }

      console.log(this.names);

    }
  
  }

}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('./assets/mountains.png');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-button{
  padding:10px;

}
.search-box-lat {
  width: 100%;
  margin-bottom: 30px;
}
.search-box-lat .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box-lat .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.search-box-lon {
  width: 100%;
  margin-bottom: 30px;
}
.search-box-lon .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box-lon .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.trail-wrap{
  font-size:20px;
  color: white;

}



</style>

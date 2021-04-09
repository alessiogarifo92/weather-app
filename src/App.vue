<template>
  <div id="app"
   v-bind:style="[typeof cityInfo.main != 'undefined' && cityInfo.weather[0].description == 'clear sky' ? {'background-image': 'url(' + clearsky + ')'} : '' ||
  typeof cityInfo.main == 'undefined' ? {'background-image': 'url(' + common + ')'} : '' ||
   cityInfo.weather[0].description.includes('rain') ? {'background-image': 'url(' + rain + ')'} : '' ||
   cityInfo.weather[0].description.includes('snow') ? {'background-image': 'url(' + snow + ')'} : '' ||
   cityInfo.weather[0].description.includes('clouds') ? {'background-image': 'url(' + clouds + ')'} : '']"
   >
    {{testo}}

    <div id="nav">
      <input
      type="text"
       placeholder="Find a city..."
       v-model="search"
       @keyup.enter="callApi()"
       >
       <!-- {{search}} -->

      <!-- <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> -->
    </div>

    <div class="weather-wrap" v-if="typeof cityInfo.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{cityInfo.name}}, {{cityInfo.sys.country}}</div>
        <div class="date">{{getDate()}}</div>

      </div>

      <div class="weather-box">
        <div class="temp">{{Math.round(((cityInfo.main.temp - 273.15)*10)/10)}}°C</div>
        <div class="weather">{{cityInfo.weather[0].description}}</div>
      </div>

    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "app",
  data(){
    return{
      testo: "Alessio's weather app",
      base_url:'http://api.openweathermap.org/data/2.5/weather?q=', //after q goes city name
      api_key:'&appid=9054ea18dfc129750f1d62bb7e70679d',
      search: '',
      cityInfo:{},
      rain:'https://media0.giphy.com/media/PbOaO2fedzQLm/giphy.gif',
      common:'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSh-vdKxqfe26w2T9cEKJspBleav6DWlSPxBA&usqp=CAU',
      clouds:'https://wallpaperaccess.com/full/1244184.jpg',
      clearsky:'https://media.istockphoto.com/photos/clear-blue-sky-background-picture-id508544168?k=6&m=508544168&s=170667a&w=0&h=azbwycCT3kYUIABf-hueRpoeLwNMvkxeQ9Cl6KiyELQ=',
      snow: 'https://24.media.tumblr.com/594b4d60991a7a5ea6fa3de7e7b60a2a/tumblr_mf57jfByLe1ri2csvo1_500.gif'
    }
  },
  methods:{
    callApi(){
      if (this.search !== '') {
        fetch(this.base_url + this.search + this.api_key)//potevo aggiungere direttamente qui &units=metric prima di api_key per avere tutto in celsius
        .then(response => response.json())
       .then(data =>
          // console.log(data,data.name,data.sys.country,Math.round(((data.main.temp - 273.15)*10)/10));
       this.cityInfo = data
        );
        // console.log(this.cityInfo);
       this.search = '';
      }

    },
    getDate(){
      let d= new Date();
      let months= ["January", "February", "March", "April", "May", "June",
      "July", "August", "September", "October", "November", "December"
      ];
      let days =  ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      let day =days.[d.getDay()];
      let date =d.getDate();
      let month =months.[d.getMonth()];
      let year =d.getFullYear();

      return day + ', '+ date + ' ' + month + ' ' + year
    }
  }

}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSh-vdKxqfe26w2T9cEKJspBleav6DWlSPxBA&usqp=CAU');
  background-repeat:no-repeat;
  background-size:cover;
  height: 500px;
}

#nav {
  padding: 30px;

  input{
    color:black;
    width:300px;
    padding: 15px;
    border:none;
    // outline-color: yellow;
     outline:none;
    background-color:rgba(255,255,255,0.7);
    border-radius: 0px 16px 0px 16px;
    transition:0.4s;
    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  }

  input:focus{
    background-color:rgba(255,255,255,1);
    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    border-radius: 16px 0px 16px 0px;
  }

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.location-box .location{
  color:white;
  font-size:32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.location-box .date{
  color:white;
  font-size:32px;
  font-weight: 300;
  font-style:italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}

.weather-box{
text-align: center;
}

.weather-box .temp{
  display:inline-block;
  padding:10px 25px;
  color:white;
  font-size:102px;
  font-weight:900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather{
  color:white;
  font-size:48px;
  font-weight:700;
  font-style:italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>

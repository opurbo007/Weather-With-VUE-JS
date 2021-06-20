<template>
   <div
      id="app"
      :class="
         typeof weatherStore.main != 'undefined' && weatherStore.main.temp > 16
            ? 'hot'
            : ''
      "
   >
      <main class="container">
         <input
            type="text"
            class="search-bar"
            placeholder="Search location"
            v-model="query"
            v-on:keypress="weatherFunc"
         />
         <div class="timezone" v-if="typeof weatherStore.main != 'undefined'">
            <h1 class="location">
               {{ weatherStore.name }},{{ weatherStore.sys.country }}
            </h1>
            <p class="date">{{ getDate() }}</p>
         </div>

         <div class="weather" v-if="typeof weatherStore.main != 'undefined'">
            <h1 class="temp">{{ Math.round(weatherStore.main.temp) }}°C</h1>
            <h2 class="condition">{{ weatherStore.weather[0].main }}</h2>
         </div>
      </main>
   </div>
</template>

<script>
export default {
   name: "App",
   data() {
      return {
         api_key: "402af7c04ffc877a3e80cc536176741c",
         url: "https://api.openweathermap.org/data/2.5/",
         query: "",
         weatherStore: {},
      };
   },
   methods: {
      weatherFunc(e) {
         if (e.key == "Enter") {
            fetch(
               `${this.url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
            )
               .then((res) => {
                  return res.json();
               })
               .then(this.finalReuslt);
         }
      },
      finalReuslt(results) {
         this.weatherStore = results;
      },
      getDate() {
         const currentDate = new Date();

         const monthName = [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December",
         ];
         const dayName = [
            "Sunday",
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday",
            "Saturday",
         ];

         let day = dayName[currentDate.getDay()];
         let date = currentDate.getDate();
         let month = monthName[currentDate.getMonth()];
         let year = currentDate.getFullYear();

         return `${day} ${date} ${month} ${year} `;
      },
   },
};
</script>

<style>
* {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
   list-style: none;
   text-decoration: none;
   font-family: "Times New Roman", Times, serif;
}

#app {
   background: url("./assets/cold.png");
   background-size: cover;
   background-position: bottom;
   transition: 0.6s;
}
.container {
   min-height: 100vh;
   background: linear-gradient(to bottom, rgb(0, 0, 0, 0.2), rgb(0, 0, 0, 0.5));
}

.search-bar {
   min-width: 80%;
   height: 55px;
   margin: 12vh 0 5vh 10vw;
   text-align: center;
   border: none;
   background-color: rgb(255, 255, 255, 0.5);
   box-shadow: 0 0 10px rgb(0, 0, 0, 0.29);
   border-radius: 0px 15px 0px 15px;
   font-size: 20px;
   outline: none;
   transition: 0.5s;
}
.search-bar:focus {
   border-radius: 15px 0px 15px 0px;
   box-shadow: 8px 10px 20px rgb(0, 0, 0, 0.25);
}

.timezone,
.weather {
   text-align: center;
   color: white;
   letter-spacing: 1.5px;
}
.location {
   font-size: 40px;
}
.date {
   font-size: 20px;
   font-style: italic;
   padding-bottom: 5vh;
}
.temp {
   display: inline-block;
   font-size: 85px;
   font-weight: bolder;
   padding: 4px 8px;
   border-radius: 15px;
   background-color: rgba(255, 255, 255, 0.315);
   box-shadow: 5px 6px 8px rgb(0, 0, 0, 0.25);
}
.condition {
   font-size: 50px;
   padding: 10px;
}
#app .hot {
   background: url("./assets/hot.png");
   background-position: right;
}
@media only screen and (max-width: 1400px) {
   #app .hot {
      background-position: -400px;
   }
}
</style>

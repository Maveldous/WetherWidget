<template>
  <div class="wetherPlugin">
   <div class="wetherPlugin__wrapper">
     <p class="wetherPlugin__place">{{dataObject.name}}</p>
     <p class="wetherPlugin__date">
       {{dataObject.dt | dateFilter}}
      </p>
      <img 
      :src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" alt="">
      <p class="wetherPlugin__wether">{{dataObject.weather[0].description}}</p>
   </div>
   <div class="wetherPlugin__temperature">
     <p class="wetherPlugin__temperature-bigNum">{{dataObject.main.temp | conversionСelsius}}&deg;</p>
     <p class="wetherPlugin__temperature-smallNum">
       {{dataObject.main.temp_max | conversionСelsius}}&deg;
       / {{dataObject.main.temp_min | conversionСelsius}}&deg;
     </p>
   </div>
  </div>
</template>

<script>
export default {
  name: "wetherPlugin",
  data: function(){
    return {
      dataObject: {},
      icon: ''
    }
  },
  filters: {
    dateFilter: function(value){
      return value
    },
    conversionСelsius: function(value){
      return (value - 273.15).toFixed(0)
    }
  },
  beforeCreate(){
    const URL = "http://api.openweathermap.org/data/2.5/weather?q=Odesa&appid=177acef01ac47358d30332c4f741e485"
    fetch(URL)
      .then( (responce) => {
        return responce.json()
      })
      .then( (data) =>{
        console.log(data);
        this.dataObject = data
        this.icon = data.weather[0].icon
      })
  }
};
</script>

<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,300;0,400;1,100&display=swap');

  p{
    margin: 0;
  }

  .wetherPlugin{
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: #fff;

    width: 600px;
    height: 395px;
    background: url('../assets/background1.png') no-repeat;
    border: none;
    border-radius: 5px;
    background-size: contain;
    padding: 50px;

    display: flex;
    justify-content: space-between;

    &__wrapper{

    }

    &__place{
      font-size: 30px;
    }

    &__date{
      font-size: 15px;
    }

    &__wether{
      margin-top: -30px;
      font-weight: 400;
    }

    &__temperature{
      text-align: center;
      font-weight: 100;
      &-bigNum{
        font-size: 130px;
      }
      &-smallNum{
        font-size: 35px;
      }
    }
  }

</style>

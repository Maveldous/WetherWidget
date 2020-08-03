<template>
  <div class="wetherPlugin">
    <div v-if="load" class="lds-default"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
  <div v-else class="wetherPlugin__wrapper">
     <p class="wetherPlugin__place">{{dataObject.name}}</p>
     <p class="wetherPlugin__date">
       {{dataObject.dt | dateFilter}}
      </p>
      <img 
      :src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" alt="">
      <p class="wetherPlugin__wether">{{dataObject.weather[0].description}}</p>
      <div class="wetherPlugin__temperature">
        <p class="wetherPlugin__temperature-bigNum">{{dataObject.main.temp | conversionСelsius}}</p>
        <p class="wetherPlugin__temperature-smallNum">
          {{dataObject.main.temp_max | conversionСelsius}}  
        </p>
        <p class="wetherPlugin__temperature-symbol">/</p>
        <p class="wetherPlugin__temperature-smallNum blue">
          {{dataObject.main.temp_min | conversionСelsius}}
        </p>
      </div>
      <div class="wetherPlugin__weekMenu"></div>
   </div>
  </div>
</template>

<script>
export default {
  name: "wetherPlugin",
  data: function(){
    return {
      dataObject: {
        dt: new Date()
      },
      icon: '',
      load: true
    }
  },
  filters: {
    dateFilter: function(value){
      return new Date(value).toISOString().substr(0, 10)
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
        this.load = false
      })
  }
};
</script>

<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,300;0,400;1,100&display=swap');

  p{
    margin: 0;
    padding: 0;
  }

  .wetherPlugin{
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
    font-weight: 300;
    color: #fff;

    position: relative;
    width: 600px;
    height: 395px;
    background: url('../assets/background1.png') no-repeat;
    border: none;
    border-radius: 5px;
    background-size: contain;
    padding: 50px;

    &__wrapper{
      height: 100%;
    }

    &__place, &__wether{
      font-size: 30px;
      font-weight: 400;
    }

    &__date{
      font-size: 15px;
    }

    &__wether{
      margin-top: -30px;
    }

    &__temperature{
      position: absolute;
      top: 30px;
      right: 80px;
      text-align: center;
      font-weight: 100;
      &-bigNum{
        font-size: 130px;
        position: relative;
        &::after{
          content: '\00B0';
          position: absolute;
          font-family: 'Roboto', sans-serif;
          font-weight: 100;
          font-size: 80px;
          top: 0px;
        }
      }
      &-smallNum{
        font-size: 35px;
        position: relative;
        display: inline-block;
        margin-right: 15px;
        &::after{
          content: '\00B0';
          position: absolute;
          font-family: 'Roboto', sans-serif;
          font-weight: 100;
          font-size: 30px;
          top: -5px;
        }
      }
      &-symbol{
        font-size: 35px;
        display: inline-block;
        margin-right: 10px;
      }
      .blue{
        color: rgb(57, 140, 250);
      }
    }
    &__weekMenu{
      height: 50%;
      border: 1px solid #fff;
    }
  }



//Loader

.lds-default {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-default div {
  position: absolute;
  width: 6px;
  height: 6px;
  background: #fff;
  border-radius: 50%;
  animation: lds-default 1.2s linear infinite;
}
.lds-default div:nth-child(1) {
  animation-delay: 0s;
  top: 37px;
  left: 66px;
}
.lds-default div:nth-child(2) {
  animation-delay: -0.1s;
  top: 22px;
  left: 62px;
}
.lds-default div:nth-child(3) {
  animation-delay: -0.2s;
  top: 11px;
  left: 52px;
}
.lds-default div:nth-child(4) {
  animation-delay: -0.3s;
  top: 7px;
  left: 37px;
}
.lds-default div:nth-child(5) {
  animation-delay: -0.4s;
  top: 11px;
  left: 22px;
}
.lds-default div:nth-child(6) {
  animation-delay: -0.5s;
  top: 22px;
  left: 11px;
}
.lds-default div:nth-child(7) {
  animation-delay: -0.6s;
  top: 37px;
  left: 7px;
}
.lds-default div:nth-child(8) {
  animation-delay: -0.7s;
  top: 52px;
  left: 11px;
}
.lds-default div:nth-child(9) {
  animation-delay: -0.8s;
  top: 62px;
  left: 22px;
}
.lds-default div:nth-child(10) {
  animation-delay: -0.9s;
  top: 66px;
  left: 37px;
}
.lds-default div:nth-child(11) {
  animation-delay: -1s;
  top: 62px;
  left: 52px;
}
.lds-default div:nth-child(12) {
  animation-delay: -1.1s;
  top: 52px;
  left: 62px;
}
@keyframes lds-default {
  0%, 20%, 80%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
}

</style>

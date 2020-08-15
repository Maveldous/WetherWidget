<template>
  <div class="wetherPlugin">
    <div v-if="load" class="lds-default"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
  <div v-else class="wetherPlugin__wrapper">
     <p class="wetherPlugin__place">{{dataObject.name}}</p>
     <p class="wetherPlugin__date">
       {{dataObject.dt | dateFilter}}
      </p>
      <div class="wetherPlugin__icon"><i :class="convertIcon(dataObject.weather[0].icon)"></i></div>
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
      <div class="wetherPlugin__weekMenu">
        <button 
          v-for="(item, index) of weekMenuArr" 
          :key="index"
          @click.prevent='switchStrip(index)' 
          :class="{'active': curChoose == index}"
          class="weekMenu__btn">
          {{item.title}}
        </button>
        <div 
          v-for="(item, index) of weekMenuArr" 
          :key="index + item"
          v-show="curChoose == index"
          class="weekMenu__content">
          <div 
            v-for="(hourItem, hourIndex) of dataMain" 
            :key="hourIndex + 'hour'" 
            class="weekMenu__content-item"
            >
            {{hourItem.dt}}
            <div class="item__icon"><i :class="convertIcon(hourItem.weather[0].icon)"></i></div>
            <p class="item-temp">
              {{hourItem.temp | conversionСelsius}}  
            </p>
          </div>
        </div>
      </div>
   </div>
  </div>
</template>

<script>
export default {
  name: "wetherPlugin",
  data: function(){
    return {
      dataObject: {
      },
      dataBig: {},
      dataMain: {},
      weekMenuArr: [
        {
          title: 'Hourly',
        },
        {
          title: 'Daily',
        },
      ],
      iconsGet: {
        '01d': 'fas fa-sun',
        '02d': 'fas fa-cloud-sun',
        '03d': 'fas fa-cloud',
        '04d': 'fas fa-cloud-sun',
        '09d': 'fas fa-cloud-showers-heavy',
        '10d': 'fas fa-cloud-rain',
        '11d': 'fas fa-bolt',
        '13d': 'far fa-snowflake',
        '50d': 'fas fa-smog',
        '01n': 'fas fa-moon',
        '02n': 'fas fa-cloud-moon',
        '03n': 'fas fa-cloud',
        '04n': 'fas fa-cloud-moon',
        '09n': 'fas fa-cloud-showers-heavy',
        '10n': 'fas fa-cloud-rain',
        '11n': 'fas fa-bolt',
        '13n': 'far fa-snowflake',
        '50n': 'fas fa-smog',
      },
      mainIcon: '',
      curChoose: 0,
      load: true
    }
  },
  props: ['country'],
  filters: {
    dateFilter: function(value){
      return new Date(value).toString().substr(4, 11)
    },
    conversionСelsius: function(value){
      return (value - 273.15).toFixed(0) + ''
    }
  },
  methods: {
    convertIcon: function(value){
      for (let key in this.iconsGet){
        if (key == value) return this.iconsGet[key];
      }
    },
    switchStrip: function(index){
      this.curChoose = index;
      if(this.dataMain.curState == this.curChoose) {return}
        switch (index) {
          case 0:
            this.dataMain = this.dataBig.hourly.slice(0,6)
            this.dataMain.curState = index
            break;
          case 1:
            this.dataMain = JSON.parse(JSON.stringify(this.dataBig.daily.slice(0,6)))
            this.dataMain.forEach(item => {
              item.temp = item.temp.day
            });
            this.dataMain.curState = index
            break;
          default:
            break;
        }
      }
  },
  mounted(){
    const URL = "https://api.openweathermap.org/data/2.5/weather?q="+ this.country+"&appid=177acef01ac47358d30332c4f741e485"
    fetch(URL)
      .then(responce => responce.json())
      .then(data => {
        data.dt = new Date();
        this.dataObject = data
        fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${data.coord.lat}&lon=${data.coord.lon}&exclude=minutely&appid=177acef01ac47358d30332c4f741e485`)
          .then( responce => responce.json())
          .then( dataNew => {
            //dataBig - массив со всем прогнозом включая часы, дни и недели
            this.dataBig = dataNew
            this.dataMain = this.dataBig.hourly.slice(0,6);
            //установка времени
            this.dataMain.forEach((elem,index) => elem.dt = 
            (new Date().getHours() + 1 + index > 24) 
            ? new Date().getHours() + 1 + index - 24 + 'pm' 
            : new Date().getHours() + index + 1 + 'pm' )
            this.load = false
            this.dataBig.daily
              .slice(0,6)
              .forEach((elem,index) => elem.dt = 
              (new Date().getDate() + index > 30)
              ? new Date().getDate() + index - 30 + 'th'
              : new Date().getDate() + index + 'th')
          })
      })
  }
};
</script>

<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;1,100;1,300&display=swap');
  @import url('https://pro.fontawesome.com/releases/v5.10.0/css/all.css');

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
    max-width: 600px;
    height: 400px;
    background: url('../assets/background1.png') no-repeat;
    border: none;
    border-radius: 20px;
    background-size: cover;
    padding: 50px;

    &__place, &__wether{
      font-size: 30px;
      font-weight: 400;
    }

    &__date{
      font-size: 15px;
    }

    &__icon{
      font-size: 55px;
      margin: 15px 0 -10px 0;
    }
    &__temperature{
      position: absolute;
      top: 34px;
      right: 80px;
      text-align: center;
      font-weight: 100;
      &-bigNum{
        font-size: 110px;
        position: relative;
        &::after{
          content: '\00B0';
          position: absolute;
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
        color: rgb(76, 150, 247);
      }
    }
    &__weekMenu{
      margin-top: 40px;
      height: 100px;
    }
  }

  //accordione styles

  .weekMenu__btn{
    background: transparent;
    border: none;
    color: rgba(255, 255, 255, 0.5);
    font-size: 16px;
    cursor: pointer;
    margin: 0 5px;
    &.active{
      color: #fff;
    }
  }

  .weekMenu__content{
    padding: 15px 5px;
    text-align: center;
    display: flex;
    justify-content: space-around;
  }

  .weekMenu__content-item{
    display: inline-block;
    font-size: 20px;
  }

  .item__icon{
    font-size: 30px;
  }

  .item-temp{
    font-size: 20px;
    position: relative;
    &::after{
      content: '\00B0';
      position: absolute;
      font-size: 20px;
      top: -3px;
    }
  }

//adaptive

  @media (max-width: 500px){
      .wetherPlugin{
        max-width: 350px;
        height: 370px;
        padding: 25px;
        &__temperature{
          top: 34px;
          right: 40px;
          &-bigNum{
            font-size: 80px;
            right: 0;
            &::after{
              font-size: 40px;
            }
          }
          &-smallNum{
            font-size: 25px;
            margin-right: 5px;
            &::after{
              font-size: 20px;
            }
          }
          &-symbol{
            font-size: 25px;
          }
        }
      }
      .weekMenu__content-item{
        font-size: 15px;
      }
      .weekMenu__content{
        justify-content: space-between;
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

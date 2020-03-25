<template>
  <div id="app">
    <div>练习使用axios与fetch</div>
    <div>
      <span>当前时间：</span>
      <span>{{timer.loc?timer.loc:''}}</span>
    </div>
    <div>
      <span>当前城市：</span>
      {{`${lOcation.admin_area}省${lOcation.parent_city}市`}}
      <span></span>
    </div>
    <div>
      <span>当前温度：</span>
      {{`${weather.tmp}°`}}
    </div>
    <!-- <div><span>当前气象：</span>{{`${weather.cond_txt}`}}</div> -->
    <div></div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  created() {
    const URL = "https://free-api.heweather.net/s6/weather/now";
    const loc = "shenzhen";
    const key = "2185a5a8b513423c8b7968dca8e7b236";
    axios
      .request({
        url: URL,
        method: "get",
        params: {
          location: loc,
          key: key
        }
      })
      .then(data => {
        const newdata = data.data;
        if (newdata.HeWeather6 && newdata.HeWeather6.length > 0) {
          const weather = newdata.HeWeather6[0];
          this.weather = weather.now;
          this.lOcation = weather.basic;
          this.timer = weather.update;
        }
      });
     fetch(URL+'?'+`location=${loc}`+'&'+`key=${key}`).then(data=>
      data.json()
     ).then(data=>{
       console.log(data,"fetch")
     })
  },
  data() {
    return {
      weather: {},
      lOcation: {},
      timer: {}
    };
  }
};
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

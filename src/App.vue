<template>
  <div id="app" class="container-fiuld">
    <div class="row">
      <div class="logos">
        <img class="logo" src="./assets/actility.png">
        <img class="logo" src="./assets/elsys.png">
        <img class="logo" src="./assets/lora.png">
      </div>
    </div>
    <hr>
    <div class="row">
      <div class="logos">
        <img class="device" src="./assets/ers_v3.jpg">
      </div>
    </div>
    <div class="row">
      <div class="container contents">
        <app-indicator name="CO2"
          :value="data.co2"
          maxvalue='100'
          warning='100'
          critical='100'
          metric=""
          />
        <app-indicator name="Temperature"
          :value="data.temperature"
          maxvalue='50'
          warning='32'
          critical='40'
          metric="°C"
          />
        <app-indicator name="Humidity"
          :value="data.humidity"
          maxvalue=100
          warning=100
          critical=100
          metric="%"
          />
        <app-indicator name="Light"
          :value="data.light"
          maxvalue=100
          warning=100
          critical=100
          metric=""
          />
        <app-indicator name="Motion"
          :value="data.motion"
          maxvalue=100
          warning=100
          critical=100
          metric=""
          v-if="false"
          />
        <app-indicator name="VDD"
          :value="data.vdd"
          maxvalue=100
          warning=100
          critical=100
          metric=""
          v-if="false"
          />
        <div class="form-group">
          <form class="form-inline">
            <div class="form-group mb-2">
              <button type="button" class="btn btn-primary" @click="fetchData">Refresh</button>
            </div>
            <div class="form-group mx-sm-3 mb-2">
              <label for="refreshrate" class="sr-only">Refresh Rate</label>
              <input type="text" class="form-control" id="refreshrate" placeholder="e.g. 1000 (ms)" v-model="refreshrate">
            </div>
            <button type="button" class="btn btn-primary" @click="updateRefresh">Update</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Indicator from "./components/Indicator";

export default {
  name: 'app',
  components:{
    appIndicator: Indicator
  },
  data () {
    return {
      data: {
        co2:0,
        humidity:0,
        light:0,
        motion:0,
        temperature:0,
        vdd:0
      },
      timer: '',
      refreshrate: 3000
    }
  },
  created(){
    this.fetchData();
    this.timer = setInterval(this.fetchData, this.refreshrate);
  },
  destroyed(){
    clearInterval(this.timer);
  },
  methods:{
    fetchData(){
      this.$http.get('sensor.json')
        .then( response => {
          return response.json();
        })
        .then(data => {
          this.data = data;
          console.log(data);
        });
    },
    updateRefresh(){
      clearInterval(this.timer);
      this.timer = setInterval(this.fetchData, this.refreshrate);
    }
  }
}
</script>

<style lang="scss">
body{
  padding: 0;
  margin: 0;
  overflow-x: hidden;
}

.logos{
  padding-top: 1em;
  margin-left: auto;
  margin-right: auto;
}
.logo{
    padding-left:  1.5em;
    padding-right: 1.5em;
}
.device{
  width: 120px;
  height: 120px;
}
</style>

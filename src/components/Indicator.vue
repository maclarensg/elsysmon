<template>
  <div class="form-group">
    <label>{{name}}</label>
    <div class="progress" data-toggle="tooltip" data-placement="top" :title="getValue">
      <div class="progress-bar progress-bar-striped" role="progressbar" :style="width" aria-valuenow="15" aria-valuemin="0" aria-valuemax="100" v-if="normalLevel">{{value}}{{metric}}</div>
      <div class="progress-bar progress-bar-striped bg-warning" role="progressbar" :style="width" aria-valuenow="30" aria-valuemin="0" aria-valuemax="100" v-if="warningLevel">{{value}}{{metric}}</div>
      <div class="progress-bar progress-bar-striped bg-danger" role="progressbar" :style="width" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100" v-if="criticalLevel">{{value}}{{metric}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props:{
    name: String,
    warning: [Number, String],
    critical:[Number, String],
    value: [Number, String],
    maxvalue: [Number, String],
    metric: String
  },
  methods:{

  },
  computed:{
    width(){
      return {
        'width': (this.value/this.maxvalue*100) + "%"
      }
    },
    getValue(){
      return this.value + this.metric;
    },
    normalLevel(){
      if((this.maxvalue === this.warning) && (this.warning === this.critical))
        return true;
      return this.value < this.warning;
    },
    warningLevel(){
      if((this.maxvalue === this.warning) && (this.warning === this.critical))
        return false;
      return (this.value >= this.warning) && (this.value < this.critical);
    },
    criticalLevel(){
      if((this.maxvalue === this.warning) && (this.warning === this.critical))
        return false;
      return (this.value >= this.critical);
    }
  }

}
</script>

<style lang="scss">
</style>

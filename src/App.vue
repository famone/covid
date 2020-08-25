<template>
  <div id="app">

<section id="inner">
  <img src="https://cdn.smassets.net/assets/cms/cc/uploads/sites/13/taller-header-coronavirus-resources.png" alt="">
  <div class="container">
    <div class="row">
      <div class="col-lg-12">
      <h1>Coronavirus <br> COVID-19</h1>
    </div>
    </div>
  </div>
</section>


<img v-if="loading" src="https://i.gifer.com/TOWb.gif" style="width: 100%;">


<section id="world">
  <div class="container">
    <div class="row">
      <div class="col-lg-12">
        <h2>World's statistics</h2>
      </div>
    </div>
  
        <div class="wrld-stat text-center">
          <div class="row">
            <div class="col-xs-4">
              <h3>Infected</h3>
              <p class="nums"  style="color: #e19a83;">{{ totalInWorld }}</p>
            </div>
            <div class="col-xs-4">
              <h3>Deaths</h3>
              <p class="nums"  style="color: #bb4d5c;">{{ totalRip }}</p>
            </div>
            <div class="col-xs-4">
              <h3>Recovered</h3>
              <p class="nums" style="color: #5ebc55;">{{ totalRecover }}</p>
            </div>
          </div>
        </div>

  </div>
</section>
        


<section id="select" v-if="countries">
  <div class="container">
    <select v-model="selected">
    <option v-for="(country, index) in countries" :key="index" :value="index">{{ country.Country }}</option>
  </select>
  
  <div class="wrld-stat text-center">
          <div class="row">
            <div class="col-xs-4">
              <h3>Infected</h3>
              <p class="nums"  style="color: #e19a83;">{{ getCountry.TotalConfirmed.toLocaleString() }}</p>
            </div>
            <div class="col-xs-4">
              <h3>Deaths</h3>
              <p class="nums"  style="color: #bb4d5c;">{{ getCountry.TotalDeaths.toLocaleString() }}</p>
            </div>
            <div class="col-xs-4">
              <h3>Recovered</h3>
              <p class="nums" style="color: #5ebc55;">{{ getCountry.TotalRecovered.toLocaleString() }}</p>
            </div>
          </div>
        </div>

<h2>Today: {{ getCountry.Date.slice(0, 10) }}</h2>

  <div class="wrld-stat text-center">
          <div class="row">
            <div class="col-xs-4">
              <h3>New confirmed</h3>
              <p class="nums"  style="color: #e19a83;">{{ getCountry.NewConfirmed.toLocaleString() }}</p>
            </div>
            <div class="col-xs-4">
              <h3>New deaths</h3>
              <p class="nums"  style="color: #bb4d5c;">{{ getCountry.NewDeaths.toLocaleString() }}</p>
            </div>
            <div class="col-xs-4">
              <h3>New recovered</h3>
              <p class="nums" style="color: #5ebc55;">{{ getCountry.NewRecovered.toLocaleString() }}</p>
            </div>
          </div>
        </div>

       <!--  <img :src="getFlag" class="flag"> -->

  </div>
</section>

 





  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      totalInWorld: null,
      totalRip: null,
      totalRecover: null,
      countries: null,
      selected: 138,
      selCountry: null,
      loading: true,
      flaglink: null
    }
  },
  computed: {
    getCountryId(){
      return this.selected
    },
    getCountry(){
     return this.countries[this.selected]
    },
    getFlag(){
      return this.flaglink = `http://www.world-globe.ru/files/flags/${this.countries[this.selected].Slug}_l.png`
    }
  },
  mounted(){
    axios
      .get('https://api.covid19api.com/summary')
      .then(response => {
        this.loading = false

        let resp = response.data
        this.totalInWorld = resp.Global.TotalConfirmed.toLocaleString()
        this.totalRip = resp.Global.TotalDeaths.toLocaleString()
        this.totalRecover = resp.Global.TotalRecovered.toLocaleString()
        this.countries = resp.Countries
        console.log(this.countries[this.selected])

      });

      
  }
}
</script>

<style>
body{
  background-color: #f3f2f2;
  font-family: 'Roboto Mono', monospace;

}
#inner{
  background: linear-gradient(65.85deg, #162388 -11.08%, #3470BC 85.05%);
  padding: 60px 0 40px 0;
  position: relative;
  overflow:hidden;
}
#inner img{
  position:absolute;
  height: 140%;
  bottom: -20%;
  right: -45%;
}
h1{
  font-size: 35px;
  color: #fff;
  margin: 0;
}
.wrld-stat{
  background-color: #fff;
  padding: 10px 10px;
  border-radius: 10px;
}
h2{
  font-size: 25px;
  margin-bottom: 20px;
}
h3{
  font-size: 15px;
}
.nums{
  font-size: 17px;
  font-weight: 500;
}
.flag{
  height: 40px;
}
#world{
  padding: 20px 0;
}
select {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    padding: 10px 15px;
    width: 100%;
    background-color: transparent;
    border: 1px #162388 solid;
    border-radius: 10px;
    position: relative;
    background-image: url('https://upload.wikimedia.org/wikipedia/commons/9/9d/Arrow-down.svg');
    background-repeat: no-repeat;
    -webkit-background-size: 20px;
    background-size: 20px;
    font-size: 18px;
    font-weight: 500;
    background-position: right 10px center;
    margin-bottom: 20px;
}

select::-ms-expand {
    display: none;
}

/* Remove outline on the forms and links */
:active, :hover, :focus {
outline: 0;
outline-offset: 0;
}
</style>





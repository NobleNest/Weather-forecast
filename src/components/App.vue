<template>
    <div>
        <input type = "text" v-model = "newCity">
        <button class = "btn btn-send" v-on:click = "add()">Add</button>
        <select v-model = "city">
            <option v-for="item in cityArr" v-bind:key="item">{{ item }}</option>
        </select>
        <button class = "btn btn-send" v-on:click = "getWeather()">Get Weather</button>
        <h3 v-if = "arr.city">Weather forecast for {{ arr.city.name}} -- {{ arr.city.country }} -- [{{ arr.city.coord.lat }},{{ arr.city.coord.lon }}]</h3>
        <table class ="table table-dark" v-for="item in arr.list" v-bind:key="item.dt">
            <tr>
                <td>Time: {{ item.dt_txt }}</td>
                <td>Wind: {{ item.wind.speed }} m/sec</td>
                <td>Temp: {{ item.main.temp }} k</td>
                <td>Feels like: {{ item.main.feels_like }} k</td>
            </tr>
            <tr>
                <td>Main: {{ item.weather[0].main }}</td>
                <td>Description: {{ item.weather[0].description }}</td>
                <td>Humidity: {{ item.main.humidity }}</td>
                <td>Pressure: {{ item.main.pressure }} hpa</td>
            </tr>
        </table>
    </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
       data: function() {
           return {
               arr:[],
               api: "d553452f5b9aaaf83281e87887427dbf",
               city:'',
               newCity:"",
               cityArr: ["London","Paris"],
               latitude:'',
               longitude:''
           };
        },
        mounted: function(){
            this.getLocation()
            if(this.latitude == ""){
                alert('Геолокация не работает из предоставленного апи в хроме версии 50+ при незащищённом соединении, а у меня 86')
            }
            axios.get("http://api.openweathermap.org/data/2.5/forecast?lat="+ this.latitude + "&lon=" + this.longitude + "&appid=d553452f5b9aaaf83281e87887427dbf").then((response) =>{
                console.log(response.data);
                this.arr = response.data;
            })
        },
        methods: {
            getWeather:function(){
                axios.get("http://api.openweathermap.org/data/2.5/forecast?q=" + this.city + "&appid=d553452f5b9aaaf83281e87887427dbf").then((response) =>{
                    console.log(response.data);
                    this.arr = response.data;
                })
            },
            add:function(){
                this.cityArr.push(this.newCity)
            },
            getLocation: function(){
                if (navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(this.showPosition);
                } else {
                    alert("Geolocation is not supported by this browser.")
                }
            },
            showPosition: function(position){
                this.latitude =  position.coords.latitude
                this.longitude =  position.coords.longitude;
            }
        }
    }
</script>
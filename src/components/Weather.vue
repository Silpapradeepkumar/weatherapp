<template>
    <div class="container p-0">
        <div class="d-flex">
            <div class="card main-div w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">Today</h2>
                    <p class="text-dark date mb-0">{{date}}</p>
                    <small>{{time}}</small>
                    <h2 class="place"><i class="fa fa-location">{{ name }}<small>{{ country }}</small></i></h2>
                    <div class="temp">
                        <h1 class="weather-temp">{{ temperature }}&deg;</h1>
                        <h2 class="text-dark">{{ description }} <img :src="iconURL"> </h2>
                    </div>
                </div>
            </div>
            <div class="card card-2 w-100">
            <table class="m-4">
                <tbody>
                    <tr>
                        <th>1AM</th>
                        <td>  {{ temperature }}</td>
                    </tr>
                    <tr>
                        <th>2AM</th>
                        <td>{{ temperature }}</td>
                        
                    </tr>    
                    <tr>
                        <th>3AM</th>
                        <td>{{ temperature }}</td>
                    </tr>    
                    <tr>
                        <th>4AM</th>
                        <td>{{ temperature }}</td>
                    </tr>
                        
                        
                </tbody>
            </table>
            <Daysweather></Daysweather>

            
        </div>
        </div>
      
    </div>
   </template>
   
   <script>
   import axios from 'axios';
   import Daysweather from './Daysweather.vue';
   
   export default(await import('vue')).defineComponent( {
        name: 'myWeather',
        components:{
            Daysweather,
        }, 
        props:{
            city: String,
        },
        data(){
            return{
                temperature: null,
                description: null,
                iconURL: null,
                date: null,
                time: null,
                name: null,
                monthNames: ["January","February","March","April","May","June","July","August","September","October","November","December"],
                country: null,
                

            }
        },
        async created() {
             const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=8331cdef4f10633c84fd856ce65588b0`;
             console.log('URL:', url);
            try {
                const response = await axios.get(url);
                const weatherData= response.data;
                this.temperature = Math.round( weatherData.main.temp);
                this.description = weatherData.weather[0].description;
                this.name = weatherData.name;
                this.country = weatherData.sys.country;
                

                this.iconURL = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
                const d = new Date();
                this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear(); 
                this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
                console.log(weatherData);
            } catch (error) {
                console.error('Error:', error.response);
            }
            },

   })
   </script>
   
   <style>

        body{
            background-color: #343d4b;
        }
        .weather-temp{
            margin:0;
            font-weight: 700;
            font-size: 4em;
        }
        h2.mb-1.day{
            font-size: 3rem;
            font-weight: 400;
        }
        .main-div{
            border-radius: 20px;
            color: #1b0808;
            
            background-image: url("https://images.unsplash.com/photo-1702341562813-43a511584d19?q=80&w=1887&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D");
            background-size: cover;
            background-position: center;
            /* background-blend-mode: overlay; */
            
            background-repeat: no-repeat;
        }
        
        .temp{
            position: absolute;
            bottom: 0;
        }
        .main-div:hover{
            transform: scale(1.1);
            transition: transform 0.5s ease;
            z-index: 1;
        }
        .card-2{
            background-color: #01060d;
            border-radius: 20px;
            background-image: url("https://images.unsplash.com/photo-1702341562813-43a511584d19?q=80&w=1887&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D");
            background-size: cover;
            background-position: center;
        }
        .card-details{
            margin-left: 19px;
        }
        .h1_left{
            position: absolute;
            bottom: 25px;
            left: 16px;
            font-size: 3vw;
            line-height: 1.2;

        }
        .h3_left{
            position: absolute;
            left: 16px;
            font-size: 2vw;
            line-height: 0.5;
            
        }
        .h3_left small{
            font-size: 1rem;
        }
        table{
            position: relative;
            left: 15px;
            border-collapse: separate;
            border-spacing: 15px;
            width: 85%;
            text-align: left;
            max-width: 600px;
            margin: 0 auto;
        }
        th{
            font-size: 18px;
            color: #01060d;
            
        }
        td{
            font-size: 18px;
            color: #01060d;
           
        }
        
        


   </style>
   
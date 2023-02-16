<template>
    <div class="container">
        <div class="card">
            <div class="location">
                <p>{{ location.name }}</p>
                <p class="secondary-text">{{ location.tz_id }}</p>
            </div>
            <p>Feels Like <strong>{{ current.feelslike_c }}°</strong></p>
            <div class="temperature">
                <img :src="current.condition.icon" alt="">
                <p>
                    {{ current.temp_c }}
                </p>
            </div>

            <p class="weather">
                {{ current.condition.text }}
            </p>

            <hr>

            <div class="condition-wrapper">
                <div class="condition">
                    <img src="https://uxwing.com/wp-content/themes/uxwing/download/weather/wind-icon.png" alt="">
                    <p>{{ current.gust_mph }}</p>
                </div>
                <div class="condition">
                    <img src="https://cdn-icons-png.flaticon.com/512/727/727790.png" alt="">
                    <p>{{ current.humidity }}</p>
                </div>
                <div class="condition">
                    <img src="https://cdn-icons-png.flaticon.com/512/14/14078.png" alt="">
                    <p>{{ current.cloud }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'WeatherCard',
    data() {
        return {
            location: {},
            current: {
                condition: {}
            },
            // position: {
            //     lat: 0,
            //     long: 0
            // }
        }
    },
    mounted() {
        this.getWeather()
    },
    methods: {
        getWeather() {
            navigator.geolocation.getCurrentPosition(position => {
                let lat, long

                lat = position.coords.latitude
                long = position.coords.longitude

                axios.get(`http://api.weatherapi.com/v1/current.json?key=12d64dc31f794d6aaca170154231502&q=${lat},${long}`)
                    .then((response) => {
                        console.log(response.data)
                        this.location = response.data.location
                        this.current = response.data.current
                    })
                    .catch((error) => {
                        console.log(error)
                    })
            });

        }
    }
}
</script>

<style scoped>
.secondary-text {
    color: gray;
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: url('https://images.pexels.com/photos/531756/pexels-photo-531756.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1');
    background-repeat: no-repeat;
    background-size: cover;
}

.card {
    display: flex;
    flex-direction: column;
    gap: 2rem;
    background: rgb(235, 235, 235);
    border-radius: 20px;
    width: 20rem;
    padding: 2rem;

    box-shadow: 0 0 5rem rgb(100, 100, 100);
}

.location {
    display: flex;
    justify-content: space-between;
    border: 1px solid grey;
    border-radius: 10rem;
    padding: 1rem;

}

.temperature {
    background: white;
    width: 10rem;
    height: 10rem;
    border-radius: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    position: relative;
}

.temperature>img {
    position: absolute;
    top: -1rem;
    left: -2rem;
    width: 100px;
}

.temperature>p {
    position: relative;
    font-size: 4rem;
    font-weight: bold;
    color: black;
}

.temperature>p::after {
    position: absolute;
    content: '°';
    top: -0.5rem;
    font-size: 3rem;
}

.condition-wrapper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}

.condition {
    display: flex;
    justify-content: center;
    gap: 10px;
}

.condition>img {
    width: 1rem;
    height: 1rem;
}
</style>
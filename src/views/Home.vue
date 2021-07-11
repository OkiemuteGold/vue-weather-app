<template>
    <div class="home">
        <div class="container" :class="[isWarm ? 'warm' : '']">
            <main>
                <h1 class="title">Weather App</h1>
                <div class="search-box">
                    <input
                        type="text"
                        class="search-bar"
                        placeholder="Enter country or city..."
                        aria-roledescription="city country or zip input"
                        v-model="query"
                        @keypress.enter="fetchWeather"
                    />
                    <i
                        class="fas fa-search-location"
                        title="search weather"
                        aria-roledescription="search icon"
                        role="search weather"
                        @click.prevent="fetchWeather"
                        v-show="query !== ''"
                    ></i>
                </div>

                <!-- <div class="errorMessage"></div> -->

                <div class="weather-wrap" v-if="isValid">
                    <div class="location-box">
                        <div class="location">
                            <i class="fas fa-map-marker-alt"></i>
                            {{ weather.name }},
                            {{ weather.sys.country }}
                        </div>
                        <div class="date">
                            <time datetime="">
                                {{ dateBuilder() }}
                            </time>
                        </div>
                    </div>

                    <div class="weather-box">
                        <div class="temp">{{ roundTemp() }}&deg;C</div>
                        <div class="weather-description-container">
                            <img
                                :src="iconSrc"
                                :alt="iconAlt"
                                width="90px"
                                height="90px"
                            />
                            <div class="weather">
                                <span>{{ weather.weather[0].main }}</span>
                                <p class="description">
                                    ...{{ weather.weather[0].description }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>
</template>

<script>
// import { mapState } from "vuex";
import days from "../data/days";
import months from "../data/months";

export default {
    name: "Home",
    computed: {
        isValid() {
            return typeof this.weather.main != "undefined";
        },
        isWarm() {
            return this.isValid && this.weather.main.temp > 18;
        },

        // ...mapState(["days", "months"]),
    },
    data() {
        return {
            // "http://api.openweathermap.org/data/2.5/forecast?id=524901&appid={API key}"

            // "https://api.openweathermap.org/data/2.5/onecall?lat={lat}&lon={lon}&exclude={part}&appid={API key}"

            // params: {
            //     forecast: "forecast",
            //     road_risk: "roadrisk",
            // },

            api_key: "55facb388257da5750113f4404c6dd36",
            url_base: "https://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {},
            unit: "metric",
            searchMethod: "",
            iconSrc: "",
            iconAlt: "",
            days,
            months,
        };
    },
    methods: {
        userSearchMethod() {
            let city = this.query;
            if (city.length === 5 && Number.parseInt(city) + "" === city) {
                this.searchMethod = "zip";
            } else {
                this.searchMethod = "q";
            }
            return this.searchMethod;
        },

        fetchWeather() {
            this.userSearchMethod(this.query);
            fetch(
                `${this.url_base}weather?${this.searchMethod}=${this.query}&units=${this.unit}&APPID=${this.api_key}`
            )
                .then((resp) => {
                    return resp.json();
                })
                // .then(this.setResults);
                .then((results) => {
                    // console.log(results);
                    this.weather = results;
                })
                .catch(this.showError);

            let icon = this.weather.weather[0].icon;
            this.iconSrc =
                "https://openweathermap.org/img/wn/" + icon + "@2x.png";

            this.iconAlt = `${this.weather.weather[0].main} icon`;

            this.query = "";
        },
        // setResults(results) {
        //     this.weather = results;
        // },

        showError(err) {
            console.log(err.message);
        },

        roundTemp() {
            return Math.round(this.weather.main.temp);
        },

        dateBuilder() {
            let d = new Date();

            let day = this.days[d.getDay()];
            let month = this.months[d.getMonth()];
            let year = d.getFullYear();
            let currentDate = d.getDate();

            return `${day} ${currentDate} ${month} ${year}`;
        },
    },
    mounted() {
        // console.log(this.$store.state.days);
        // console.log(this.days);
    },
    beforeMount() {
        this.fetchWeather;
    },
};
</script>

<style scoped>
.home {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 100vw;
}
.container {
    transition: 0.4s;
    max-width: 700px;
    margin: 35px auto 0;
    background-image: linear-gradient(
            to bottom,
            rgba(0, 0, 0, 0.18),
            rgba(0, 0, 0, 0.7)
        ),
        url("../assets/images/cold-bg.jpg");
    background-size: cover;
    background-position: center;
    border-radius: 3px;
}

.title {
    margin-bottom: 30px;
    color: var(--text-color);
    /* font-family: "lato", sans-serif; */
    text-shadow: 2px 2px rgba(0, 0, 0, 0.25);
}

main {
    padding: 30px 60px;
    min-height: 550px;
    max-width: 500px;
    text-align: center;
}

.container.warm {
    background-image: url("../assets/images/warm-bg.jpg");
}

.search-box {
    width: 100%;
    margin-bottom: 20px;
    position: relative;
}

.search-box .search-bar {
    display: block;
    width: 100%;
    padding: 10px 15px;
    color: #2c3e50;
    font-size: 16px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.85);
    border-radius: 0px 12px 0px 12px;
    transition: 0.4s;
}

.search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    /* background-color: rgba(255, 255, 255, 0.75); */
    border-radius: 12px 0px 12px 0px;
}

.search-box i {
    position: absolute;
    top: 8px;
    right: 8px;
    padding: 5px;
    font-size: 16px;
    text-shadow: 1.2px -1.1px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: 0.4s;
}

.location-box {
    padding: 8px;
}

.location-box .location {
    color: var(--text-color);
    font-size: 32px;
    font-weight: 500;
    max-width: 290px;
    text-shadow: 1.7px 1.5px rgba(0, 0, 0, 0.25);
    margin-bottom: 5px;
}

.location i {
    font-size: 20px;
    position: relative;
    color: #2c3e50;
    top: -4px;
    animation: translate 0.55s linear 0s alternate infinite;
}

@keyframes translate {
    0% {
        top: 0px;
    }
    25% {
        top: -1px;
    }
    50% {
        top: -2px;
    }
    75% {
        top: -3px;
    }
    100% {
        top: -4px;
    }
}

.location-box .date {
    color: #f2f2f2;
    font-size: 16px;
    font-weight: 300;
    font-style: italic;
}

.weather-box {
    color: var(--text-color);
}

.weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    font-size: 100px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 12px;
    margin: 25px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-description-container {
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}

.weather-description-container img {
    max-width: 100%;
    height: auto;
}

.weather-box .weather {
    font-size: 45px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    flex: 2;
}

.description {
    font-size: 16px;
    font-weight: 300;
    font-style: italic;
    color: #f2f2f2;
    margin-left: 20px;
}

@media screen and (max-width: 425px) {
    main {
        padding: 25px 15px;
    }
    .weather-box .temp {
        padding: 10px 20px;
        font-size: 92px;
    }
}
</style>

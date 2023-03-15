<script>

    import './styles.css';
    let cityName = '';
    const key = 'dKpQaxIGkfaAPAz6d26mBDJfMlnzZbZR';


    const today = new Date();
    const date = today.getMonth()+'-'+(today.getDate()+'-'+today.getFullYear());
    const time = today.getHours() + ":" + today.getMinutes();
    const dateTime = 'Current DateTime: '+date+' | '+time;


    // get weather information
    async function getWeather(id) {
        const base = 'http://dataservice.accuweather.com/currentconditions/v1/';
        const query = `${id}?apikey=${key}`;

        const response = await fetch(base + query);
        const data = await response.json();
        // console.log(data[0])
        return data[0];
    };

    // get city info
    async function getCity(cityName) {
        const base = 'http://dataservice.accuweather.com/locations/v1/cities/search';
        const query = `?apikey=${key}&q=${cityName}`;

        const response = await fetch(base + query);
        const data = await response.json();
        // console.log(data[0].Key)
        // getWeather(data[0].Key)
        return data[0];
    };

    async function loadData(cityName) {
        const citykey = await getCity(cityName);
        const weather_data = await getWeather(citykey.Key);
        // console.log(weather_data)
        return weather_data;
    };

    let promise = loadData(cityName)

    function handleClick() {
        promise = loadData(cityName)
        console.log(promise)
    }


</script>

<!--<Forecast />-->
<h1>Weather App</h1>


<h2>Enter city or zip code below</h2>

<div class="search-bar">
<form class="location-weather">
    <input type="text" name="cityName" placeholder="enter a city name or zip code" bind:value={cityName} />
        <button type="reset" on:click={handleClick}>Submit</button>
</form>

</div>
<p class="time">{dateTime}</p>



{#await promise}

    <p>Loading...</p>
{:then response}
    <div class="weather-block">
    <p class="city-name">{cityName[0].toUpperCase() + cityName.substring(1)}</p>
    <p class="degrees">{response.Temperature.Imperial.Value}&#8457;</p>
    <p>
        <img src="/images/{response.WeatherIcon}.png" alt="weather icon"/>
    <p class="weather-text">{response.WeatherText}</p>

    <p></p>
    <p></p>
    </div>
    {:catch error}
        <!-- <p>Error: {error.message}</p> -->
{/await}


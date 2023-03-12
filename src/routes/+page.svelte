<script>

    import './styles.css';
    let cityName = '';
    let data = undefined;
    const key = 'dKpQaxIGkfaAPAz6d26mBDJfMlnzZbZR';



    // get weather information
    const getWeather = async (id) => {
        const base = 'http://dataservice.accuweather.com/currentconditions/v1/';
        const query = `${id}?apikey=${key}`;

        const response = await fetch(base + query);
        const data = await response.json();
        // console.log(data[0])
        return data[0];
    };

    // get city info
    const getCity = async (cityName) => {
        const base = 'http://dataservice.accuweather.com/locations/v1/cities/search';
        const query = `?apikey=${key}&q=${cityName}`;

        const response = await fetch(base + query);
        const data = await response.json();
        // console.log(data[0].Key)
        // getWeather(data[0].Key)
        return data[0];
    };

    const loadData = async (cityName) => {
        const citykey = await getCity(cityName);
        const weather_data = await getWeather(citykey.Key);
        console.log(weather_data)
        return weather_data;
    }


    // console.log(loadData())

    // getCity(cityName).then(data => {
    //     return getWeather(data.Key)
    // }).then(data => {
    //     console.log(data);
    // }).catch(err => console.log(err));
    //
    // const updateCity = async (cityName) => {
    //     const cityDets = await getCity(cityName);
    //     console.log(cityDets)
    //     const weather = await getWeather(cityDets.Key);
    //
    //     return {
    //         cityDets: cityDets,
    //         weather: weather
    //     };
    // };
    // updateCity(cityName).then(data => console.log(data))
    // .catch(err => console.log(err));

//
// const result = getWeather(getCity(cityName))
//
// console.log(result)
</script>

<!--<Forecast />-->
<h1>Weather App</h1>


<h2>Weather Information for {cityName || 'city not entered yet'}</h2>



<form class="location-weather">
    <input type="text" name="cityName" placeholder="enter a city name" bind:value={cityName} />
        <button type="reset" on:click={loadData(cityName)}>Submit</button>




</form>

<h3>Data</h3>

{#await loadData()}
    <p>Loading...</p>
{:then loadData}
    <p>{loadData}</p>
    {:catch error}
    <p>Error: {error.message}</p>
{/await}
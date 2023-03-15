<script>

    import '../styles.css';
    let cityName = '';
    const key = 'dKpQaxIGkfaAPAz6d26mBDJfMlnzZbZR';

</script>


<!--<Forecast />-->
<h1>Weekly Weather</h1>


<h2>Enter city or zip code below</h2>
<div class="search-bar">
<form class="location-weather">
    <input type="text" name="cityName" placeholder="enter a city name or zip code" bind:value={cityName} />
    <button type="reset" on:click={handleClick}>Submit</button>

</form>
</div>
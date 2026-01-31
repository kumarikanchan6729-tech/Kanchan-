# Kanchan-
const condition = currentData.weather[0].main.toLowerCase();
changeBackground(condition);
if (!currentResponse.ok) {
    alert("City not found!");
    return;
}
if (!city) {
    alert("Please enter a city name");
    return;
}
const forecast = forecastData.list[index * 8];
if (!forecast) return;
async function getWeather() {
    const city = document.getElementById('city').value;
    if (!city) {
        alert("Please enter a city name");
        return;
    }

    const apiKey = 'YOUR_API_KEY';
    const currentWeatherUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
    const forecastWeatherUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${apiKey}&units=metric`;

    try {
        const currentResponse = await fetch(currentWeatherUrl);
        if (!currentResponse.ok) {
            alert("City not found!");
            return;
        }
        const currentData = await currentResponse.json();

        document.getElementById('cityName').textContent = currentData.name;
        document.getElementById('temperature').textContent =
            `Temperature: ${currentData.main.temp}°C`;
        document.getElementById('description').textContent =
            currentData.weather[0].description;

        const currentIcon = currentData.weather[0].icon;
        document.querySelector('.current-weather .icon').innerHTML =
            `<img src="https://openweathermap.org/img/wn/${currentIcon}@2x.png">`;

        // 🔥 Change background
        changeBackground(currentData.weather[0].main.toLowerCase());

        // Forecast
        const forecastResponse = await fetch(forecastWeatherUrl);
        const forecastData = await forecastResponse.json();

        const forecastDays = document.querySelectorAll('.day');
        forecastDays.forEach((day, index) => {
            const forecast = forecastData.list[index * 8];
            if (!forecast) return;

            const weekday = new Date(forecast.dt_txt)
                .toLocaleDateString('en-US', { weekday: 'long' });

            day.querySelector('.weekday').textContent = weekday;
            day.querySelector('.icon').innerHTML =
                `<img src="https://openweathermap.org/img/wn/${forecast.weather[0].icon}@2x.png">`;
            day.querySelector('.temp').textContent =
                `${Math.round(forecast.main.temp)}°C`;
        });

    } catch (error) {
        console.error("Error:", error);
        alert("Something went wrong!");
    }
}
const apiKey = "YOUR_API_KEY_HERE"; // replace with your OpenWeatherMap API key

async function getWeather() {
    const city = document.getElementById("city").value;

    if (!city) {
        alert("Please enter a city name");
        return;
    }

    try {
        // Current weather
        const currentResponse = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`
        );
        const currentData = await currentResponse.json();

        if (currentData.cod !== 200) {
            alert("City not found");
            return;
        }

        document.getElementById("cityName").innerText = currentData.name;
        document.getElementById("temperature").innerText =
            `${Math.round(currentData.main.temp)}°C`;
        document.getElementById("description").innerText =
            `Description: ${currentData.weather[0].description}`;

        // Forecast (4 days)
        const forecastResponse = await fetch(
            `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`
        );
        const forecastData = await forecastResponse.json();

        const days = document.querySelectorAll(".forecast .day");

        for (let i = 0; i < days.length; i++) {
            const data = forecastData.list[i * 8]; // every 24 hours
            const date = new Date(data.dt_txt);

            days[i].querySelector(".weekday").innerText =
                date.toLocaleDateString("en-US", { weekday: "short" });
            days[i].querySelector(".temp").innerText =
                `${Math.round(data.main.temp)}°C`;
        }

    } catch (error) {
        console.error(error);
        alert("Something went wrong. Try again later.");
    }
}
*{margin:0;padding:0;box-sizing:border-box}

body{
    font-family:Poppins,sans-serif;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
    background-size:cover;
    background-position:center;
}

/* Weather backgrounds */
body.clear{background-image:url('clear.jpg')}
body.clouds{background-image:url('clouds.jpg')}
body.rain{background-image:url('rain.jpg')}
body.snow{background-image:url('snow.jpg')}
body.default{background-image:url('default.jpg')}

.app-container{
    background:rgba(34,34,34,.8);
    padding:20px;
    border-radius:15px;
    width:90%;
    max-width:600px;
    text-align:center;
    box-shadow:0 6px 20px rgba(0,0,0,.5);
}

h1{color:#ff8c00;font-size:2.5rem}
p{color:#aaa}

.search-container{
    display:flex;
    gap:10px;
    margin:20px 0;
}

input{
    flex:1;
    padding:10px;
    border:none;
    border-radius:25px;
    background:#444;
    color:#fff;
}

button{
    padding:10px 20px;
    border:none;
    border-radius:25px;
    background:#ff8c00;
    color:#fff;
    cursor:pointer;
}

button:hover{background:#cc6b00}

.current-weather,.day{
    background:#333;
    padding:15px;
    border-radius:15px;
    box-shadow:0 4px 15px rgba(0,0,0,.3);
}

.current-weather{
    display:flex;
    gap:20px;
    align-items:center;
    margin-bottom:20px;
}

.forecast{
    display:grid;
    grid-template-columns:repeat(4,1fr);
    gap:15px;
}

.weekday{color:#ff8c00;font-weight:bold}

@media(max-width:600px){
    .forecast{grid-template-columns:repeat(2,1fr)}
}

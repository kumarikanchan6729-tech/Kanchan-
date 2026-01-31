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

# API-INTEGRATION-AND-DATA-VISUALIZATION

*COMPANY*: CODTECH IT SOLUTIONS

*NAME*: SANKET ANIL SHELAR

*INTERN ID*: CTO4DH1987

*DOMAIN*: PYTHON PROGRAMMING

*DURATION*: 4 WEEKS

*MENTOR*: NEELA SANTOSH KUMAR


# API-INTEGRATION-AND-DATA-VISUALIZATION

import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# === CONFIGURATION ===
API_KEY = "7b747e2f7c8feafface47df1b2da7a6a" 
CITY = "Titwala"
UNITS = "metric"
URL = f"https://api.openweathermap.org/data/2.5/forecast?q={CITY}&appid={API_KEY}&units={UNITS}"

# === FETCH DATA ===
response = requests.get(URL)
if response.status_code != 200:
    print("Failed to fetch forecast data. Please check API key or city name.")
    exit()
data = response.json()

# === PARSE DATA ===
forecast_list = data["list"]
df = pd.DataFrame({
    "datetime": [item["dt_txt"] for item in forecast_list],
    "Temperature (°C)": [item["main"]["temp"] for item in forecast_list],
    "Humidity (%)": [item["main"]["humidity"] for item in forecast_list],
    "Wind Speed (m/s)": [item["wind"]["speed"] for item in forecast_list],
    "Condition": [item["weather"][0]["main"] for item in forecast_list]
})
df["datetime"] = pd.to_datetime(df["datetime"])

# === MAP EMOJIS ===
emoji_map = {
    "Clear": "☀️", "Clouds": "☁️", "Rain": "🌧️",
    "Snow": "❄️", "Thunderstorm": "⛈️", "Mist": "🌫️"
}
df["Emoji"] = df["Condition"].map(emoji_map).fillna("🌈")

# === PLOT ALL METRICS TOGETHER ===
sns.set_style("whitegrid")
plt.figure(figsize=(14, 6))
sns.lineplot(data=df, x="datetime", y="Temperature (°C)", label="Temperature (°C)", marker="o", color="tomato")
sns.lineplot(data=df, x="datetime", y="Humidity (%)", label="Humidity (%)", marker="s", color="royalblue")
sns.lineplot(data=df, x="datetime", y="Wind Speed (m/s)", label="Wind Speed (m/s)", marker="^", color="green")

# === FINAL TOUCHES ===
plt.title(f"Weather Forecast Trends for {CITY}", fontsize=16)
plt.xlabel("Date & Time")
plt.ylabel("Values")
plt.xticks(rotation=45)
plt.legend()
plt.tight_layout()
plt.savefig("weather_forecast_with_emojis.png", dpi=300)
plt.show()

## OUTPUT

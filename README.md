# Weather-ETL-AI-Text-Generation-Pipeline-using-Microsoft-Fabric-and-Telegram-Bot



Project Overview

This project demonstrates how to:

Extract live weather data from an API
Transform and clean the data using PySpark
Store data in Bronze & Silver Delta tables
Generate human-readable weather summaries using an LLM
Send generated insights directly to Telegram

The project follows a simple Medallion Architecture approach inside Microsoft Fabric.

Technologies Used
Python
PySpark
Microsoft Fabric
Delta Lake
OpenWeather API
Hugging Face Transformers
FLAN-T5 Base
Telegram Bot API
Pipeline Architecture
OpenWeather API
        ↓
   Raw JSON Data
        ↓
 Bronze Layer (weather_bronze)
        ↓
 Data Cleaning & Transformation
        ↓
 Silver Layer (weather_silver)
        ↓
 Hugging Face FLAN-T5 Model
        ↓
 AI Generated Weather Report
        ↓
 Telegram Bot Notification
Features
Data Extraction

Fetches live weather data using OpenWeather API.

Bronze Layer

Stores raw ingested weather data in Delta format.

Silver Layer

Performs transformations such as:

Temperature conversion (Kelvin → Celsius)
Data cleaning
Structured formatting
AI Text Generation

Uses Google's FLAN-T5 model to generate natural language weather reports.

Example:

City Alex has clear sky weather with a temperature of 18.7°C.
Telegram Integration

Automatically sends generated weather summaries to a Telegram chat using a bot.

Notebook Workflow
1. Extract Weather Data
description = data['weather'][0]['description']
temp = data['main']['temp']
pressure = data['main']['pressure']
humidity = data['main']['humidity']
2. Create Bronze Table
df = spark.createDataFrame(row)

df.write.format('delta') \
.mode('overwrite') \
.saveAsTable('weather_bronze')
3. Create Silver Table
from pyspark.sql.functions import *

df = df.withColumn('temp', col('temp') - 273)

df.write.format('delta') \
.mode('overwrite') \
.saveAsTable('weather_silver')
4. AI Weather Text Generation
from transformers import pipeline

generator = pipeline(
    "text2text-generation",
    model="google/flan-t5-base"
)
5. Generate Final Weather Report
input_text = f"""
city Alex has {description} weather
with a temperature of {temp}°C
"""

result = generator(input_text, max_length=100)
6. Send Result to Telegram
import requests

url = f"https://api.telegram.org/bot{access_token}/sendMessage"

requests.post(url, json={
    "chat_id": chat_id,
    "text": result
})
How to Run
1. Clone Repository
git clone https://github.com/yourusername/weather-etl-fabric.git
2. Open Microsoft Fabric Notebook

Upload the notebook into Microsoft Fabric.

3. Install Required Libraries
pip install transformers
pip install requests
4. Add Your API Keys

Replace:

access_token = "YOUR_TELEGRAM_BOT_TOKEN"

and

api_key = "YOUR_OPENWEATHER_API_KEY"
5. Run Notebook Cells Sequentially

Execute all notebook cells in order.



Data Engineering Enthusiast
AI & Machine Learning Learner
Microsoft Fabric Explorer

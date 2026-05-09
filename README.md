## Overview
This project is an automated Data Engineering pipeline built on Microsoft Fabric. It orchestrates an ETL process that extracts real-time weather data for Alexandria, Egypt, transforms it using PySpark, leverages a Large Language Model (LLM) to generate a natural language summary, and automatically broadcasts the update to a Telegram channel via a bot.

## Architecture & Tech Stack
Platform: Microsoft Fabric (Pipelines, Notebooks, OneLake)

Languages: Python, PySpark

Storage: Delta Lake (Medallion Architecture)

APIs: OpenWeatherMap API, Telegram Bot API

Machine Learning: Hugging Face transformers (google/flan-t5-base)

## Pipeline Workflow
1. Data Extraction (Ingestion)
Connects to the OpenWeatherMap API using the Python requests library to fetch current weather data (temperature, pressure, humidity, and weather description) in JSON format.

2. Bronze Layer (Raw Storage)
Extracts the target variables from the API response.

Creates a PySpark DataFrame from the raw data.

Writes the DataFrame to OneLake as a Delta table named weather_bronzee in overwrite mode.

3. Silver Layer (Transformation)
Reads from the Bronze layer.

Applies data transformations using PySpark, specifically converting the temperature from Kelvin to Celsius (col('temp') - 273).

Writes the cleaned data to a new Delta table named weather_silver.

4. AI Text Generation
Initializes a text-to-text generation pipeline using Hugging Face's transformers library.

Uses the google/flan-t5-base model to construct a human-readable sentence based on the extracted weather conditions and converted temperature.

5. Automated Notification (Telegram)
Takes the AI-generated text output.

Sends a POST request to the Telegram Bot API (/sendMessage endpoint) using the required Bot Access Token and Target Chat ID.

The final result is an automated message delivered straight to the configured Telegram channel.

## Prerequisites
To replicate or deploy this project, you will need:

A Microsoft Fabric workspace.

An OpenWeatherMap API Key.

A Telegram Bot Access Token (generated via BotFather).

The Chat ID of the target Telegram group or user.

## Configuration
Before running the pipeline, ensure the following variables are updated in the PySpark notebook:

appid in the OpenWeatherMap URL.

access_token for the Telegram Bot.

chat_id for the destination Telegram chat.

Execution
The entire process is automated and orchestrated via a Fabric Pipeline (Pipeline_1), which triggers the main notebook containing the ETL and API logic.

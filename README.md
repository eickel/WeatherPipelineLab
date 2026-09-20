# Weather Pipeline

An asynchronous weather data pipeline that cleans city coordinates, retrieves
hourly forecasts from the [Open-Meteo API](https://open-meteo.com/), compares
synchronous and asynchronous request times, and generates summary reports.

## Requirements

- Python 3.14 or newer
- [uv](https://docs.astral.sh/uv/)
- Internet access for the Open-Meteo API requests

## Setup

Install the project dependencies and create the virtual environment with:

```powershell
uv sync
```

## Run the Pipeline

From the project root, run:

```powershell
uv run python src/weather_pipeline/pipeline.py
```

The pipeline reads city data from `data/cities.csv`, normalizes city names,
fetches hourly temperature and precipitation data, and writes reports to the
`reports/` directory.

## Input Data

The input CSV must contain these columns:

```text
CityName,Lat,Lon
```

City names may contain extra whitespace, inconsistent capitalization, and
special characters. The cleaning step removes special characters and applies
title case before making API requests.

## Configuration

Optional settings can be provided in a `.env` file:

| Variable | Default | Description |
| --- | --- | --- |
| `LOG_LEVEL` | `INFO` | Logging level |
| `CHUNK_SIZE` | `100` | Number of records processed per chunk |
| `ALERT_THRESHOLD` | `30.0` | Maximum temperature threshold in Celsius |
| `MAX_RETRIES` | `3` | Number of API request attempts |

## Generated Reports

- `reports/weather_summary.xlsx` contains daily maximum temperatures and total precipitation by city.
- `reports/alerts.json` contains cities whose daily maximum temperature exceeds `ALERT_THRESHOLD`.
- `pipeline-trace.log` contains execution and performance details.

## Run Tests

```powershell
uv run pytest
```

Run the linter with:

```powershell
uv run ruff check .
```
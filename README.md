# aws-smsl-predict-airquality-via-weather
A Jupyter Notebook that connects to Amazon Sustainability Data Initiative (ASDI) datasets from NOAA and OpenAQ to build a Machine Learning (ML) model to predict air quality via weather.

## 1 out of 8 deaths in the world is due to poor air quality
This notebook explores correlations between weather and air quality since we know factors like temporatures, wind speeds, dew points, etc, affect certain air quality parameters. Predicting air quality based on weather can get into highly sophisticated ML techniques, but this demo merges NOAA GSOD weather data with OpenAQ air quality data to build an ML model using AutoGluon (AutoML from AWS). The model is ~75-85% accurate at predicting Particulate Matter 2.5microns (pm25) based on a subset of weather inputs in the Los Angeles, CA, and Las Vegas, NV.


## Amazon Sustainability Data Initiative (ASDI) Datasets Used
ASDI Homepage: https://sustainability.aboutamazon.com/environment/the-cloud/asdi \
ASDI Datasets: https://registry.opendata.aws/collab/asdi/

- **NOAA GSOD**\
  Consists of daily weather summaries from various NOAA weather stations.\
  Dataset URL : https://registry.opendata.aws/noaa-gsod/
  - Example Weather Parameters (features)
    - MAX: Maximum temperature (.1 Fahrenheit)
    - MIN: Minimum temperature (.1 Fahrenheit)
    - WDSP: Mean wind speed (.1 knots)
    - DEWP: Mean dew point (.1 Fahrenheit)
    - PRCP: Precipitation amount (.01 inches)
- **OpenAQ**\
  Consists of air quality parameter readings that can be queried as daily averages.\
  Dataset URL : https://registry.opendata.aws/openaq/
  - Main Air Quality Parameters (target labels)
    - Particulate Matter 2.5microns (PM2.5)
    - Particulate Matter 10.0microns (PM10)
    - Ozone (O3)
    - Sulfur Dioxide (SO2)
    - Nitrogen Dioxide (NO2)
    - Carbon Monoxide (CO)

## AWS SageMaker Studio Lab
You can sign up for SageMaker Studio Lab and use it for free without an AWS account. You can run for 4 hours with GPU or 12 hours with CPU and then logout and log back in for another session. Your data and notebooks are persisted. After clicking the launch button below, choose "download whole repo" and then "build conda environment" when prompted.

When it's done installing and configuring the conda environment, open the "Nexrad_Demo.ipynb" notebook. Click-Enter to run each row and wait a moment to see the results of each line before proceeding to the next. The line marker should change to a number when it's successfully run that line, ie "[5]" means that it has run line 5.

## Security

See CONTRIBUTING for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

# aws-smsl-predict-airquality-via-weather
A Jupyter Notebook that connects to Amazon Sustainability Data Initiative (ASDI) datasets from NOAA and OpenAQ to build a Machine Learning (ML) model to predict air quality levels using weather data via a Binary Classification Autogluon model. This Notebook is NOT for scientific or health purposes.

This Jupyter Notebook can be run using Amazon SageMaker Studio Lab and open-source Amazon Sustainability Data Initiative (ASDI) datasets without needing an AWS account.

[![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/aws-samples/aws-smsl-predict-airquality-via-weather/blob/main/aq_by_weather.ipynb)

## PROBLEM: 1 out of 8 deaths in the world is due to poor air quality*
This notebook explores correlations between weather and air quality since we know factors like temperatures, wind speeds, etc, affect certain air quality parameters. Predicting air quality based on weather can get into highly sophisticated ML techniques, but this demo shows how merging NOAA GSOD weather data with OpenAQ air quality data to build an ML model using AutoGluon (AutoML from AWS) can result in prediction accuracy of ~75-90% using Binary Classification models for various high-pollution areas and air quality parameters (mostly tested for 2.5 micron Particulate Matter and some Ground Level Ozone).\
*Source: [OpenAQ.org](https://OpenAQ.org)

## DATA: ASDI Datasets for Weather and Air Quality
Learn more about the Amazon Sustainability Data Initiative (ASDI)...\
**ASDI Homepage:** https://sustainability.aboutamazon.com/environment/the-cloud/asdi \
**ASDI Datasets:** https://registry.opendata.aws/collab/asdi/

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
    - Particulate Matter 2.5 microns (PM2.5)
    - Particulate Matter 10.0 microns (PM10)
    - Ozone (O3)
    - Sulfur Dioxide (SO2)
    - Nitrogen Dioxide (NO2)
    - Carbon Monoxide (CO)

## PROCESS: Binary Classification Model via Amazon SageMaker Studio Lab
A specific Scenario is selected which includes a location (eg: Los Angeles) and an air quality parameter to target as our label to predict (eg: pm2.5). Users can customize the predefined Scenarios, add their own, and select the Scenario to use throughout the Notebook. Instructions are provided at the top of the Notebook.

NOAA GSOD daily summary weather data is merged with OpenAQ daily averages by date for a given OpenAQ parameter (ie: one of the six parameters listed above). Additionally, the corresponding US EPA healthy threshold value is used to convert the daily average air quality measurement into a binary classification of { 0=Healthy, 1=Unhealthy }.

AutoGluon is used to train a Binary Classification model using the merged dataset contain weather and air quality data.  Prediction accuracy observed ranged from ~75-90% for Particulate Matter 2.5 micron healthy vs unhealthy predictions and is largely dependent on how much data is available at the chosen location and that location's specific relationship between weather and air quality. Some additional testing was done using Ground Level Ozone around Los Angeles.  See the following list of US cities with high amounts of PM2.5 and Ozone pollution, if you'd like to add more Scenarios. Non-US locations are possible where data exists and the Notebook provides an example Scenario for Lahore, Pakistan. See: [American Lung Association: Most Polluted Cities](https://www.lung.org/research/sota/city-rankings/most-polluted-cities)

### Key Learnings
- The relationship between weather and air quality varies geographically and this was confirmed.
  - Feature Importance review shows differing factors in models built for different locations.
  - Academic research indicates the relationship can change by season and an engineered “MONTH” feature was important for most models.
  - An engineered “DAYOFWEEK” feature was expected to be important, but observed correlation was minimal.
- Predicting a target measurement was infeasible, but predicting a binary “healthy” vs “unhealthy” classification based on US EPA threshold values yielded decent results (~75-90% accuracy metrics).

### Using Amazon SageMaker Studio Lab
You can sign up for SageMaker Studio Lab and use it for free without an AWS account. You can use both GPU-based and CPU-based runtimes with free included local storage (limits apply). Your data and notebooks are persisted automatically across sessions. After clicking the launch button below, choose "copy notebook only" and then "build conda environment" when prompted (or use the provided _pip install_ commands in the notebook).

When it's done installing and configuring the conda environment, open the .ipynb notebook file. Run each row and wait a moment to see the results of each line before proceeding to the next. The line marker should change to a number when it's successfully run that line, ie "[5]" means that it has run line 5.

[![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/aws-samples/aws-smsl-predict-airquality-via-weather/blob/main/aq_by_weather.ipynb)

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
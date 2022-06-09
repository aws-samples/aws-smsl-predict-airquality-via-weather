# aws-smsl-predict-airquality-via-weather
A Jupyter Notebook that connects to Amazon Sustainability Data Initiative (ASDI) datasets from NOAA and OpenAQ to build a Machine Learning (ML) model to predict air quality via weather. This Jupyter Notebook can be run using Amazon SageMaker Studio Lab and open-source Amazon Sustainability Data Initiative (ASDI) datasets without needing an AWS account.

[![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/aws/studio-lab-examples/blob/main/natural-language-processing/NLP_Disaster_Recovery_Translation.ipynb)

## PROBLEM: 1 out of 8 deaths in the world is due to poor air quality (source: OpenAQ.org)
This notebook explores correlations between weather and air quality since we know factors like temperatures, wind speeds, etc, affect certain air quality parameters. Predicting air quality based on weather can get into highly sophisticated ML techniques, but this demo shows how merging NOAA GSOD weather data with OpenAQ air quality data to build an ML model using AutoGluon (AutoML from AWS) can result in prediction accuracy of ~75-85% using Binary Classification models for the Los Angeles, CA, and Las Vegas, NV areas for a target parameter of 2.5 micron Particulate Matter (PM2.5).

## DATA: Amazon Sustainability Data Initiative (ASDI) Datasets for Weather and Air Quality
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
    - Particulate Matter 2.5 microns (PM2.5)
    - Particulate Matter 10.0 microns (PM10)
    - Ozone (O3)
    - Sulfur Dioxide (SO2)
    - Nitrogen Dioxide (NO2)
    - Carbon Monoxide (CO)

## PROCESS: Binary Classification Model via Amazon SageMaker Studio Lab
A specific area to target is selected according to NOAA StationID and OpenAQ LocationID values (eg: Los Angeles, CA or Las Vegas, NV).  Users can customize the inputs for any location that has years worth of data that you find the corresponding IDs for (the limiting factor is usually how far back OpenAQ data exists).

The NOAA GSOD daily summary weather data is merged with OpenAQ daily averages by date for a given OpenAQ parameter (ie: one of the six parameters listed above). Additionally, the corresponding US EPA healthy threshold value is used to convert the daily average air quality measurement into a binary classification of { 0=Healthy, 1=Unhealthy }.

AutoGluon is used to train a Binary Classification model using the merged dataset contain weather and air quality data.  Prediction accuracies observed ranged from ~75-85% for Particulate Matter 2.5 micron healthy vs unhealthy predictions and is largely dependent on how much data is available at the chosen location and that location's specific relationship between weather and air quality.

### Key Learnings
- The relationship between weather and air quality varies geographically and this was confirmed.
  - Feature Importance review shows differing factors in models built for Los Angeles vs Las Vegas.
  - Academic research indicates the relationship can change by season and an engineered “MONTH” feature was important for most models.
  - I expected “DAYOFWEEK” to be an important feature, but observed correlation was minimal.
- Predicting a target measurement was infeasible, but predicting a binary “healthy” vs “unhealthy” classification based on US EPA threshold values yielded decent results (~80% accurate).

### Using Amazon SageMaker Studio Lab
You can sign up for SageMaker Studio Lab and use it for free without an AWS account. You can run for 4 hours with GPU or 12 hours with CPU and then logout and log back in for another session. Your data and notebooks are persisted. After clicking the launch button below, choose "download whole repo" and then "build conda environment" when prompted.

When it's done installing and configuring the conda environment, open the .ipynb notebook file. Click-Enter to run each row and wait a moment to see the results of each line before proceeding to the next. The line marker should change to a number when it's successfully run that line, ie "[5]" means that it has run line 5.

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
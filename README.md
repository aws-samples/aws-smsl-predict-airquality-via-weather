# ML Demo: Predicting Air Quality w/ ASDI NOAA + OpenAQ Datasets
This demo consists of a Jupyter Notebook that connects to Amazon Sustainability Data Initiative (ASDI) datasets from NOAA and OpenAQ to build a Machine Learning (ML) model to predict air quality levels using weather data via a Binary Classification [AutoGluon](https://auto.gluon.ai/stable/index.html) model. The project's purpose is to demonstrate using two different types of ASDI datasets (files in Amazon S3 and HTTPS APIs) within a Jupyter Notebook, such as provided by Amazon SageMaker Studio Lab. This demo is NOT for scientific or health purposes.

This Jupyter Notebook can be run for free using Amazon SageMaker Studio Lab and open-source Amazon Sustainability Data Initiative (ASDI) datasets without needing an AWS account.

[![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/aws-samples/aws-smsl-predict-airquality-via-weather/blob/main/aq_by_weather.ipynb)

## BACKGROUND: 1 out of 8 deaths in the world is due to poor air quality*
This demo explores correlations between weather and air quality since we know factors like temperatures, wind speeds, etc, affect certain air quality parameters. Predicting air quality can get into highly sophisticated ML techniques, but this demo shows how merging NOAA GSOD weather data with OpenAQ air quality data to build an ML model using [AutoGluon](https://auto.gluon.ai/stable/index.html) (AutoML from AWS) can result in prediction accuracy of ~75-90% using Binary Classification models for various high-pollution areas and air quality parameters (mostly tested for 2.5 micron Particulate Matter and some Ground Level Ozone).\
*Source: [OpenAQ.org](https://OpenAQ.org)

### Top 5 US Locations with Worst Air Quality
Source: https://www.lung.org/research/sota/city-rankings/most-polluted-cities
- By Particulate Pollution
  1. Bakersfield, CA
  2. Fresno-Madera-Hanford, CA
  3. Visalia, CA
  4. San Jose-San Francisco-Oakland, CA
  5. Los Angeles-Long Beach, CA
- By Ground Level Ozone
  1. Los Angeles-Long Beach, CA
  2. Bakersfield, CA
  3. Visalia, CA
  4. Fresno-Madera-Hanford, CA
  5. Phoenix-Mesa, AZ

## DATA: ASDI Datasets for Weather and Air Quality
Learn more about the Amazon Sustainability Data Initiative (ASDI)...\
**ASDI Homepage:** https://sustainability.aboutamazon.com/environment/the-cloud/asdi \
**ASDI Datasets:** https://registry.opendata.aws/collab/asdi/

- **National Oceanic and Atmospheric Administration: Global Surface Summary of the Day**\
  NOAA GSOD consists of daily weather summaries from various NOAA weather stations.\
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
This Notebook contains pre-defined scenarios that pair a physical location's weather (eg: Los Angeles) with an air quality parameter to predict (eg: PM2.5). Users can customize the pre-defined scenarios, add their own, and select the scenario to use throughout the Notebook via a drop-down list. Additional instructions are provided at the top of the Notebook.

NOAA GSOD daily summary weather data is merged with OpenAQ daily measurement averages by date for a given OpenAQ parameter (ie: one of the six parameters listed above). Additionally, corresponding US EPA unhealthy threshold values are used to convert the daily average air quality measurements into a binary classification of { 0=OKAY, 1=Unhealthy }.

AutoGluon [_TabularPredictor_](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html) is used to train a Binary Classification model using the merged dataset containing weather and air quality data.  Prediction accuracy observed ranged from ~75-90% for Particulate Matter 2.5 micron predictions of OKAY vs unhealthy and is largely dependent on how much data is available at the chosen location and that location's specific relationship between weather and air quality. Some additional testing was done using Ground Level Ozone around some of the pre-defined scenarios.  See the following list of US cities with high amounts of PM2.5 and Ozone pollution, if you'd like to add more scenarios. Non-US locations are possible where data exists and the Notebook provides an example scenario for Lahore, Pakistan. See: [American Lung Association: Most Polluted Cities](https://www.lung.org/research/sota/city-rankings/most-polluted-cities)

## SUMMARY...
This demo showed how to use a SageMaker Studio Lab Jupyter Notebook to connect to open-source Amazon Sustainability Data Initiative (ASDI) datasets both via Amazon S3 (NOAA weather data) and an HTTPS API (OpenAQ air quality averages). The NOAA weather data and OpenAQ measurement averages are merged into a single table with irrelevant features dropped. This table is then used to build an Autogluon [_TabularPredictor_](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html) ML model to predict if weather features would result in a 0=OKAY or 1=Unhealthy air quality target label.

### Key Learnings
- The relationship between weather and air quality varies geographically and this was confirmed.
  - Feature Importance review shows differing factors in models built for different locations.
  - Academic research indicates the relationship can change by season and an engineered “MONTH” feature was important for most models.
  - An engineered “DAYOFWEEK” feature was expected to be important, but observed correlation was minimal and removing the feature improved most models.
- Predicting a target measurement was infeasible, but predicting a binary “OKAY” vs “unhealthy” classification based on US EPA threshold values yielded decent results (~75-90% accuracy metrics).
- To learn more, research how climatology and deeper statistical analysis are merged into 3D models that incorporate emissions models, meteorological models, and chemical models.

### Using Amazon SageMaker Studio Lab
You can sign up for SageMaker Studio Lab and use it for free without an AWS account. You can use both GPU-based and CPU-based runtimes with free included local storage (limits apply). Your data and notebooks are persisted automatically across sessions. After clicking the launch button below, choose "clone entire repo" and then "build conda environment" when prompted (or use the provided _pip install_ commands in the notebook).

When it's done installing and configuring the conda environment, open the .ipynb notebook file. Run each cell and wait a moment to see the results of each line before proceeding to the next. The line marker should change to a number when it's successfully run that line, ie "[5]" means that it has run cell 5.

[![Open in SageMaker Studio Lab](https://studiolab.sagemaker.aws/studiolab.svg)](https://studiolab.sagemaker.aws/import/github/aws-samples/aws-smsl-predict-airquality-via-weather/blob/main/aq_by_weather.ipynb)

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
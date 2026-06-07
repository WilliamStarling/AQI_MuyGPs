# AQI_MuyGPs
an implementation of the MuyGPs toolkit to create a gaussian process to predict the air quality index. Instructions assume Windows OS. Initial datases aquired from https://aqs.epa.gov/aqsweb/airdata/download_files.html#AQI. Most of the work here is done in the aqi_preprocessing.ipynb and aqi_regression.ipynb files.

## Setup
1. First create and activate a virtual environment in the project directory, in a command prompt.
```
py -3.12 -m venv GP_env

GP_env\Scripts\activate
```
2. upgrade pip install
```
python -m pip install --upgrade pip
```
3. Install the required packages.
```
python -m pip install -r requirements.txt
```
4. Setup MuyGPs. there are multiple ways of doing this,available here: https://github.com/LLNL/MuyGPyS
I personally downloaded and built it from the source, with a copy of the repo in my project root, using the following command and flags ran from the project root.
```
python -m pip install -e MuyGPyS .[tests, docs]
```


## Project Structure
* <b>\data\\</b> - stores the datasets created and used.
    * <b>\daily_aqi_by_county_2003.csv</b> - This has the reported aqi for each county for each day, along with other info like the reporting site. This is used with aqs_sites.csv to generate training data with only coords and values.
    * <b>\aqs_sites.csv</b> - list of sites and their information, including gps coordinates. Used to build lists of datapoints with gps coords.   
    * <b>\annual_aqi__coords_scaled.csv</b> - equivalent to annual_aqi_coords, except the coordinates are scaled between 0 and 1.
    * <b>\biggest_day_aqi_coords_scaled.csv</b> - all reported AQIs from May 3rd 2003, the day with the most number of reported values. This is the data set we use when training, because it's more natural then the annual averages.
    * <b>\biggest_day_aqi_coords_scaled_noAH.csv</b> - all reported AQIs from May 3rd 2003, but with Alaska and Hawaii removed because they're so far away.
    * <b>\biggest_day_aqi_coords_y_scaled.csv</b> - Same data as biggest_day_aqi_coords_scaled_noAH.csv, but with the AQI values scaled as well. Used to test if scaling the y-data will improve accuracy.
    * <b>\scaler_oX.pkl</b> - The saved scaler from scaling only the x data, the coordinates.
    * <b>\scaler_wY.pkl</b> - The saved scaler from scaling the data including the y data.
* <b>\notebooks\\</b> - Stores the Jupyter Notebooks used to create the preprocessed data, and traing the Gaussian Processes
    * <b>\example_sparsification_prediction.ipynb</b> - implementation of the "Illustrating MuyGPs Sparcification, Prediction, and Uncertainty Quantification" tutorial.
    * <b>\example_univariate.ipynb</b> - Implementation of the "Univariate Regression Tutorial" tutorial.
    * <b>\aqi_preprocessing.ipynb</b> - Handles preprocessing the public aqi datasets to create training datasets. Used to generate the various extra .csv files.
    * <b>\aqi_regression_prekfold.ipynb</b> - the aqi regression before kfold evaluation was implemented. Easier to understand, since it was easier to seperate and label the steps for model creation and use.
    * <b>\aqi_regression.ipynb</b> - Where the training of the AQI Gausian process happens. follows the form of the univariate regression tutorial. Has Kfold evaluation applied. Also has code enabling checking how the model performs with a standardized AQI.
    * <b>\Sklearn_aqi_regression.ipynb</b> - An implementation of the sklearn Gaussian Process, to act as a baseline.
* <b>\README.md</b> - You are here.
* <b>\Results.md</b> - Very simiple file keeping track of changes to the model and the resulting performance. Used when tryomg to optimize the models, to see what the effects of changing various hyperparameters was.
* <b>\requirements.txt</b> - the pip packages you need to install.
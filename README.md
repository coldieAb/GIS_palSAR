PALSAR Data Processing and Soil Moisture Estimation
This Jupyter Notebook (PALSAR.ipynb) provides a workflow for processing ALOS PALSAR (Phased Array type L-band Synthetic Aperture Radar) data to estimate soil moisture using the Oh Model. The notebook demonstrates loading SAR imagery, converting Digital Numbers (DN) to sigma naught (
sigma 
0
 ) in decibels (dB), transforming dB values to a linear scale, calculating the HV/HH ratio (R-value), and applying empirical models to derive soil moisture. Finally, it visualizes the soil moisture map and saves the result as a GeoTIFF file.

Features
Google Drive Integration: Mounts Google Drive to access PALSAR TIFF files.

Raster Data Handling: Utilizes rasterio for efficient reading and writing of GeoTIFF files.

SAR Data Conversion: Converts raw PALSAR Digital Number (DN) values to calibrated sigma naught (
sigma 
0
 ) in dB.

Linear Scale Conversion: Transforms 
sigma 
0
  values from dB to a linear scale for ratio calculations.

R-value Calculation: Computes the ratio of HV to HH polarization (
textR=
sigma 
0
 _textHV/(
sigma 
0
 _textHH+
epsilon)), with a small epsilon to avoid division by zero.

Oh Model Application: Implements the empirical Oh Model (Oh et al. 2004) to estimate volumetric soil moisture (m_v) from the R-value, using adjusted coefficients.

Formula: m_v=a
cdotR 
b
 +c

Adjusted coefficients: a=0.5, b=0.6, c=0.06

Data Clipping: Clips estimated soil moisture values to a realistic range (0 to 0.5 m 
3
 /m 
3
 ).

Visualization: Generates a visually intuitive soil moisture map using matplotlib with a 'YlGnBu' colormap and a custom colorbar.

GeoTIFF Export: Saves the generated soil moisture map as a new GeoTIFF file, preserving the original geospatial metadata.

Requirements
To run this notebook, you will need the following Python libraries:

numpy

rasterio

matplotlib

You can install them using pip:

pip install numpy rasterio matplotlib

Data
This notebook expects ALOS PALSAR data in GeoTIFF format, specifically:

HH-ALPSRPXXXXXXXXX-H2.2_UA.tif (HH polarization)

HV-ALPSRPXXXXXXXXX-H2.2_UA.tif (HV polarization)

INC-ALPSRPXXXXXXXXX-H2.2_UA.tif (Incidence Angle - Note: While loaded, this specific notebook version does not directly use INC in the Oh model calculation, but it's good practice to load it if available for other analyses.)

These files are assumed to be located in your Google Drive at the path specified in the notebook: /content/drive/MyDrive/new PALSAR WAYANAD/ALPSRP084410210-H2.2_UA/. Please adjust the hh_path, hv_path, and inc_path variables if your data is stored elsewhere.

Usage
Open the Notebook: Upload and open PALSAR.ipynb in Google Colab or any Jupyter environment.

Mount Google Drive: Run the first cell to mount your Google Drive. This is necessary to access the input TIFF files.

Install Libraries: Ensure all required libraries are installed by running the !pip install commands if prompted or if you are in a new environment.

Load Data: Execute the cells to load the HH, HV, and INC bands.

Data Preprocessing: Run the cells that perform DN to sigma naught conversion and handle zero values by masking them as NaN.

Calculate Soil Moisture: Execute the cells that compute the R-value and apply the Oh Model to estimate soil moisture.

Visualize and Save: The final cells will display the soil moisture map and save it as oh_model_soil_moisture.tif in the same directory as the input files (or the current working directory if paths are relative).

Notes
The Oh Model coefficients (a,b,c) used in this notebook are adjusted and may need further calibration for specific study areas or sensor configurations.

The notebook handles RuntimeWarning: divide by zero encountered in log10 by masking zero values in the input bands, ensuring robust calculations.

CRS deprecation warnings from rasterio are noted but do not affect the core soil moisture calculation in this context.

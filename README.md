# Landsat-8 Land Surface Temperature downscaling using Sentinel-2 in Google Earth Engine
This code repository is an attachment for the article in Remote Sensing: Onačillová, K., Gallay, M., Paluba, D., Péliová, A., Tokarčík, O., Laubertová, D. 2022: Combining Landsat 8 and Sentinel 2 data in Google Earth Engine to derive higher resolution land surface temperature maps in urban environment. Remote Sensing. The repository contains a folder "javascript_codes" where you can find:
  - A JavaScript Google Earth Engine (GEE) code "LST_downscaling_GEE_APP.js" used in the [LST-downscaling GEE Application](https://danielp.users.earthengine.app/view/l8lstto10mlst) to downscale (sharpen) the Land Surface Temperature (LST) derived from Landsat 8 thermal sensing using the spectral bands of Sentinel-2 
  
   ## How to use the [LST-downscaling GEE Application](https://danielp.users.earthengine.app/view/l8lstto10mlst)
1. Define the 5 input parameters: 
    - start and end date (to select the desired time frame), 
    - Landsat collection (to select whether to use the Landsat 8 or Landsat 9 image collection), 
    - maximum cloud cover allowed for the image tiles and 
    - the region of interest (ROI). 
      - <sub>The default ROI set-ting is focused on Košice City. However, the user can change the location where the analysis will be performed. The ROI is created using the Geometry Tools in the GEE, which allow users to move or delete and then delineate their own new geometric fea-tures, such as polygons, to be applied in the analyses. </sub>

2. Click on **“Generate Landsat 8/9 and Sentinel-2 Image Collections”** button to generate available image IDs. 
    - Based on this information, the list of Landsat 8/9 and Sentinel-2 imagery IDs that meet the entry criteria will be displayed under the button in the right panel. 
    - The user will recieve a list of Landsat 8/9 and Sentinel-2 Image IDs that meet the selected criteria.
3. Enter Image IDs for the Landsat and for the Sentinel-2 Collection. 
    - The user can select two image IDs from the resulting list – one ID for the Landsat 8/9 collection and one for the Sentinel-2 collection and enter their exact ID to the newly displayed text fields. Several Landsat 8/9 and Sentinel-2 imagery may be available in a given time window, therefore we recommend selecting data sets that were acquired on the same day. If images for Landsat 8/9 and Sentinel-2 collections are not available on the same acquisition day, we recommend choosing the datasets by the closest acquisition time to account for similar spectral characteristics of the derived spectral indices from both satellites. 
4. Click on **"Generate Downscaled LST"** button to perform the Landsat 8/9 LST Downscaling to 10 m spatial resolution.
**Note:** All input parameters, including image IDs, are pre-filled with parameters required to perform analysis showed in this paper. Without modifying the input parameters, the user gets the (slightly different) results produced in this article.
    - Tthe following 5 images: Landsat 8/9 and Sentinel-2 natural color images (RGB), original Landsat 8/9 LST in 30 m, downscaled LST to 10 m spatial resolution with and without assuming residuals are added to the Map.
5. (Optional) Click on **"Generate charts of spectral indices vs Landsat LST"** to generate scatterplots of correlation between Landsat 8/9 NDVI, NDWI and NDBI spectral indices and Landsat 8/9 LST bands.

  ## About the Landsat-8 Land Surface Temperature downscaling using Sentinel-2 in GEE
  This algorithm aims to downscale the coarse spatial resolution (100/30 m) Landsat 8 LST to finer spatial resolution (10 m) for more accurate mapping of LST. Three different indices, namely the normalized difference vegetation index (NDVI), built-up index (NDBI), and water index (NDWI) were used for disaggregation of Landsat LST (100/30 m) to 10 m Sentinel-2 spatial resolution using linear regression. The algorithm was developed in the cloud-based platform Google Earth Engine (GEE) using Landsat-8 and Sentinel-2 open access data. We conclude that the proposed downscaling model, by addressing the linear relationship of LST at coarse and fine spatial resolutions, can be successfully applied to produce high-resolution LST maps suitable for studies of the urban thermal environment at local scales. The performance was validated by the adjusted R2 returned at the stage of model development as well as by the validation of downscaled LST results using data logger measurements at 6 sites in the Košice city, Slovakia, to represent different types of land cover.
  
  ## Methodology:
  First, the region of interest (“ROI”) is defined to process the date range and maximum cloud cover for the Landsat 8 and Sentinel-2 satellite imagery. The ROI is created using the Geometry Tools in the GEE to allow users for delineating their own geometric features, such as polygons, to be applied in the analyses. (Note: The default setting of the GEE tool is focused on Košice City, Slovakia).
</b>Based on this information, the main module loads the respective images of Landsat Surface Reflectance (SR) and Sentinel-2 Level 2A (L2A) collections that meet the input criteria. The users then select two images from the resulting list (visible in the Console) – one image from the Landsat 8 SR collection and one from the Sentinel-2 L2A collection and enter their exact ID. We recommend choosing datasets, which were acquired on the same day. These images are used to compute spectral indices NDVI, NDBI, and NDWI for Landsat 8 in 30 m resolution (NDVIL8, NDBIL8, NDWIL8) and Sentinel-2 in 10 m resolution, respectively. The NDVI L8 is also converted to Fractional Vegetation (FV) values, which are then used to derive emissivity values.
</b>The LST is then calculated at 30 m resolution using the Landsat 8 thermal band (B10) converted to brightness temperature in degrees Celsius (LSTL8). Then, the linear regression model between LSTL8 and the spectral indices NDVIL8, NDBIL8, NDWIL8 is used to calculate regression coefficients. Finally, these regression coefficients are used to calculate the downscaled LST using Sentinel-2 spectral indices NDVI, NDBI, and NDWI in 10 m resolution. Regression residuals are resampled and added back to the downscaled LST. Also, Landsat 8 and Sentinel-2 natural color images (RGB) are generated to compare with the final LST layers.

## Outputs of the algorithm
There are three different output types of the algorithm: (1) the main output is the downscaled LST 10 m with residuals, (2) bivariate scatter plots of LSTL8 vs. NDVIL8, NDBIL8, NDWIL8, (3) downscaling regression model. In the GEE code, the user will obtain the following outputs:
  - LST 10 m with residuals - final downscaled LST with regression residuals in 10 m spatial resolution
  - LST 10 m (no residuals) - downscaled LST in 10 m spatial resolution (without added regression residuals)
  - Landsat 8 LST – Landsat 8 Land Surface Temperature in 30 m spatial resolution
  - Landsat 8 LST regression residuals – regression residuals of LST computed as the difference between model estimation and the corresponding observation by Landsat 8 at 30 m resolution 
  - Sentinel-2 RGB – true color composite for Sentinel-2
  - Landsat 8 RGB –  true color composite for Landsat 8

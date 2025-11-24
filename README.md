# ReadMe (Updated; 2025.11.24.)
# UGCI_raw_files

These are the raw files used to produce Figures 1 to 4.
In particular, for Figure 2, sharing the raw file of the updated land-cover map is discouraged due to potential data sensitivity.
Please use these raw files only for reviewing the corresponding raw images.
A publicly shareable version of the land-cover map has been included in the “Calculating UGCI” directory together with the code.

# data sources and method used to create the maps
This document clarifies the data sources and processing steps used to create Figures 1-4.
As Figure 1 directly uses the raster outputs shown in Figure 4, please take a look at the explanation under Figure 4.
Detailed methodologies can be found in Section 2 of the main manuscript.

[Figure 2]
The original vector datasets used for Figure 2 include:
* 2020 Subdivision Land Cover Map provided by the Korea Ministry of Climate, Energy, and Environment
* 2023 Spatial Facilities Map provided by the Korea Ministry of Land, Infrastructure, and Transport.
Both datasets are freely accessible on the Korean government portals, although user login is required:
* Environmental Spatial Information Service: https://aid.mcee.go.kr/req/intro.do
* Vworld Platform: https://www.vworld.kr/dtmk/dtmk_ntads_s002.do?dsld=30068&svcCde=MK

The subdivision land cover vector map was rasterized to a 1 m resolution. For each 1 m grid cell, the land-cover class occupying the largest area within that cell was assigned.
The original 41 detailed classes were reclassified into the following consolidated categories based on the large classification system: broadleaf forest, coniferous forest, mixed forest, agricultural area, wetlands, grassland, bare land, impervious area, rivers/lakes, and others.
Using airborne LiDAR and orthophotos, a U-Net model was developed to classify tree and shrub cover at a 0.25 m resolution.
When tree/shrub grid cells overlapped with forest classes from the land cover raster, they were reassigned to the corresponding forest category.
The remaining tree/shrub cells were classified as roadside vegetation.
Urban park polygons extracted from the Vworld spatial facilities map were rasterized in the same manner. Wherever urban park overlapped with forest, wetland, or grassland cells, the grid cell was reassigned to urban park.
Finally, only green-space categories (forest, agricultural area, urban parks, roadside vegetation) were retained as the area of interest. Non-green classes (impervious area, bare land, rivers/lakes, and others) were excluded.
The remaining 1 m raster for green spaces was aggregated to 30 m resolution using the majority rule and nearest-neighbor interpolation.

[Figure 3]
For the area of interest, we generated four carbon-related raster layers: vegetation C storage, soil C storage, net C uptake, and soil C storage potential.
* Vegetation C storage was estimated separately for roadside vegetation (using a machine-learning model based on relative growth functions with bias correction) and other land-cover types (using Korean land-cover-specific coefficients and area).
* Soil C storage was derived from GSOCmap (1 km resolution, https://data.apps.fao.org/glosis/?share=f-6756da2a-5c1d-4ac9-9b94-297d1f105e83&lang=en) and downscaled to 30 m using bilinear interpolation.
* Net C uptake was estimated from 250-m hourly gross primary productivity predicted by a light-use-efficiency random forest model and hourly ecosystem respiration predicted by an empirical model. It was also downscaled to 30 m via bilinear interpolation.
* Soil C storage potential was estimated 30-m silt and clay content produced using a light-gradient-boosting model trained by 225 legacy soil samples with land cover, terrain, and climate predictors.

[Figure 4]
Urban Green Carbon Index (UGCI) was computed using the datasets described in Figure 3.
UGCI is a weighted average of normalized carbon indicators, where each dataset's spatial variability was used as a weight.
The final UGCI raster was categorized into quartiles: Extremely low, Low, Moderate, and High.
Using the 30-m land-cover raster produced in Figure 2, each grid cell was classified into forest, agricultural area, urban park, and roadside vegetation, and visualized with distinct color schemes.

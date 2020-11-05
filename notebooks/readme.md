# Demonstration Notebooks

We demonstrate the RABASAR multitemporal denoising on ALOS-1 and UAVSAR Images.

## Outline

+ Download data according using instructions
+ Update `config.json`.
+ Run through notebooks 0 - 4.

### UAVSAR at Waxlake.

1. Follow the directions in the [uavsar_waxlake/readme.md](uavsar_waxlake/readme.md). Once completed, there should be `uavsar_waxlake/data_original_tiff` with the RTC images from UAVSAR. Check with QGIS or another GIS viewer.

2. Make sure `config.json` is properly configured.

3. Run the notebooks in order.

4. Inspect the products in `out/uavsar_waxlake_tv/` or `out/uavsar_waxlake_bm3d/` depending on the regularizer.

Note this could in theory be adapted at other sites that have the newly added `*.rtc` file. However, note we had to manually download numerous files and organize them. This was quite time-intensive.

### ALOS-1 at Waxlake.

1. Follow the directions in the [alos1_waxlake](alos1_waxlake/readme.md). Once completed, there should be `alos1_waxlake/data_original_tiff` with the RTC images from UAVSAR. Check with QGIS or another GIS viewer.

2. Make sure `config.json` is properly configured as below.

3. Run the notebooks in order.

4. Inspect the products in `out/alos1_waxlake_tv/` or `out/alos1_waxlake_bm3d/` depending on the regularizer.

Note this could more easily reproduced (relative to UAVSAR) because of the [asf search tool](https://search.asf.alaska.edu/) which produces a python script for downloading time series determined via a GUI.

### Example Config Files

#### TV

+ ALOS-1 @ the Waxlake
  
  ```
  {
    "sensor": "alos1",
    "site": "waxlake",
    "regularizer": "tv",
    "spatial_weight": 1.0,
    "temporal_average_spatial_weight": 1.0,
    "ratio_weight": 1.0
  }
  ```

+ UAVSAR @ the Waxlake

  ```
  {
    "sensor": "alos1",
    "site": "waxlake",
    "regularizer": "tv",
    "spatial_weight": 1.0,
    "temporal_average_spatial_weight": 1.0,
    "ratio_weight": 1.0
  }
  ```

#### BM3D

+ ALOS-1 @ the Waxlake
  
  ```
  {
    "sensor": "alos1",
    "site": "waxlake",
    "regularizer": "bm3d",
    "spatial_weight": 0.05,
    "temporal_average_spatial_weight": 0.005,
    "ratio_weight": 0.05
  }
  ```

+ UAVSAR @ the Waxlake

  ```
  {
    "sensor": "alos1",
    "site": "waxlake",
    "regularizer": "bm3d",
    "spatial_weight": 0.05,
    "temporal_average_spatial_weight": 0.005,
    "ratio_weight": 0.05
  }
  ```


### Remarks about BM3D 

The `bm3d` regularizer is quite complex and thus required many more computational resources. In the notebooks, we only apply the RABASAR with the `bm3d` regularizer to `1000 x 1000` box otherwise such an application would likely require days to run in the current implementation. Moreover, we also note the weights across the different despeckling is also different. This is in part due to the implementation of the `bm3d`, but also because it is clearly much more sensitive to smoothness in the original image than `tv`. As such, the weight for the ratio image is different than that used for the temporally averaged reference.